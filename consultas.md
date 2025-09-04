
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
-- Análise estatística descritiva
SELECT
  APPROX_QUANTILES(age, 4) AS quartis,
  AVG(age) AS media,
  STDDEV(age) AS desvio_padrao
FROM
  `projeto-risco-relativo-470919.bancoSuperCaja.user_info`;
```

#### Tratamento das tabelas 

🔧 Tratamento da `Tabela user_info`

| Ação                            | Variável            | Justificativa                                                                            |
| ------------------------------- | ------------------- | ---------------------------------------------------------------------------------------- |
| **Preencher nulos com mediana** | `last_month_salary` | Alta proporção de nulos (\~20%). Substituir pela mediana evita viés extremo.             |
| **Preencher nulos com mediana** | `number_dependents` | Proporção de nulos baixa (\~2,6%). A mediana mantém consistência e evita perda de dados. |
| **Remover variável**            | `sex`               | Informação sensível (ética/legal). Não deve ser usada em modelos de crédito.             |
| **Manter variável**             | `user_id`           | Necessária para realizar junções (`JOINs`) com outras tabelas posteriormente.            |

```
CREATE OR REPLACE TABLE `projeto-risco-relativo-470919.bancoSuperCaja.user_info_tratada` AS
SELECT
  user_id,
  age,
  IFNULL(last_month_salary, 5400) AS last_month_salary,  
  IFNULL(number_dependents, 0) AS number_dependents     
FROM
  `projeto-risco-relativo-470919.bancoSuperCaja.user_info`;
```

🔧 Tratamento da Tabela loans_outstanding

| **Ação**                           | **Variável**           | **Justificativa**                                                                          |
| ---------------------------------- | ---------------------- | ------------------------------------------------------------------------------------------ |
| **Padronizar valores categóricos** | `loan_type`            | Corrigir inconsistências de escrita (ex: “REAL ESTATE”, “others”) e padronizar os valores. |
| **Criar variável derivada**        | `loan_count`           | Representa o total de empréstimos por cliente, útil para entender o perfil de contratação. |
| **Criar variável derivada**        | `count_real_estate`    | Mostra quantos empréstimos do tipo “real estate” o cliente possui.                         |
| **Criar variável derivada**        | `count_other`          | Mostra quantos empréstimos do tipo “other” o cliente possui.                               |
| **Criar variável binária**         | `has_real_estate_loan` | Indica (0/1) se o cliente possui ao menos um empréstimo “real estate”.                     |
| **Criar variável binária**         | `has_other_loan`       | Indica (0/1) se o cliente possui ao menos um empréstimo “other”.                           |
| **Manter variável**                | `user_id`              | Chave para junção com outras tabelas (JOIN).                                               |

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

| **Ação**             | **Variável**                                   | **Justificativa**                                                                   |
| -------------------- | ---------------------------------------------- | ----------------------------------------------------------------------------------- |
| **Remover variável** | `number_times_delayed_payment_loan_30_59_days` | Correlação muito alta com outras variáveis de atraso (corr > 0,98). Redundante.     |
| **Remover variável** | `number_times_delayed_payment_loan_60_89_days` | Alta correlação com outras variáveis (corr > 0,99). Pode causar multicolinearidade. |
| **Remover variável** | `debt_ratio`                                   | Correlação quase nula com inadimplência (corr ≈ -0,007). Pouco valor preditivo.     |
| **Manter variável**  | `more_90_days_overdue`                         | Correlação moderada com inadimplência (corr ≈ 0,31). Indicador relevante de risco.  |
| **Manter variável**  | `using_lines_not_secured_personal_assets`      | Pode representar uso de crédito rotativo; útil na modelagem.                        |
| **Manter variável**  | `user_id`                                      | Necessária para junção com outras tabelas (JOIN).                                   |

```
CREATE OR REPLACE TABLE `projeto-risco-relativo-470919.bancoSuperCaja.loans_detail_tratada` AS
SELECT
  user_id,
  more_90_days_overdue,
  using_lines_not_secured_personal_assets
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
  -- user_info
  u.user_id,
  u.age,
  u.last_month_salary,
  u.number_dependents,

  -- loans_outstanding
  lo.loan_count,
  lo.count_real_estate,
  lo.count_other,
  lo.has_real_estate_loan,
  lo.has_other_loan,

  -- loans_detail
  ld.more_90_days_overdue,
  ld.using_lines_not_secured_personal_assets,

  -- default
  d.default_flag

FROM `projeto-risco-relativo-470919.bancoSuperCaja.user_info_tratada` u

LEFT JOIN `projeto-risco-relativo-470919.bancoSuperCaja.loans_outstanding_tratada` lo
  ON u.user_id = lo.user_id

LEFT JOIN `projeto-risco-relativo-470919.bancoSuperCaja.loans_detail_tratada` ld
  ON u.user_id = ld.user_id

LEFT JOIN `projeto-risco-relativo-470919.bancoSuperCaja.default_tratada` d
  ON u.user_id = d.user_id;
```

#### 🔵 Construir tabelas auxiliares

Objetivo: Use o comando WITH ou Subqueries para construir tabelas temporárias.