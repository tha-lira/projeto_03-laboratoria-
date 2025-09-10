
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