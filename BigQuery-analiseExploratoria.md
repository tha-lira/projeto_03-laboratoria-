## Estrutura da Documentação Técnica — Análise Exploratória

### 📊 Fazer uma análise exploratória

#### Agrupar dados de acordo com variáveis ​​categóricas

✅ 1. Empréstimo Imobiliário (has_real_estate_loan)

```
SELECT
has_real_estate_loan,
COUNT(user_id) AS total_clientes,
ROUND(AVG(default_flag) * 100, 2) AS taxa_inadimplencia_percentual,
ROUND(AVG(salary_last_month), 2) AS salario_medio,
ROUND(AVG(debt_ratio), 3) AS debt_ratio_medio
FROM
`projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
GROUP BY
has_real_estate_loan
ORDER BY
has_real_estate_loan;
```

✅ 2. Outros Empréstimos (has_other_loan)

```
SELECT
has_other_loan,
COUNT(user_id) AS total_clientes,
ROUND(AVG(default_flag) * 100, 2) AS taxa_inadimplencia_percentual,
ROUND(AVG(salary_last_month), 2) AS salario_medio,
ROUND(AVG(debt_ratio), 3) AS debt_ratio_medio
FROM
`projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
GROUP BY
has_other_loan
ORDER BY
has_other_loan;
```

✅ 3. Número de Dependentes (dependents)

```
SELECT
dependents,
COUNT(user_id) AS total_clientes,
ROUND(AVG(default_flag) * 100, 2) AS taxa_inadimplencia_percentual,
ROUND(AVG(salary_last_month), 2) AS salario_medio,
ROUND(AVG(debt_ratio), 3) AS debt_ratio_medio
FROM
`projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
GROUP BY
dependents
ORDER BY
dependents;
```

✅ 4. Quantidade de Empréstimos (loan_count)

```
SELECT
loan_count,
COUNT(user_id) AS total_clientes,
ROUND(AVG(default_flag) * 100, 2) AS taxa_inadimplencia_percentual,
ROUND(AVG(salary_last_month), 2) AS salario_medio,
ROUND(AVG(debt_ratio), 3) AS debt_ratio_medio
FROM
`projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
GROUP BY
loan_count
ORDER BY
loan_count;
```

✅ 5. Faixas de Quantidade de Empréstimos

```
SELECT
CASE
WHEN loan_count = 0 THEN '0 empréstimos'
WHEN loan_count = 1 THEN '1 empréstimo'
WHEN loan_count = 2 THEN '2 empréstimos'
ELSE '3 ou mais empréstimos'
END AS faixa_emprestimos,
COUNT(user_id) AS total_clientes,
ROUND(AVG(default_flag) * 100, 2) AS taxa_inadimplencia_percentual,
ROUND(AVG(salary_last_month), 2) AS salario_medio,
ROUND(AVG(debt_ratio), 3) AS debt_ratio_medio
FROM
`projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
GROUP BY
faixa_emprestimos
ORDER BY
faixa_emprestimos;
```

####  Ver variáveis ​​categóricas

✅ 1. Inadimplência (%) por Empréstimo Imobiliário

```
SELECT
CASE 
WHEN has_real_estate_loan = 1 THEN 'Sim'
ELSE 'Não'
END AS possui_emprestimo_imobiliario,
ROUND(AVG(default_flag) * 100, 2) AS inadimplencia_percentual
FROM
`projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
GROUP BY
possui_emprestimo_imobiliario
ORDER BY
possui_emprestimo_imobiliario;
```

✅ 2. Inadimplência (%) por Outros Empréstimos

```
SELECT
CASE 
WHEN has_other_loan = 1 THEN 'Sim'
ELSE 'Não'
END AS possui_outros_emprestimos,
ROUND(AVG(default_flag) * 100, 2) AS inadimplencia_percentual
FROM
`projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
GROUP BY
possui_outros_emprestimos
ORDER BY
possui_outros_emprestimos;
```

✅ 3. Inadimplência (%) por Número de Dependentes

```
SELECT
dependents AS numero_dependentes,
ROUND(AVG(default_flag) * 100, 2) AS inadimplencia_percentual
FROM
`projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
GROUP BY
numero_dependentes
ORDER BY
numero_dependentes;
```

✅ 4. Inadimplência (%) por Faixa de Empréstimos

```
SELECT
CASE
WHEN loan_count = 0 THEN '0 empréstimos'
WHEN loan_count = 1 THEN '1 empréstimo'
WHEN loan_count = 2 THEN '2 empréstimos'
ELSE '3 ou mais empréstimos'
END AS faixa_emprestimos,
ROUND(AVG(default_flag) * 100, 2) AS inadimplencia_percentual
FROM
`projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
GROUP BY
faixa_emprestimos
ORDER BY
CASE 
WHEN faixa_emprestimos = '0 empréstimos' THEN 1
WHEN faixa_emprestimos = '1 empréstimo' THEN 2
WHEN faixa_emprestimos = '2 empréstimos' THEN 3
ELSE 4
END;
```

####  Aplicar medidas de tendência central (moda, média, mediana)

✅ 1. Média

```
SELECT
AVG(salary_last_month) AS media_salario
FROM
`projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
```

✅ 2. Mediana

```
SELECT
APPROX_QUANTILES(salary_last_month, 2)[OFFSET(1)] AS mediana_salario
FROM
`projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
```

✅ 3. Moda 

```
SELECT
dependents AS moda_dependentes,
COUNT(*) AS freq
FROM
`projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
GROUP BY
dependents
ORDER BY
freq DESC
LIMIT 1
```

####  Ver distribuição

✅ 1. Histograma com faixas fixas (bin de R$1000)

```
SELECT
FLOOR(salary_last_month / 1000) * 1000 AS bin_range,
COUNT(*) AS frequency,
ROUND(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM `...base_unificada`), 2) AS percent_total
FROM
`projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
WHERE
salary_last_month IS NOT NULL
GROUP BY
bin_range
ORDER BY
bin_range;
```

✅ 2. Histograma por Quantis (aproximadamente 10 faixas com clientes equilibrados)

```
WITH boundaries AS (
SELECT
APPROX_QUANTILES(salary_last_month, 10) AS quantiles
FROM
`projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
)
SELECT
CONCAT(
'[',
quantiles[offset(i)],
', ',
quantiles[offset(i+1)],
')'
) AS quantile_range,
COUNT(*) AS clientes
FROM
`projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`,
boundaries,
UNNEST(GENERATE_ARRAY(0, ARRAY_LENGTH(quantiles) - 2)) AS i
WHERE
salary_last_month BETWEEN quantiles[offset(i)] AND quantiles[offset(i+1)]
GROUP BY
quantile_range
ORDER BY
quantile_range;
```

✅ 3. Estatísticas para Boxplot (salário)

```
SELECT
APPROX_QUANTILES(salary_last_month, 4)[OFFSET(0)] AS minimo,
APPROX_QUANTILES(salary_last_month, 4)[OFFSET(1)] AS Q1,
APPROX_QUANTILES(salary_last_month, 4)[OFFSET(2)] AS mediana,
APPROX_QUANTILES(salary_last_month, 4)[OFFSET(3)] AS Q3,
APPROX_QUANTILES(salary_last_month, 4)[OFFSET(4)] AS maximo
FROM
`projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`;
```

✅ 4. Distribuição de Salário por Quantis (8 Faixas)

```
WITH bucketized AS (
SELECT
ML.QUANTILE_BUCKETIZE(salary_last_month, 10, 'bucket_ranges') OVER() AS bucket
FROM
`projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
)
SELECT
bucket,
COUNT(*) AS count_in_bucket
FROM
bucketized
GROUP BY
bucket
ORDER BY
bucket;
```

####  Aplicar medidas de dispersão (desvio padrão)

✅ 1. Desvio Padrão de Salário e Debt Ratio por Empréstimo Imobiliário

```
SELECT
CASE WHEN has_real_estate_loan = 1 THEN 'Sim' ELSE 'Não' END AS possui_emprestimo_imobiliario,
STDDEV(salary_last_month) AS desvio_padrao_salario,
STDDEV(debt_ratio) AS desvio_padrao_debt_ratio
FROM
`projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
GROUP BY
has_real_estate_loan
ORDER BY
possui_emprestimo_imobiliario;
```

✅ 2. Desvio Padrão por Outros Empréstimos

```
SELECT
CASE WHEN has_other_loan = 1 THEN 'Sim' ELSE 'Não' END AS possui_outros_emprestimos,
STDDEV(salary_last_month) AS desvio_padrao_salario,
STDDEV(debt_ratio) AS desvio_padrao_debt_ratio
FROM
`projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
GROUP BY
has_other_loan
ORDER BY
possui_outros_emprestimos;
```

✅ 3. Desvio Padrão por Número de Dependentes

```
SELECT
dependents AS numero_dependentes,
STDDEV(salary_last_month) AS desvio_padrao_salario,
STDDEV(debt_ratio) AS desvio_padrao_debt_ratio
FROM
`projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
GROUP BY
numero_dependentes
ORDER BY
numero_dependentes;
```

✅ 4. Desvio Padrão por Faixa de Empréstimos (loan_count agrupado)

```
SELECT
CASE
WHEN loan_count = 0 THEN '0 empréstimos'
WHEN loan_count = 1 THEN '1 empréstimo'
WHEN loan_count = 2 THEN '2 empréstimos'
ELSE '3 ou mais empréstimos'
END AS faixa_emprestimos,
STDDEV(salary_last_month) AS desvio_padrao_salario,
STDDEV(debt_ratio) AS desvio_padrao_debt_ratio
FROM
`projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
GROUP BY
faixa_emprestimos
ORDER BY
faixa_emprestimos;
```

####  Calcular quartis, decis ou percentis

✅ 1. Quartis de salário e inadimplência por quartil

```
WITH idade_quartil AS (
  SELECT
    user_id,
    age,
    default_flag,
    NTILE(4) OVER (ORDER BY age) AS quartil_idade
  FROM
    `projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
  WHERE
    age IS NOT NULL
)

SELECT
  quartil_idade,
  COUNT(*) AS total_clientes,
  SUM(default_flag) AS inadimplentes,
  ROUND(SUM(default_flag) * 100.0 / COUNT(*), 2) AS taxa_inadimplencia_percentual,
  MIN(age) AS idade_minima,
  MAX(age) AS idade_maxima
FROM
  idade_quartil
GROUP BY
  quartil_idade
ORDER BY
  quartil_idade;
```

✅ 2. Decis (10 grupos) para salary_last_month

```
WITH salario_decis AS (
  SELECT
    user_id,
    salary_last_month,
    default_flag,
    NTILE(10) OVER (ORDER BY salary_last_month) AS decil_salario
  FROM
    `projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
  WHERE
    salary_last_month IS NOT NULL
)

SELECT
  decil_salario,
  COUNT(*) AS total_clientes,
  SUM(default_flag) AS inadimplentes,
  ROUND(SUM(default_flag) * 100.0 / COUNT(*), 2) AS taxa_inadimplencia_percentual,
  MIN(salary_last_month) AS salario_minimo,
  MAX(salary_last_month) AS salario_maximo
FROM
  salario_decis
GROUP BY
  decil_salario
ORDER BY
  decil_salario;
```

✅ 3. Percentis (100 grupos) para debt_ratio

```
WITH debt_percentis AS (
  SELECT
    user_id,
    debt_ratio,
    default_flag,
    NTILE(100) OVER (ORDER BY debt_ratio) AS percentil_debt
  FROM
    `projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
  WHERE
    debt_ratio IS NOT NULL
)

SELECT
  percentil_debt,
  COUNT(*) AS total_clientes,
  SUM(default_flag) AS inadimplentes,
  ROUND(SUM(default_flag) * 100.0 / COUNT(*), 2) AS taxa_inadimplencia_percentual,
  MIN(debt_ratio) AS debt_minimo,
  MAX(debt_ratio) AS debt_maximo
FROM
  debt_percentis
GROUP BY
  percentil_debt
ORDER BY
  percentil_debt;
```

####  Calcular correlação entre variáveis ​​numéricas

✅ 1. Correlação com default_flag (inadimplência)

```
SELECT
  CORR(salary_last_month, default_flag) AS corr_salario_default,
  CORR(dependents, default_flag) AS corr_dependentes_default,
  CORR(debt_ratio, default_flag) AS corr_debt_default,
  CORR(loan_count, default_flag) AS corr_loans_default
FROM `projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`;
```

✅ 2.  Correlação entre outras variáveis

```
SELECT
  CORR(salary_last_month, debt_ratio) AS corr_salario_debt,
  CORR(salary_last_month, loan_count) AS corr_salario_loans,
  CORR(debt_ratio, loan_count) AS corr_debt_loans
  CORR(age, salary_last_month) AS corr_age_salary,
  CORR(dependents, loan_count) AS corr_dep_loans,
FROM `projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`;
```