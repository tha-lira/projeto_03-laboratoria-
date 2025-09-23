## Estrutura da Documentação Técnica — Análise de Score de Risco

### 📊 Aplicar técnica de análise

####  Calcular risco relativo

✅ 1. Risco relativo por quartis (age)

```
WITH clientes_quartis AS (
  SELECT
    *,
    NTILE(4) OVER (ORDER BY age) AS quartil
  FROM `projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
),

inadimplencia_por_quartil AS (
  SELECT
    quartil,
    COUNTIF(default_flag = 1) AS inadimplentes,
    COUNTIF(default_flag = 0) AS nao_inadimplentes,
    COUNT(*) AS total,
    ROUND(SAFE_DIVIDE(COUNTIF(default_flag = 1), COUNT(*)) * 100, 2) AS pct_inadimplencia
  FROM clientes_quartis
  GROUP BY quartil
),

referencia AS (
  SELECT pct_inadimplencia AS taxa_ref
  FROM inadimplencia_por_quartil
  WHERE quartil = 1
)

SELECT
  ROW_NUMBER() OVER (ORDER BY iq.quartil) AS linha,
  iq.quartil,
  iq.inadimplentes,
  iq.nao_inadimplentes,
  iq.total,
  iq.pct_inadimplencia,
  ROUND(SAFE_DIVIDE(iq.pct_inadimplencia, r.taxa_ref), 2) AS risco_relativo
FROM inadimplencia_por_quartil iq
CROSS JOIN referencia r
ORDER BY iq.quartil;
```

####  Aplicar segmentação por Score

✅ 1. Construção do Score de Risco

Criação da Tabela com Score

```
CREATE OR REPLACE TABLE `projeto-risco-relativo-470919.bancoSuperCaja.base_score_clientes` AS
SELECT
  *,
  -- Dummies por perfil de risco
  CASE WHEN age < 25 THEN 1 ELSE 0 END AS risco_idade_baixa,
  CASE WHEN salary_last_month < 2000 THEN 1 ELSE 0 END AS risco_salario_baixo,
  CASE WHEN dependents >= 3 THEN 1 ELSE 0 END AS risco_muitos_dependentes,
  CASE WHEN loan_count >= 4 THEN 1 ELSE 0 END AS risco_muitos_emprestimos,
  CASE WHEN has_real_estate_loan = 1 THEN 0 ELSE 1 END AS risco_sem_real_estate,
  CASE WHEN has_other_loan = 1 THEN 1 ELSE 0 END AS risco_possui_emprestimo_outro,
  CASE WHEN overdue_90_days = 1 THEN 1 ELSE 0 END AS risco_historico_atraso,
  CASE WHEN unsecured_credit_lines > 0.5 THEN 1 ELSE 0 END AS risco_credito_nao_garantido,
  CASE WHEN debt_ratio > 0.4 THEN 1 ELSE 0 END AS risco_alta_divida,

  -- Score total
  (
    (CASE WHEN age < 25 THEN 1 ELSE 0 END) +
    (CASE WHEN salary_last_month < 2000 THEN 1 ELSE 0 END) +
    (CASE WHEN dependents >= 3 THEN 1 ELSE 0 END) +
    (CASE WHEN loan_count >= 4 THEN 1 ELSE 0 END) +
    (CASE WHEN has_real_estate_loan = 1 THEN 0 ELSE 1 END) +
    (CASE WHEN has_other_loan = 1 THEN 1 ELSE 0 END) +
    (CASE WHEN overdue_90_days = 1 THEN 2 ELSE 0 END) +
    (CASE WHEN unsecured_credit_lines > 0.5 THEN 1 ELSE 0 END) +
    (CASE WHEN debt_ratio > 0.4 THEN 1 ELSE 0 END)
  ) AS score_risco

FROM
  `projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`;
```

✅ 2. Distribuição do Score

```
SELECT score_risco, COUNT(*) AS total
FROM `projeto-risco-relativo-470919.bancoSuperCaja.base_score_clientes`
GROUP BY score_risco
ORDER BY score_risco;
```

✅ 3. Corte Binário: Bom vs Mau Pagador

```
SELECT
  default_flag,
  CASE WHEN score_risco >= 5 THEN 1 ELSE 0 END AS classificacao_score,
  COUNT(*) AS total
FROM `projeto-risco-relativo-470919.bancoSuperCaja.base_score_clientes`
GROUP BY default_flag, classificacao_score
ORDER BY default_flag, classificacao_score;
```

✅ 4. Matriz de Confusão (Score ≥ 5)

Default = 1
Default = 0
Score ≥ 5 (Positivo)
587 (TP)
2.425 (FP)
Score < 5 (Negativo)
96 (FN)
32.892 (TN)

✅ 5. Métricas de Classificação (Accuracy, Precision, Recall, F1)

```
SELECT
  default_flag,
  CASE WHEN score_risco >= 5 THEN 1 ELSE 0 END AS classificacao_score,
  COUNT(*) AS total
FROM `projeto-risco-relativo-470919.bancoSuperCaja.base_score_clientes`
GROUP BY default_flag, classificacao_score
ORDER BY default_flag, classificacao_score;
```

✅ 6. Avaliação de Pontos de Corte

```
SELECT
  corte,
  SUM(CASE WHEN score_risco >= corte AND default_flag = 1 THEN 1 ELSE 0 END) AS tp,
  SUM(CASE WHEN score_risco >= corte AND default_flag = 0 THEN 1 ELSE 0 END) AS fp,
  SUM(CASE WHEN score_risco < corte AND default_flag = 1 THEN 1 ELSE 0 END) AS fn,
  SUM(CASE WHEN score_risco < corte AND default_flag = 0 THEN 1 ELSE 0 END) AS tn,
  COUNT(*) AS total_clientes,
  SAFE_DIVIDE(tp + tn, COUNT(*)) AS accuracy,
  SAFE_DIVIDE(tp, tp + fp) AS precision,
  SAFE_DIVIDE(tp, tp + fn) AS recall,
  SAFE_DIVIDE(2 * SAFE_DIVIDE(tp, tp + fp) * SAFE_DIVIDE(tp, tp + fn),
              SAFE_DIVIDE(tp, tp + fp) + SAFE_DIVIDE(tp, tp + fn)) AS f1_score
FROM (
  SELECT
    score_risco,
    default_flag,
    corte,
    COUNT(*) OVER() AS total_clientes
  FROM `projeto-risco-relativo-470919.bancoSuperCaja.base_score_clientes`,
    UNNEST(GENERATE_ARRAY(0, 9)) AS corte
)
GROUP BY corte
ORDER BY corte;
```

✅ 7. Faixas de Risco (Segmentação)

```
SELECT
  CASE
  WHEN score_risco <= 2 THEN "1 - Baixo"
  WHEN score_risco = 3 THEN "2 - Medio"
  WHEN score_risco = 4 THEN "3 - Moderado"
  WHEN score_risco >= 5 THEN "4 - Alto"
  ELSE "5 - Inválido"
  END AS faixa_risco,
  COUNT(*) AS total_clientes,
  SUM(CASE WHEN default_flag = 1 THEN 1 ELSE 0 END) AS total_inadimplentes,
  SAFE_DIVIDE(SUM(CASE WHEN default_flag = 1 THEN 1 ELSE 0 END), COUNT(*)) AS inadimplencia_pct
FROM `projeto-risco-relativo-470919.bancoSuperCaja.base_score_clientes`
GROUP BY faixa_risco
ORDER BY faixa_risco;
```

