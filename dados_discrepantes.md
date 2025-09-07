🔍 Análise de Outliers para variáveis da tabela **user_info**

| Variável                                                | Q1    | Q2    | Q3    | Média    | Desvio Padrão | Máximo    | IQR  | Limite inferior| Limite superior |
| ------------------------------------------------------- | ----- | ----- | ----- | -------- | ------------- | --------- |----- | -------------- | --------------- |
| **user_id**     `Identificador`                         | ----- | ----- | ----- | -------- | ------------- | --------- |----- | -------------- | --------------- |
| **age**                                                 | 41    | 52    | 63    | 52.42    | 14.79         | 109       | 22   | 8              | 96              |
| **sex**                                                 | ----- | ----- | ----- | -------- | ------------- | --------- |----- | -------------- | --------------- |
| **last_month_salary**                                   |  3358 | 5400  | 8219  | 6675.05  | 12961.78      | 1560100   | 4861 | -3933.5        | 15510.5         |
| **number_dependents**                                   | 0     | 0     | 1     | 0.76     | 1.12          | 13        | 1    | -1.5           | 2.5             |

---

📌 Idade (age)

- Q1 = 41, Q3 = 63 → IQR = 22

- Limite superior = 96

- Valor máximo observado = 109

- Tratamento: valores acima de 96 foram winsorizados para 96.

📌 Salário do último mês (last_month_salary)

- Q1 = 3.358, Q3 = 8.219 → IQR = 4.861

- Limite superior = 15.510,5

- Valor máximo observado = 1.560.100

- Tratamento: valores acima de 15.510,5 foram winsorizados para 15.510,5.

📌 Número de dependentes (number_dependents)

- Q1 = 0, Q3 = 1 → IQR = 1

- Limite superior = 2,5

- Valor máximo observado = 13

- Tratamento: valores acima de 2,5 foram winsorizados para 2.

---

🔍 Análise de Outliers para variáveis da tabela **loans_outstanding**

| Variável    | Observações                                                                     |
| ---------------------------- | -------------------------------------------------------------- |
| **loan_id**  `Identificador` | Identificador, sem análise estatística                         |
| **user_id**  `Identificador` | Identificador, sem análise estatística                         |
| **loan_type**                | Inconsistência textual: "OTHER", "others", "real estate", etc. |

---

- As variáveis desta tabela referem-se a identificadores e categorias de empréstimos.

- Não houve detecção de outliers numéricos → nenhuma ação necessária.

---

🔍 Análise de Outliers para variáveis da tabela **loans_detail**

| Variável                                                | Q1    | Q2    | Q3    | Média  | Desvio Padrão | Máximo  | IQR   | Limite Inferior | Limite Superior |
| ------------------------------------------------------- | ----- | ----- | ----- | -------| ------------- | --------|  ---- | ----------------|  -------------- |
| **user_id** `Identificador`                             | ----- | ----- | ----- | -------| ------------- | --------|  ---- | ----------------|  -------------- |
| **more_90_days_overdue**                                |  0.0  | 0.0   | 0.0   | 0.261  | 4.12          | 98.0    | 0.0   | 0.0             | 0.0             |
| **using_lines_not_secured_personal_assets**             | 0.029 | 0.151 | 0.545 | 5.807  | 223.41        | 22000.0 | 0.516 | -0.745          | 1.319           |
| **lnumber_times_delayed_payment_loan_30_59_days**       |  0.0  | 0.0   | 0.0   | 0.419  | 4.144         | 98.0    | 0.0   | 0.0             | 0.0             |
| **debt_ratio**                                          | 0.173 | 0.361 | 0.891 | 351.58 | 2011.63       | 307001  | 0.718 | -0.905          | 1.968           |
| **number_times_delayed_payment_loan_60_89_days**        |  0.0  | 0.0   | 0.0   | 0.238  | 4.106         | 98.0    | 0.0   | 0.0             | 0.0             |

📌 Atrasos acima de 90 dias (more_90_days_overdue)

Distribuição extremamente concentrada em 0, com valores máximos em 98 (outliers).

Tratamento: valores acima de 0 foram winsorizados para 0.

📌 Uso de linhas de crédito não garantidas (using_lines_not_secured_personal_assets)

- Q1 = 0,029, Q3 = 0,545 → IQR = 0,516

- Limite superior ≈ 1,319

- Valor máximo observado = 22.000

- Tratamento: valores acima de 1,319 foram winsorizados para 1,319.

📌 Atrasos de 30–59 dias (number_times_delayed_payment_loan_30_59_days)

- Distribuição semelhante ao atraso de 90 dias (outliers com valor máximo de 98).

- Tratamento: valores acima de 0 foram winsorizados para 0.

📌 Atrasos de 60–89 dias (number_times_delayed_payment_loan_60_89_days)

- Mesmo padrão dos demais atrasos, com valor máximo = 98.

- Tratamento: valores acima de 0 foram winsorizados para 0.

📌 Índice de endividamento (debt_ratio)

- Q1 = 0,173, Q3 = 0,891 → IQR = 0,718

- Limite superior = 1,968

- Valor máximo observado = 307.001

- Tratamento: valores acima de 1,968 foram winsorizados para 1,968.

---

🔍 Análise de Outliers para variáveis da tabela **defalt**

| Variável                    | Q1 | Q2 | Q3 | Média | Desvio Padrão | Máximo | IQR | Limite Inferior | Limite Superior |
| --------------------------- | -- | -- | -- | ----- | ------------- | ------ | --- | --------------- | --------------- |
| **user_id** `Identificador` | -- | -- | -- | ----- | ------------- | ------ | --- | --------------- | --------------- |
| **default_flag**            | 0  | 0  | 0  | 0.019 | 0.136         | 1      | 0   | 0               | 0               |

- A variável default_flag é binária (0/1).

- Não foram identificados outliers.

---