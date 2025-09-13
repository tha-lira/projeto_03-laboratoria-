# 📊 Conceitos Estatísticos: Risco Relativo, Segmentação por Score e Regressão Logística

---

## 🔹 1. Risco Relativo (RR)

### ✅ O que é?

O **risco relativo** compara a **probabilidade de um evento acontecer em dois grupos diferentes**. Muito usado em estudos de epidemiologia, crédito e fraude.

### ✅ Fórmula:

RR = P(evento | grupo 1) / P(evento | grupo 2)

### ✅ Exemplo:

Suponha que estamos avaliando dois grupos de clientes:

- **Grupo 1**: Clientes com score baixo  
- **Grupo 2**: Clientes com score alto

| Grupo        | Inadimplentes | Total | Probabilidade |
|--------------|---------------|-------|----------------|
| Score Baixo  | 30            | 100   | 30%           |
| Score Alto   | 10            | 100   | 10%           |

RR = 0,30 / 0,10 = 3,0

**Interpretação:** clientes com score baixo têm **3x mais risco de inadimplência** comparado aos com score alto.

---

## 🔹 2. Segmentação por Score

### ✅ O que é?

É a divisão de uma população em **grupos (ou segmentos)** com base em uma **pontuação preditiva (score)**.

Aplicações:
- **Crédito:** Score de risco de inadimplência
- **Marketing:** Score de propensão à compra
- **Saúde:** Score de risco de doença

### ✅ Como fazer?

1. **Construir um score preditivo** (ex: com regressão logística)
2. **Ordenar a base** de acordo com esse score
3. **Dividir em grupos (faixas, decis, quintis, etc.)**

### ✅ Exemplo:

Você pode dividir os clientes em 5 grupos de score (quintis):

| Faixa de Score | % Inadimplência |
|----------------|------------------|
| 0–200          | 35%             |
| 201–400        | 25%             |
| 401–600        | 15%             |
| 601–800        | 8%              |
| 801–1000       | 2%              |

**Interpretação:** à medida que o score aumenta, o risco diminui.

---

## 🔹 3. Regressão Logística

### ✅ O que é?

É um modelo estatístico usado para prever **probabilidades de eventos binários** (ex: "vai pagar" ou "não vai pagar").

### ✅ Fórmula:

A saída da regressão logística é a **probabilidade de ocorrência do evento**:

P(y = 1) = 1 / [1 + exp(-(b0 + b1*x1 + b2*x2 + ... + bn*xn))]

Onde:

- P(y = 1) é a probabilidade do evento ocorrer (por exemplo, inadimplência)

- b0 é o intercepto

- b1, b2, ..., bn são os coeficientes do modelo

- x1, x2, ..., xn são as variáveis independentes

### ✅ Exemplo:

Modelo hipotético:

P(inadimplência) = 1 / [1 + exp(-(-2 + 0.01*renda - 0.5*idade))]


Para um cliente com:
- **renda = 3000**
- **idade = 30**

Substituindo:

P = 1 / [1 + exp(-(-2 + 0.01*3000 - 0.5*30))]
P = 1 / [1 + exp(-13)] ≈ 0.9999977


**Resultado:** altíssima chance de inadimplência.

### ✅ Como calcular na prática?

Usando Python com `scikit-learn`:

```
python
from sklearn.linear_model import LogisticRegression
model = LogisticRegression()
model.fit(X, y)  # X = variáveis preditoras, y = 0 ou 1 (ex: inadimplente ou não)
```

---

✅ Resumo:

| Conceito                  | Para que serve                          | Como calcular                                               |
| ------------------------- | --------------------------------------- | ----------------------------------------------------------- |
| **Risco Relativo (RR)**   | Comparar riscos entre dois grupos       | $RR = P1 / P2$                                              |
| **Segmentação por Score** | Dividir população por faixa de risco    | Criar score, ordenar e segmentar em grupos (quintis, decis) |
| **Regressão Logística**   | Estimar probabilidade de evento binário | Fórmula logística / modelo com Python, R ou Excel           |










criei uma tabela


CREATE OR REPLACE TABLE
  `projeto-risco-relativo-470919.bancoSuperCaja.base_score_clientes` AS
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



SELECT score_risco, COUNT(*) AS total
  FROM `projeto-risco-relativo-470919.bancoSuperCaja.base_score_clientes`
GROUP BY score_risco
ORDER BY score_risco;

  Linha	score_risco	total
1	0	1
2	1	302
3	2	9247
4	3	15619
5	4	7819
6	5	1934
7	6	879
8	7	174
9	8	24
10	9	1

SELECT
  default_flag,
  CASE WHEN score_risco >= 5 THEN 1 ELSE 0 END AS classificacao_score,
  COUNT(*) AS total
FROM `projeto-risco-relativo-470919.bancoSuperCaja.base_score_clientes`
GROUP BY default_flag, classificacao_score
ORDER BY default_flag, classificacao_score;

Linha	default_flag	classificacao_score	total
1	0	0	32892
2	0	1	2425
3	1	0	96
4	1	1	587

SELECT CASE WHEN score_risco >= 5 THEN 1 ELSE 0 END AS classificacao_score, default_flag, COUNT(*) AS total FROM seu_projeto.seu_dataset.base_score_clientes GROUP BY classificacao_score, default_flag ORDER BY classificacao_score, default_flag; Linha classificacao_score default_flag total 1 0 0 32892 2 0 1 96 3 1 0 2425 4 1 1 587 SELECT score_risco, COUNT(*) AS total_clientes, SUM(CASE WHEN default_flag = 1 THEN 1 ELSE 0 END) AS total_inadimplentes, SUM(CASE WHEN score_risco >= 5 AND default_flag = 1 THEN 1 ELSE 0 END) AS tp, -- verdadeiros positivos para corte 5 SUM(CASE WHEN score_risco >= 5 AND default_flag = 0 THEN 1 ELSE 0 END) AS fp, -- falsos positivos para corte 5 SUM(CASE WHEN score_risco < 5 AND default_flag = 1 THEN 1 ELSE 0 END) AS fn, -- falsos negativos para corte 5 SUM(CASE WHEN score_risco < 5 AND default_flag = 0 THEN 1 ELSE 0 END) AS tn -- verdadeiros negativos para corte 5 FROM projeto-risco-relativo-470919.bancoSuperCaja.base_score_clientes GROUP BY score_risco ORDER BY score_risco; Linha score_risco total_clientes total_inadimplentes tp fp fn tn 1 0 1 0 0 0 0 1 2 1 302 0 0 0 0 302 3 2 9247 1 0 0 1 9246 4 3 15619 6 0 0 6 15613 5 4 7819 89 0 0 89 7730 6 5 1934 185 185 1749 0 0 7 6 879 304 304 575 0 0 8 7 174 87 87 87 0 0 9 8 24 11 11 13 0 0 10 9 1 0 0 1 0 0


verificar cortes 

SELECT
  corte,
  SUM(CASE WHEN score_risco >= corte AND default_flag = 1 THEN 1 ELSE 0 END) AS tp,
  SUM(CASE WHEN score_risco >= corte AND default_flag = 0 THEN 1 ELSE 0 END) AS fp,
  SUM(CASE WHEN score_risco < corte AND default_flag = 1 THEN 1 ELSE 0 END) AS fn,
  SUM(CASE WHEN score_risco < corte AND default_flag = 0 THEN 1 ELSE 0 END) AS tn,
  COUNT(*) AS total_clientes
FROM
  `projeto-risco-relativo-470919.bancoSuperCaja.base_score_clientes`,
  UNNEST(GENERATE_ARRAY(0, 9)) AS corte
GROUP BY
  corte
ORDER BY
  corte;



Linha	corte	tp	fp	fn	tn	total_clientes
1	0	683	35317	0	0	36000
2	1	683	35316	0	1	36000
3	2	683	35014	0	303	36000
4	3	682	25768	1	9549	36000
5	4	676	10155	7	25162	36000
6	5	587	2425	96	32892	36000
7	6	402	676	281	34641	36000
8	7	98	101	585	35216	36000
9	8	11	14	672	35303	36000
10	9	0	1	683	35316	36000


WITH base AS (
  SELECT
    score_risco,
    default_flag
  FROM
    `projeto-risco-relativo-470919.bancoSuperCaja.base_score_clientes`
),
total AS (
  SELECT COUNT(*) AS total_clientes FROM base
),
metrics AS (
  SELECT
    corte,
    SUM(CASE WHEN score_risco >= corte AND default_flag = 1 THEN 1 ELSE 0 END) AS tp,
    SUM(CASE WHEN score_risco >= corte AND default_flag = 0 THEN 1 ELSE 0 END) AS fp,
    SUM(CASE WHEN score_risco < corte AND default_flag = 1 THEN 1 ELSE 0 END) AS fn,
    SUM(CASE WHEN score_risco < corte AND default_flag = 0 THEN 1 ELSE 0 END) AS tn
  FROM
    base,
    UNNEST(GENERATE_ARRAY(0, 9)) AS corte
  GROUP BY corte
)

SELECT
  m.corte,
  m.tp,
  m.fp,
  m.fn,
  m.tn,
  t.total_clientes,
  SAFE_DIVIDE(CAST(m.tp + m.tn AS FLOAT64), t.total_clientes) AS accuracy,
  SAFE_DIVIDE(m.tp, m.tp + m.fp) AS precision,
  SAFE_DIVIDE(m.tp, m.tp + m.fn) AS recall,
  SAFE_DIVIDE(2 * SAFE_DIVIDE(m.tp, m.tp + m.fp) * SAFE_DIVIDE(m.tp, m.tp + m.fn),
              SAFE_DIVIDE(m.tp, m.tp + m.fp) + SAFE_DIVIDE(m.tp, m.tp + m.fn)) AS f1_score
FROM
  metrics m,
  total t
ORDER BY
  m.corte;

  Linha	corte	tp	fp	fn	tn	total_clientes	accuracy	precision	recall	f1_score
1	0	683	35317	0	0	36000	0.018972222222222224	0.018972222222222224	1.0	0.037237957637052593
2	1	683	35316	0	1	36000	0.019	0.018972749243034527	1.0	0.037238972793195574
3	2	683	35014	0	303	36000	0.02738888888888889	0.019133260498081072	1.0	0.03754810335349093
4	3	682	25768	1	9549	36000	0.28419444444444447	0.025784499054820414	0.99853587115666176	0.050270887848745061
5	4	676	10155	7	25162	36000	0.71772222222222226	0.062413442895392857	0.98975109809663253	0.11742226854264375
6	5	587	2425	96	32892	36000	0.9299722222222222	0.1948871181938911	0.85944363103953147	0.31772665764546681
7	6	402	676	281	34641	36000	0.97341666666666671	0.37291280148423006	0.58857979502196189	0.45655877342419077
8	7	98	101	585	35216	36000	0.9809444444444444	0.49246231155778897	0.14348462664714495	0.22222222222222227
9	8	11	14	672	35303	36000	0.9809444444444444	0.44	0.016105417276720352	0.031073446327683614
10	9	0	1	683	35316	36000	0.981	0.0	0.0





vou fazer Corte binário (bom/mau), Faixas de risco (segmentos), Validação com matriz, Faixas de risco (segmentos)






SELECT
  faixa_risco,
  COUNT(*) AS total_clientes,
  SUM(default_flag) AS total_inadimplentes,
  SAFE_DIVIDE(SUM(default_flag), COUNT(*)) AS inadimplencia_pct
FROM (
  SELECT
    *,
    CASE
      WHEN score_risco BETWEEN 0 AND 3 THEN 'Risco Baixo'
      WHEN score_risco BETWEEN 4 AND 6 THEN 'Risco Médio'
      WHEN score_risco >= 7 THEN 'Risco Alto'
    END AS faixa_risco
  FROM `projeto-risco-relativo-470919.bancoSuperCaja.base_score_clientes`
)
GROUP BY faixa_risco
ORDER BY faixa_risco;

Linha	faixa_risco	total_clientes	total_inadimplentes	inadimplencia_pct
1	Risco Alto	199	98	0.49246231155778897
2	Risco Baixo	25169	7	0.00027811990941237235
3	Risco Médio	10632	578	0.054364183596689243