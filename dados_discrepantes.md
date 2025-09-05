🔍 Análise de Outliers para variáveis da tabela **user_info**

| Variável                                                | Q1    | Q2    | Q3    | Média    | Desvio Padrão | Máximo    | IQR  | Limite inferior| Limite superior |
| ------------------------------------------------------- | ----- | ----- | ----- | -------- | ------------- | --------- |----- | -------------- | --------------- |
| **user_id**     `Identificador`                         | ----- | ----- | ----- | -------- | ------------- | --------- |----- | -------------- | --------------- |
| **age**                                                 | 41    | 52    | 63    | 52.42    | 14.79         | 109       | 22   | 8              | 96              |
| **sex**                                                 | ----- | ----- | ----- | -------- | ------------- | --------- |----- | -------------- | --------------- |
| **last_month_salary**                                   |  3358 | 5400  | 8219  | 6675.05  | 12961.78      | 1560100   | 4861 | -3933.5        | 15510.5         |
| **number_dependents**                                   | 0     | 0     | 1     | 0.76     | 1.12          | 13        | 1    | -1.5           | 2.5             |

🔍 Análise de Outliers para variáveis da tabela **loans_outstanding**
- Apenas variáveis categóricas ou identificadores, não aplicável análise de outliers.

| Variável    | Observações                                                                     |
| ---------------------------- | -------------------------------------------------------------- |
| **loan_id**  `Identificador` | Identificador, sem análise estatística                         |
| **user_id**  `Identificador` | Identificador, sem análise estatística                         |
| **loan_type**                | Inconsistência textual: "OTHER", "others", "real estate", etc. |

🔍 Análise de Outliers para variáveis da tabela **loans_detail**

| Variável                                                | Q1    | Q2    | Q3    | Média  | Desvio Padrão | Máximo  | IQR   | Limite Inferior | Limite Superior |
| ------------------------------------------------------- | ----- | ----- | ----- | -------| ------------- | --------|  ---- | ----------------|  -------------- |
| **user_id** `Identificador`                             | ----- | ----- | ----- | -------| ------------- | --------|  ---- | ----------------|  -------------- |
| **more_90_days_overdue**                                |  0.0  | 0.0   | 0.0   | 0.261  | 4.12          | 98.0    | 0.0   | 0.0             | 0.0             |
| **using_lines_not_secured_personal_assets**             | 0.029 | 0.151 | 0.545 | 5.807  | 223.41        | 22000.0 | 0.516 | -0.745          | 1.319           |
| **lnumber_times_delayed_payment_loan_30_59_days**       |  0.0  | 0.0   | 0.0   | 0.419  | 4.144         | 98.0    | 0.0   | 0.0             | 0.0             |
| **debt_ratio**                                          | 0.173 | 0.361 | 0.891 | 351.58 | 2011.63       | 307001  | 0.718 | -0.905          | 1.968           |
| **number_times_delayed_payment_loan_60_89_days**        |  0.0  | 0.0   | 0.0   | 0.238  | 4.106         | 98.0    | 0.0   | 0.0             | 0.0             |

🔍 Análise de Outliers para variáveis da tabela **defalt**

| Variável                    | Q1 | Q2 | Q3 | Média | Desvio Padrão | Máximo | IQR | Limite Inferior | Limite Superior |
| --------------------------- | -- | -- | -- | ----- | ------------- | ------ | --- | --------------- | --------------- |
| **user_id** `Identificador` | -- | -- | -- | ----- | ------------- | ------ | --- | --------------- | --------------- |
| **default_flag**            | 0  | 0  | 0  | 0.019 | 0.136         | 1      | 0   | 0               | 0               |
