🔧 Tratamento da Tabela user_info

| **Ação**                        | **Variável**        | **Justificativa**                                                                    |
| ------------------------------- | ------------------- | ------------------------------------------------------------------------------------ |
| **Preencher nulos com mediana** | `last_month_salary` | Proporção relevante de nulos (\~20%). A mediana reduz impacto de valores extremos.   |
| **Preencher nulos com mediana** | `number_dependents` | Proporção baixa de nulos (\~2,6%). A mediana mantém consistência sem perda de dados. |
| **Remover variável**            | `sex`               | Informação sensível e de uso restrito em crédito (questões éticas/legais).           |
| **Manter variável**             | `user_id`           | Identificador único, necessário para junções (`JOINs`).                              |

🔧 Tratamento da Tabela loans_outstanding

| **Ação**                           | **Variável**           | **Justificativa**                                                                   |
| ---------------------------------- | ---------------------- | ----------------------------------------------------------------------------------- |
| **Padronizar valores categóricos** | `loan_type`            | Corrigir inconsistências textuais (ex: “REAL ESTATE”, “others”) e unificar valores. |
| **Criar variável derivada**        | `loan_count`           | Total de empréstimos por cliente, indicador do perfil de contratação.               |
| **Criar variável derivada**        | `count_real_estate`    | Quantidade de empréstimos do tipo “real estate”.                                    |
| **Criar variável derivada**        | `count_other`          | Quantidade de empréstimos do tipo “other”.                                          |
| **Criar variável binária**         | `has_real_estate_loan` | Indica se o cliente possui pelo menos um empréstimo “real estate” (0/1).            |
| **Criar variável binária**         | `has_other_loan`       | Indica se o cliente possui pelo menos um empréstimo “other” (0/1).                  |
| **Manter variável**                | `user_id`              | Identificador único, necessário para junções (`JOINs`).                             |

🔧 Tratamento da Tabela loans_detail

| **Ação**             | **Variável**                                   | **Justificativa**                                                                       |
| -------------------- | ---------------------------------------------- | --------------------------------------------------------------------------------------- |
| **Remover variável** | `number_times_delayed_payment_loan_30_59_days` | Correlação muito alta com outras variáveis de atraso (corr > 0,98). Redundante.         |
| **Remover variável** | `number_times_delayed_payment_loan_60_89_days` | Correlação quase perfeita com outras variáveis (corr > 0,99). Pode gerar colinearidade. |
| **Manter variável**  | `debt_ratio`                                   | Correlação quase nula com inadimplência (corr ≈ -0,007), mas mantida para teste.        |
| **Manter variável**  | `more_90_days_overdue`                         | Correlação moderada com inadimplência (corr ≈ 0,31). Indicador relevante.               |
| **Manter variável**  | `using_lines_not_secured_personal_assets`      | Representa uso de crédito rotativo; potencialmente informativa.                         |
| **Manter variável**  | `user_id`                                      | Identificador único, necessário para junções (`JOINs`).                                 |

🔧 Tratamento da Tabela default

- A tabela foi mantida integralmente, com as variáveis user_id e default_flag, necessárias para modelagem preditiva.