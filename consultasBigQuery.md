
### 🟦 Processar e preparar a base de dados

#### 🔵 Identificar valores nulos

Objetivo: Identifique e trate valores nulos usando comandos SQL.

```
-- 🔍 Verificação de valores nulos por coluna
SELECT 
  COUNTIF(user_id IS NULL) AS coluna1_nulos,
  COUNTIF(age IS NULL) AS coluna2_nulos,
  COUNTIF(sex IS NULL) AS coluna3_nulos,
  COUNTIF(last_month_salary IS NULL) AS coluna4_nulos,
  COUNTIF(number_dependents IS NULL) AS coluna5_nulos,
FROM `projeto-risco-relativo-470919.bancoSuperCaja.user_info` 
```

```
-- 🔍 Proporção de valores nulos em last_month_salary por perfil de inadimplência
SELECT 
  d.default_flag,
  COUNT(*) AS total_registros,
  COUNTIF(u.last_month_salary IS NULL) AS nulos_last_month_salary,
  COUNTIF(u.last_month_salary IS NOT NULL) AS nao_nulos_last_month_salary,
  ROUND(COUNTIF(u.last_month_salary IS NULL) / COUNT(*) * 100, 2) AS percentual_nulo
FROM `projeto-risco-relativo-470919.bancoSuperCaja.user_info` u
LEFT JOIN `projeto-risco-relativo-470919.bancoSuperCaja.default` d
  ON u.user_id = d.user_id
GROUP BY d.default_flag
ORDER BY d.default_flag
```

```
-- 🔍 Cálculo da mediana de last_month_salary 
SELECT
  APPROX_QUANTILES(last_month_salary, 100)[OFFSET(50)] AS mediana
FROM `projeto-risco-relativo-470919.bancoSuperCaja.user_info`
WHERE last_month_salary IS NOT NULL
```

#### 🔵 Identificar valores duplicados

Objetivo: Identifique e trate dados duplicados usando comandos SQL.

```
-- 🔍 Consulta para detectar dados duplicados 
SELECT
  user_id,
  age,
  sex,
  last_month_salary,
  number_dependents,
  COUNT(*) AS qtd_duplicados
FROM `projeto-risco-relativo-470919.bancoSuperCaja.user_info`
GROUP BY user_id, age, sex, last_month_salary, number_dependents
HAVING COUNT(*) > 1
ORDER BY qtd_duplicados DESC;
```

#### 🔵 Identificar e gerenciar dados fora do escopo de análise

Objetivo: Use CORR para entender a correlação entre variáveis

```
-- 🔍 Análise de correlação entre variáveis 
SELECT
  CORR(more_90_days_overdue, number_times_delayed_payment_loan_30_59_days)
FROM `projeto-risco-relativo-470919.bancoSuperCaja.loans_detail`

SELECT
  CORR(more_90_days_overdue, number_times_delayed_payment_loan_60_89_days)
FROM `projeto-risco-relativo-470919.bancoSuperCaja.loans_detail`

SELECT
  CORR(number_times_delayed_payment_loan_30_59_days, number_times_delayed_payment_loan_60_89_days) AS corr_30_60
FROM `projeto-risco-relativo-470919.bancoSuperCaja.loans_detail`

```

#### 🔵 Identificar e tratar dados discrepantes em variáveis ​​categóricas

Objetivo: Identifique e resolva valores inconsistentes usando comandos SQL, como UPPER, LOWER.

```
--  🔍 Identificação de valores distintos
SELECT DISTINCT sex
FROM `projeto-risco-relativo-470919.bancoSuperCaja.user_info`
ORDER BY sex;

SELECT DISTINCT loan_type
FROM `projeto-risco-relativo-470919.bancoSuperCaja.loans_outstanding`
ORDER BY loan_type;
```

#### 🔵 Identificar e tratar dados discrepantes em variáveis ​​numéricas

Objetivo: Identificar e processar OUTLIERS

```
SELECT
  'nome_variavel' AS variavel,
  APPROX_QUANTILES(coluna, 4)[OFFSET(1)] AS Q1,
  APPROX_QUANTILES(coluna, 4)[OFFSET(2)] AS Q2,
  APPROX_QUANTILES(coluna, 4)[OFFSET(3)] AS Q3,
  AVG(coluna) AS media,
  STDDEV(coluna) AS desvio_padrao,
  MAX(coluna) AS maximo,
  (APPROX_QUANTILES(coluna, 4)[OFFSET(3)] - APPROX_QUANTILES(coluna, 4)[OFFSET(1)]) AS IQR,
  (APPROX_QUANTILES(coluna, 4)[OFFSET(1)] - 1.5 * (APPROX_QUANTILES(coluna, 4)[OFFSET(3)] - APPROX_QUANTILES(coluna, 4)[OFFSET(1)])) AS limite_inferior,
  (APPROX_QUANTILES(coluna, 4)[OFFSET(3)] + 1.5 * (APPROX_QUANTILES(coluna, 4)[OFFSET(3)] - APPROX_QUANTILES(coluna, 4)[OFFSET(1)])) AS limite_superior
FROM
  `projeto-risco-relativo-470919.bancoSuperCaja.nome_tabela`;
```

#### Tratamento individual das tabelas

Antes da unificação das tabelas, cada uma passou por um processo de limpeza e transformação com foco na padronização, tratamento de valores nulos, outliers e preparação para criação de variáveis agregadas. Abaixo estão os detalhes de cada etapa.

🔧 Tratamento da `Tabela user_info`

```
CREATE OR REPLACE TABLE `projeto-risco-relativo-470919.bancoSuperCaja.user_info_tratada` AS
SELECT
  user_id,
  IF(age > 96, 96, age) AS age,
  IF(IFNULL(last_month_salary, 5400) > 15510.5, 15510.5, IFNULL(last_month_salary, 5400)) AS last_month_salary,
  IF(IFNULL(number_dependents, 0) > 2, 2, IFNULL(number_dependents, 0)) AS number_dependents
FROM
  `projeto-risco-relativo-470919.bancoSuperCaja.user_info`;

```

🔧 Tratamento da Tabela loans_outstanding

```
CREATE OR REPLACE TABLE `projeto-risco-relativo-470919.bancoSuperCaja.loans_outstanding_tratada` AS
SELECT
  user_id,
  COUNT(*) AS loan_count,
  COUNTIF(loan_type_padrao = 'real estate') AS count_real_estate,
  COUNTIF(loan_type_padrao = 'other') AS count_other,
  IF(COUNTIF(loan_type_padrao = 'real estate') > 0, 1, 0) AS has_real_estate_loan,
  IF(COUNTIF(loan_type_padrao = 'other') > 0, 1, 0) AS has_other_loan
FROM (
  SELECT
    user_id,
    -- padroniza valores de loan_type
    CASE
      WHEN LOWER(loan_type) LIKE '%real%' THEN 'real estate'
      WHEN LOWER(loan_type) LIKE '%other%' THEN 'other'
      ELSE LOWER(loan_type)
    END AS loan_type_padrao
  FROM
    `projeto-risco-relativo-470919.bancoSuperCaja.loans_outstanding`
)
GROUP BY user_id;
```

🔧 Tratamento da Tabela loans_detail

```
CREATE OR REPLACE TABLE `projeto-risco-relativo-470919.bancoSuperCaja.loans_detail_tratada` AS
SELECT
  user_id,
  debt_ratio,
  IF(more_90_days_overdue > 0, 1, more_90_days_overdue) AS more_90_days_overdue,
  IF(using_lines_not_secured_personal_assets > 1.319, 1.319, using_lines_not_secured_personal_assets) AS using_lines_not_secured_personal_assets
FROM
  `projeto-risco-relativo-470919.bancoSuperCaja.loans_detail`;

```

🔧 Tratamento da Tabela default

- A tabela manteve as duas colunas originais, para uso posterior na análise preditiva.

####  🔵 Unir tabelas

Objetivo: Entenda o comando JOIN e suas variedades para unir diferentes tabelas.

```
CREATE OR REPLACE TABLE `projeto-risco-relativo-470919.bancoSuperCaja.base_unificada` AS
SELECT 
  -- 🔹 user_info_tratada
  u.user_id,
  u.age,
  u.last_month_salary AS salary_last_month,
  u.number_dependents AS dependents,

  -- 🔹 loans_outstanding_tratada
  IFNULL(lo.loan_count, 0) AS loan_count,
  IFNULL(lo.count_real_estate, 0) AS loan_count_real_estate,
  IFNULL(lo.count_other, 0) AS loan_count_other,
  IFNULL(lo.has_real_estate_loan, 0) AS has_real_estate_loan,
  IFNULL(lo.has_other_loan, 0) AS has_other_loan,

  -- 🔹 loans_detail_tratada
  IFNULL(ld.more_90_days_overdue, 0) AS overdue_90_days,
  IFNULL(ld.using_lines_not_secured_personal_assets, 0) AS unsecured_credit_lines,
  IFNULL(ld.number_times_delayed_payment_loan_30_59_days, 0) AS delays_30_59_days,
  IFNULL(ld.number_times_delayed_payment_loan_60_89_days, 0) AS delays_60_89_days,
  IFNULL(ld.debt_ratio, 0) AS debt_ratio,

  -- 🔹 default_tratada
  IFNULL(d.default_flag, 0) AS default_flag

FROM `projeto-risco-relativo-470919.bancoSuperCaja.user_info_tratada` u
LEFT JOIN `projeto-risco-relativo-470919.bancoSuperCaja.loans_outstanding_tratada` lo
  ON u.user_id = lo.user_id
LEFT JOIN `projeto-risco-relativo-470919.bancoSuperCaja.loans_detail_tratada` ld
  ON u.user_id = ld.user_id
LEFT JOIN `projeto-risco-relativo-470919.bancoSuperCaja.default_tratada` d
  ON u.user_id = d.user_id;
```

### 2.2 Fazer uma análise exploratória

#### 🟣 Agrupar dados de acordo com variáveis ​​categóricas

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

#### 🟣 Ver variáveis ​​categóricas

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

#### 🟣 Aplicar medidas de tendência central (moda, média, mediana)

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

#### 🟣 Ver distribuição

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

#### 🟣 Aplicar medidas de dispersão (desvio padrão)

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

#### 🟣 Calcular quartis, decis ou percentis
Objetivo: Calcular quartis para variáveis ​​de risco relativo no BigQuery

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

#### 🟣 Calcular correlação entre variáveis ​​numéricas
Objetivo: Compreender a relação que existe entre variáveis ​​numéricas através de correlações. Use gráficos de dispersão e linhas de tendência. Você também pode usar o comando CORR no BigQuery

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

### 🟥 2.3 Aplicar técnica de análise


#### 🔴 Calcular risco relativo

✅ 1. Risco relativo por faixa etária (age)

```
WITH base AS (
    SELECT
        CASE
            WHEN age < 25 THEN '18-24'
            WHEN age BETWEEN 25 AND 34 THEN '25-34'
            WHEN age BETWEEN 35 AND 44 THEN '35-44'
            WHEN age BETWEEN 45 AND 54 THEN '45-54'
            ELSE '55+'
        END AS faixa_etaria,
        COUNT(*) AS total_clientes,
        SUM(CASE WHEN default_flag = 1 THEN 1 ELSE 0 END) AS inadimplentes,
        ROUND(1.0 * SUM(CASE WHEN default_flag = 1 THEN 1 ELSE 0 END) / COUNT(*), 4) AS taxa_inadimplencia
FROM `projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
    GROUP BY faixa_etaria
),
referencia AS (
    SELECT taxa_inadimplencia FROM base WHERE faixa_etaria = '55+'
)
SELECT
    b.faixa_etaria,
    b.total_clientes,
    b.inadimplentes,
    b.taxa_inadimplencia,
    ROUND(b.taxa_inadimplencia / r.taxa_inadimplencia, 2) AS risco_relativo
FROM base b, referencia r
ORDER BY risco_relativo DESC;
```

✅ 2. Risco relativo por debt_ratio (baixo, médio, alto)

```
WITH base AS (
    SELECT
        CASE
            WHEN debt_ratio < 0.2 THEN 'Baixo'
            WHEN debt_ratio BETWEEN 0.2 AND 0.5 THEN 'Médio'
            ELSE 'Alto'
        END AS faixa_debt_ratio,
        COUNT(*) AS total_clientes,
        SUM(CASE WHEN default_flag = 1 THEN 1 ELSE 0 END) AS inadimplentes,
        ROUND(1.0 * SUM(CASE WHEN default_flag = 1 THEN 1 ELSE 0 END) / COUNT(*), 4) AS taxa_inadimplencia
    FROM `projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
    GROUP BY faixa_debt_ratio
),
referencia AS (
    SELECT taxa_inadimplencia FROM base WHERE faixa_debt_ratio = 'Baixo'
)
SELECT
    b.faixa_debt_ratio,
    b.total_clientes,
    b.inadimplentes,
    b.taxa_inadimplencia,
    ROUND(b.taxa_inadimplencia / r.taxa_inadimplencia, 2) AS risco_relativo
FROM base b, referencia r
ORDER BY risco_relativo DESC;
```

✅ 3. Risco relativo por has_real_estate_loan (com ou sem imóvel financiado)

```
WITH base AS (
    SELECT
        CASE
            WHEN has_real_estate_loan = 1 THEN 'Com Imóvel Financiado'
            ELSE 'Sem Imóvel Financiado'
        END AS grupo_imovel,
        COUNT(*) AS total_clientes,
        SUM(CASE WHEN default_flag = 1 THEN 1 ELSE 0 END) AS inadimplentes,
        ROUND(1.0 * SUM(CASE WHEN default_flag = 1 THEN 1 ELSE 0 END) / COUNT(*), 4) AS taxa_inadimplencia
    FROM `projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
    GROUP BY grupo_imovel
),
referencia AS (
    SELECT taxa_inadimplencia FROM base WHERE grupo_imovel = 'Sem Imóvel Financiado'
)
SELECT
    b.grupo_imovel,
    b.total_clientes,
    b.inadimplentes,
    b.taxa_inadimplencia,
    ROUND(b.taxa_inadimplencia / r.taxa_inadimplencia, 2) AS risco_relativo
FROM base b, referencia r
ORDER BY risco_relativo DESC;
```

✅ 4. Risco relativo por loan_count (poucos vs. muitos empréstimos)

```
WITH base AS (
    SELECT
        CASE
            WHEN loan_count = 0 THEN 'Sem Empréstimos'
            WHEN loan_count BETWEEN 1 AND 2 THEN 'Poucos'
            ELSE 'Muitos'
        END AS grupo_emprestimos,
        COUNT(*) AS total_clientes,
        SUM(CASE WHEN default_flag = 1 THEN 1 ELSE 0 END) AS inadimplentes,
        ROUND(1.0 * SUM(CASE WHEN default_flag = 1 THEN 1 ELSE 0 END) / COUNT(*), 4) AS taxa_inadimplencia
    FROM `projeto-risco-relativo-470919.bancoSuperCaja.base_unificada`
    GROUP BY grupo_emprestimos
),
referencia AS (
    SELECT taxa_inadimplencia FROM base WHERE grupo_emprestimos = 'Sem Empréstimos'
)
SELECT
    b.grupo_emprestimos,
    b.total_clientes,
    b.inadimplentes,
    b.taxa_inadimplencia,
    ROUND(b.taxa_inadimplencia / r.taxa_inadimplencia, 2) AS risco_relativo
FROM base b, referencia r
ORDER BY risco_relativo DESC;
```

#### 🔴 Aplicar segmentação por Score

