
### 🟦 Processar e preparar a base de dados

#### 🔵 Identificar e tratar valores nulos


SELECT
  COUNTIF(user_id IS NULL) AS coluna1_nulos,
  COUNTIF(default_flag IS NULL) AS coluna2_nulos,
FROM `projeto-risco-relativo-470919.bancoSuperCaja.default`;

Linha	coluna1_nulos	coluna2_nulos
1	0	0

SELECT 
  COUNTIF(user_id IS NULL) AS coluna1_nulos,
  COUNTIF(more_90_days_overdue IS NULL) AS coluna2_nulos,
  COUNTIF(using_lines_not_secured_personal_assets IS NULL) AS coluna3_nulos,
  COUNTIF(number_times_delayed_payment_loan_30_59_days IS NULL) AS coluna4_nulos,
  COUNTIF(debt_ratio IS NULL) AS coluna5_nulos,
  COUNTIF(number_times_delayed_payment_loan_60_89_days IS NULL) AS coluna6_nulos,
FROM `projeto-risco-relativo-470919.bancoSuperCaja.loans_detail` 

Linha	coluna1_nulos	coluna2_nulos	coluna3_nulos	coluna4_nulos	coluna5_nulos	coluna6_nulos
1	0	0	0	0	0	0


SELECT 
  COUNTIF(loan_id IS NULL) AS coluna1_nulos,
  COUNTIF(user_id IS NULL) AS coluna2_nulos,
  COUNTIF(loan_type IS NULL) AS coluna3_nulos,
FROM `projeto-risco-relativo-470919.bancoSuperCaja.loans_outstanding`

Linha	coluna1_nulos	coluna2_nulos	coluna3_nulos
1	0	0	0

SELECT 
  COUNTIF(user_id IS NULL) AS coluna1_nulos,
  COUNTIF(age IS NULL) AS coluna2_nulos,
  COUNTIF(sex IS NULL) AS coluna3_nulos,
  COUNTIF(last_month_salary IS NULL) AS coluna4_nulos,
  COUNTIF(number_dependents IS NULL) AS coluna5_nulos,
FROM `projeto-risco-relativo-470919.bancoSuperCaja.user_info` 

Linha	coluna1_nulos	coluna2_nulos	coluna3_nulos	coluna4_nulos	coluna5_nulos
1	0	0	0	7199	943