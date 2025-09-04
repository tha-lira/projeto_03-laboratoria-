🔍 Análise de Outliers para variáveis da tabela **user_info**

| Variável                                                | Q1    | Q2    | Q3    | Média    | Desvio Padrão | Máximo    |
| ------------------------------------------------------- | ----- | ----- | ----- | -------- | ------------- | --------- | 
| **user_id**     `Identificador`                         | ----- | ----- | ----- | -------- | ------------- | --------- | 
| **age**                                                 | 41    | 52    | 63    | 52,42    | 14,79         | 109       |
| **sex**                                                 | ----- | ----- | ----- | -------- | ------------- | --------- | 
| **last_month_salary**                                   | 3.358 | 5.400 | 8.219 | 6.675,05 | 12.961,78     | 1.560.100 |
| **number_dependents**                                   | 0     | 0     | 1     | 0,76     | 1,11          | 13        |

🔍 Análise de Outliers para variáveis da tabela **loans_outstanding**

| Variável    | Observações                                                                     |
| ---------------------------- | -------------------------------------------------------------- |
| **loan_id**  `Identificador` | Identificador, sem análise estatística                         |
| **user_id**  `Identificador` | Identificador, sem análise estatística                         |
| **loan_type**                | Inconsistência textual: "OTHER", "others", "real estate", etc. |

🔍 Análise de Outliers para variáveis da tabela **loans_detail**

| Variável                                                | Q1    | Q2    | Q3    | Média    | Desvio Padrão | Máximo    |
| ------------------------------------------------------- | ----- | ----- | ----- | -------- | ------------- | --------- | 
| **user_id** `Identificador`                             | ----- | ----- | ----- | -------- | ------------- | --------- | 
| **more_90_days_overdue**                                | 0     | 0     | 0     | 0,26     | 4,12          | 98        |
| **using_lines_not_secured_personal_assets**             | 0,029 | 0,15  | 0,54  | 5,81     | 223,41        | 22.000    |
| **lnumber_times_delayed_payment_loan_30_59_days**       | 0     | 0     | 0     | 0,42     | 4,14          | 98        |
| **debt_ratio**                                          | 0,17  | 0,36  | 0,89  | 351,58   | 2011,63       | 307001    |
| **number_times_delayed_payment_loan_60_89_days**        | 0     | 0     | 0     | 0,24     | 4,11          | 98        |

🔍 Análise de Outliers para variáveis da tabela **defalt**

| Variável                    | Q1    | Q2    | Q3    | Média    | Desvio Padrão | Máximo    |
| --------------------------- | ----- | ----- | ----- | -------- | ------------- | --------- | 
| **user_id** `Identificador` | ----- | ----- | ----- | -------- | ------------- | --------- | 
| **default_flag**            | ----- | ----- | ----- | 0,019    | 0,14          | 1         | 










🔍 Análise de Outliers para variáveis da tabela **loans_detail**

| Variável                    | Q1    | Q2    | Q3    | Média    | Desvio Padrão | Máximo    |
| -----------------------     | ----- | ----- | ----- | -------- | ------------- | --------- | 
| **user_id**                 | ----- | ----- | ----- | -------- | ------------- | --------- | 
| **more_90_days_overdue**    | 41    | 52    | 63    | 52,42    | 14,79         | 109       |
| **sex (F/M)**               | ----- | ----- | ----- | -------- | ------------- | --------- | 
| **last_month_salary**       | 3.358 | 5.400 | 8.219 | 6.675,05 | 12.961,78     | 1.560.100 |
| **number_dependents**       | 0     | 0     | 1     | 0,76     | 1,11          | 13        |

🔍 Análise de Outliers para variáveis da tabela **user_info**

| Variável                                                | Q1    | Q2    | Q3    | Média    | Desvio Padrão | Máximo    |
| ---------------------------------------------------     | ----- | ----- | ----- | -------- | ------------- | --------- | 
| **user_id**                                             | ----- | ----- | ----- | -------- | ------------- | --------- | 
| **age**                                                 | 0,029 | 0,15  | 0,54  | 5,81     | 223,41        | 22.000    |
| **using_lines_not_secured_personal_assets**             | ----- | ----- | ----- | -------- | ------------- | --------- | 
| **lnumber_times_delayed_payment_loan_30_59_days**       | 0     | 0     | 0     | 0,42     | 4,14          | 98        |
| **debt_ratio**                                          | 0,17  | 0,36  | 0,89  | 351,58   | 2011,63       | 307001    |
| **number_times_delayed_payment_loan_60_89_days**        | 0     | 0     | 0     | 0,24     | 4,11          | 98        |




🔍 Análise de Outliers para variáveis da tabela **defalt**

| Variável          | Q1    | Q2    | Q3    | Média    | Desvio Padrão | Máximo    |
| ----------------- | ----- | ----- | ----- | -------- | ------------- | --------- | 
| **user_id**       | ----- | ----- | ----- | -------- | ------------- | --------- | 
| **default_flag**  | ----- | ----- | ----- | -------- | ------------- | --------- | 