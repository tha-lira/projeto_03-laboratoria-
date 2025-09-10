🔧 Tratamento da Tabela user_info

| **Ação**                    | **Variável**        | **Justificativa**                                                                      |
| --------------------------- | ------------------- | -------------------------------------------------------------------------------------- |
| Preencher nulos com mediana | `last_month_salary` | Apresenta proporção relevante de nulos (\~20%). A mediana reduz o impacto de outliers. |
| Preencher nulos com mediana | `number_dependents` | Apesar da baixa proporção de nulos (\~2,6%), a mediana mantém consistência sem perda.  |
| Remover variável            | `sex`               | Informação sensível com restrições éticas e legais em modelos de crédito.              |
| Manter variável             | `user_id`           | Identificador único. Essencial para realizar junções (`JOINs`).                        |

🔧 Tratamento da Tabela loans_outstanding

| **Ação**                       | **Variável**           | **Justificativa**                                                                     |
| ------------------------------ | ---------------------- | ------------------------------------------------------------------------------------- |
| Padronizar valores categóricos | `loan_type`            | Corrigir inconsistências de escrita (ex: "REAL ESTATE", "others") e unificar valores. |
| Criar variável derivada        | `loan_count`           | Total de empréstimos ativos por cliente. Indica perfil de contratação.                |
| Criar variável derivada        | `count_real_estate`    | Quantidade de empréstimos do tipo **real estate**.                                    |
| Criar variável derivada        | `count_other`          | Quantidade de empréstimos do tipo **other**.                                          |
| Criar variável binária         | `has_real_estate_loan` | Indica presença (1) ou ausência (0) de empréstimos do tipo **real estate**.           |
| Criar variável binária         | `has_other_loan`       | Indica presença (1) ou ausência (0) de empréstimos do tipo **other**.                 |
| Manter variável                | `user_id`              | Identificador único. Utilizado para integração entre tabelas.                         |

🔧 Tratamento da Tabela loans_detail

| **Ação**         | **Variável**                                   | **Justificativa**                                                                        |
| ---------------- | ---------------------------------------------- | ---------------------------------------------------------------------------------------- |
| Remover variável | `number_times_delayed_payment_loan_30_59_days` | Correlação muito alta com outras variáveis de atraso (corr > 0,98). Redundante.          |
| Remover variável | `number_times_delayed_payment_loan_60_89_days` | Correlação quase perfeita com outras variáveis (corr > 0,99). Pode causar colinearidade. |
| Manter variável  | `debt_ratio`                                   | Correlação baixa com inadimplência, mas mantida para avaliação posterior.                |
| Manter variável  | `more_90_days_overdue`                         | Indicador relevante com correlação moderada com inadimplência (corr ≈ 0,31).             |
| Manter variável  | `using_lines_not_secured_personal_assets`      | Representa uso de linhas de crédito rotativo. Pode contribuir à previsão.                |
| Manter variável  | `user_id`                                      | Identificador único para junção das tabelas.                                             |

🔧 Tratamento da Tabela default

- A tabela foi mantida integralmente, com as variáveis user_id e default_flag, que são essenciais para a etapa de modelagem preditiva. Não houve necessidade de transformação ou limpeza nesta tabela.