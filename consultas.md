
### 🟦 Processar e preparar a base de dados

#### 🔵 Identificar valores nulos

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

#### 🔵 Identificar e tratar valores duplicados
#### 🔵 Identificar e gerenciar dados fora do escopo de análise
#### 🔵 Identificar e tratar dados discrepantes em variáveis ​​categóricas
#### 🔵 Identificar e tratar dados discrepantes em variáveis ​​numéricas
#### 🔵 Verificar e alterar o tipo de dados
