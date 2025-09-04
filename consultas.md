
### 🟦 Processar e preparar a base de dados

#### 🔵 Identificar valores nulos

Objetivo: Identifique e trate valores nulos usando comandos SQL.

```
-- 🔍 Verificação de valores nulos por coluna na tabela

SELECT 
  COUNTIF(user_id IS NULL) AS coluna1_nulos,
  COUNTIF(age IS NULL) AS coluna2_nulos,
  COUNTIF(sex IS NULL) AS coluna3_nulos,
  COUNTIF(last_month_salary IS NULL) AS coluna4_nulos,
  COUNTIF(number_dependents IS NULL) AS coluna5_nulos,
FROM `projeto-risco-relativo-470919.bancoSuperCaja.user_info` 
```

```
-- 📊 Proporção de valores nulos em last_month_salary por perfil de inadimplência

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
-- 📐 Cálculo da mediana de last_month_salary 

SELECT
  APPROX_QUANTILES(last_month_salary, 100)[OFFSET(50)] AS mediana
FROM `projeto-risco-relativo-470919.bancoSuperCaja.user_info`
WHERE last_month_salary IS NOT NULL
```

#### 🔵 Identificar valores duplicados

Objetivo: Identifique e trate dados duplicados usando comandos SQL.

```
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
--

SELECT
  CORR(more_90_days_overdue, number_times_delayed_payment_loan_30_59_days)
FROM `projeto-risco-relativo-470919.bancoSuperCaja.loans_detail`

--

SELECT
  CORR(more_90_days_overdue, number_times_delayed_payment_loan_60_89_days)
FROM `projeto-risco-relativo-470919.bancoSuperCaja.loans_detail`

--

SELECT
  CORR(number_times_delayed_payment_loan_30_59_days, number_times_delayed_payment_loan_60_89_days) AS corr_30_60
FROM `projeto-risco-relativo-470919.bancoSuperCaja.loans_detail`

```

#### 🔵 Identificar e tratar dados discrepantes em variáveis ​​categóricas

Objetivo: Identifique e resolva valores inconsistentes usando comandos SQL, como UPPER, LOWER.

```
-- 

SELECT DISTINCT sex
FROM `projeto-risco-relativo-470919.bancoSuperCaja.user_info`
ORDER BY sex;

-- 

SELECT DISTINCT loan_type
FROM `projeto-risco-relativo-470919.bancoSuperCaja.loans_outstanding`
ORDER BY loan_type;
```

#### 🔵 Identificar e tratar dados discrepantes em variáveis ​​numéricas

Objetivo: Identificar e processar OUTLIERS

```
---

SELECT
  APPROX_QUANTILES(age, 4) AS quartis,
  AVG(age) AS media,
  STDDEV(age) AS desvio_padrao
FROM
  `projeto-risco-relativo-470919.bancoSuperCaja.user_info`;
```

####  🔵Criar novas variáveis

Objetivo: Crie novas variáveis ​​e salve-as em uma nova tabela ou visualização.

####  🔵 Unir tabelas

Objetivo: Entenda o comando JOIN e suas variedades para unir diferentes tabelas.

#### 🔵 Construir tabelas auxiliares

Objetivo: Use o comando WITH ou Subqueries para construir tabelas temporárias.