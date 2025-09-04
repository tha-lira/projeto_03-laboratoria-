# 📘 Guia do Projeto 3 – Risco Relativo

### 🟦 Processar e preparar a base de dados

#### 🔵 Conectar/importar dados para ferramentas

- Projeto
ID: `projeto-risco-relativo-470919`
Descrição: Análise de risco relativo dos clientes da instituição financeira fictícia SuperCaja.

- Dataset Principal
Nome: `bancoSuperCaja`
Descrição: Reúne informações cadastrais dos usuários, detalhes dos empréstimos e dados de inadimplência.

- Tabelas no Dataset

1. user_info — Dados cadastrais (idade, sexo, salário, dependentes, etc.)

2. loans_outstanding — Resumo dos empréstimos ativos (tipo, identificador)

3. loans_detail — Detalhes dos empréstimos, incluindo atrasos e uso de crédito

4. default — Indicador de inadimplência (default_flag)

---

#### 🔵  Identificar e tratar valores nulos

Foi realizada uma verificação de valores nulos nas tabelas `user_info`, `loans_outstanding`, `loans_detail` e `default`, utilizando a função COUNTIF() combinada com o operador IS NULL. Essa abordagem permitiu identificar, em cada coluna, a quantidade de registros ausentes.

A análise revelou que apenas duas variáveis da tabela `user_info` apresentam valores nulos:

- **last_month_salary**: 7.199 registros nulos (~20%)

- **number_dependents**: 943 registros nulos (~2.6%)

As demais colunas das outras tabelas estão completas e não exigem tratamento adicional.

Ao fazer uma comparação entre as variáveis last_month_salary e default_flag, os dados revelaram que:

- A proporção de valores nulos entre inadimplentes (default_flag = 1) é: 130 / 683 ≈ 19,03%;

- A proporção de valores nulos entre bons pagadores (default_flag = 0) é: 7.069 / 35.317 ≈ 20,02%.

Isso nos mostra que os percentuais de nulos são quase iguais entre os dois grupos. Portanto, a ausência de salário declarado não está associada à inadimplência neste caso.

🛠️ As ações corretivas para os campos nulos listados acima serão a substituição dos valores pela mediana.

#### 🔵 Identificar e tratar valores duplicados

Nenhuma das tabelas apresentou registros duplicados que exigissem tratamento ou exclusão. Todos os dados estão consistentes com a estrutura esperada de cada tabela. A checagem de duplicatas foi feita com GROUP BY, HAVING, e validações específicas por tabela, garantindo a integridade das informações para as próximas etapas da análise.

🛠️ Nenhuma ação corretiva foi necessária para valores duplicados.

#### 🔵 Identificar e gerenciar dados fora do escopo de análise

Para compreender melhor as relações entre variáveis numéricas e auxiliar na seleção de variáveis relevantes para modelagem, foram realizadas análises de correlação utilizando a função CORR().

| Variáveis Comparadas                                                                            | Correlação |
| ----------------------------------------------------------------------------------------------- | ---------- |
| `more_90_days_overdue` × `number_times_delayed_payment_loan_30_59_days`                         | **0,98**   |
| `more_90_days_overdue` × `number_times_delayed_payment_loan_60_89_days`                         | **0,99**   |
| `number_times_delayed_payment_loan_30_59_days` × `number_times_delayed_payment_loan_60_89_days` | **0,99**   |
| `last_month_salary` × `debt_ratio`                                                              | -0,025     |
| `more_90_days_overdue` × `default_flag`                                                         | 0,31       |
| `debt_ratio` × `default_flag`                                                                   | -0,007     |

🛠️  Variáveis Eliminadas

| Variável                                       | Motivo da Remoção                                                               |
| ---------------------------------------------- | ------------------------------------------------------------------------------- |
| `sex`                                          | Informação sensível – exclusão por **ética/legalidade** em modelos de crédito   |
| `number_times_delayed_payment_loan_30_59_days` | Alta correlação com outras variáveis de atraso – **redundância** (corr > 0,98)  |
| `number_times_delayed_payment_loan_60_89_days` | Alta correlação com outras variáveis de atraso – **redundância** (corr > 0,99)  |

[Consulta detalhada](https://github.com/tha-lira/projeto_03-laboratoria-/blob/main/dados_discrepantes.md)

#### 🔵 Identificar e tratar dados discrepantes em variáveis ​​categóricas

Foi utilizado a função DISTINCT, para encontar inconsistencias de escritacnas variaveis catecoricas. com isso foram identificadas inconsistências nos valores registrados. Por exemplo, na variável **loan_type**, foram encontradas variações como "OTHER", "Other", "others" que foram unificadas em "other", e "REAL ESTATE", "Real Estate", "real estate". 

🛠️ A padronização desses valores foi realizada para garantir a uniformidade dos dados, evitar duplicidades e permitir análises e modelagens mais precisas e confiáveis.

[Consulta detalhada](https://github.com/tha-lira/projeto_03-laboratoria-/blob/main/dados_discrepantes.md)

#### 🔵 Identificar e tratar dados discrepantes em variáveis ​​numéricas

A etapa seguinte focou na detecção de valores extremos (outliers) nas variáveis numéricas presentes nas tabelas. Para isso, foram utilizadas as funções APPROX_QUANTILES(), AVG() e STDDEV() no Google BigQuery, permitindo o cálculo dos quartis, média e desvio padrão para cada variável. 

As principais descobertas foram:

📁 Tabela user_info

- **last_month_salary** apresentou valores extremamente altos, com o valor máximo de `R$ 1.560.100`, enquanto o terceiro quartil era de apenas `R$ 8.219`. Aproximadamente 1.187 registros estavam acima do limite superior (definido como 1,5 * IQR acima do Q3).

- **age** apresentou 10 registros com idade acima de `96 anos`, enquanto o Q3 era de `63`. Esses casos foram classificados como outliers superiores.

- **number_dependents** tinha valor máximo de 13, porém com uma média baixa (0,76), indicando casos isolados de dependentes muito altos.

📁 Tabela loans_detail

- **debt_ratio** apresentou forte assimetria, com média de `351,58` e valor máximo de `307.001`, sendo que o Q3 era de apenas 0,89. Foram identificados 7.575 registros acima do limite superior.

- Variáveis de atraso de pagamento como **more_90_days_overdue**, **number_times_delayed_payment_loan_30_59_days** e **number_times_delayed_payment_loan_60_89_days** apresentaram valor `máximo de 98`, com `mediana igual a zero`. Isso indica que a maioria dos clientes não teve histórico de atraso, mas uma minoria teve muitos casos — sugerindo necessidade de atenção na modelagem, pois esses valores extremos podem influenciar algoritmos de classificação.

📁 Tabela default

- A variável **default_flag** (indicadora de inadimplência) apresentou distribuição muito desbalanceada, com média de 0,019 — o que significa que apenas cerca de 1,9% da base de clientes está inadimplente. Esse desequilíbrio deve ser considerado na modelagem, para evitar viés.

[Consulta detalhada](https://github.com/tha-lira/projeto_03-laboratoria-/blob/main/dados_discrepantes.md)

#### 🔵 Criar novas variáveis

Com base na tabela loans_outstanding, foi observado que cada cliente (user_id) pode possuir múltiplos empréstimos cadastrados. A fim de tornar esses dados mais úteis para análise preditiva, foram criadas novas variáveis agregadas por cliente:

| Nova Variável          | Descrição                                                               |
| ---------------------- | ----------------------------------------------------------------------- |
| `loan_count`           | Quantidade total de empréstimos feitos por um cliente                   |
| `count_real_estate`    | Quantidade de empréstimos do tipo “real estate”                         |
| `count_other`          | Quantidade de empréstimos do tipo “other”                               |
| `has_real_estate_loan` | Indicador binário se o cliente fez ao menos um empréstimo “real estate” |
| `has_other_loan`       | Indicador binário se o cliente fez ao menos um empréstimo “other”       |

Essas variáveis foram salvas na nova tabela loans_features e poderão ser utilizadas na modelagem de risco de crédito. Elas representam o comportamento histórico do cliente em relação à contratação de empréstimos.

#### 🔵 Unir tabelas

Foi criado a tabela para união das bases `base_unificada`, foi realizado o tratamento dos valores nulos provenientes dos LEFT JOINs. Realizamos a união das tabelas user_info_tratada, loans_outstanding_tratada, loans_detail_tratada e default para criar uma base única e consistente para análise de risco. A tabela user_info serviu como base principal, onde agregamos informações dos empréstimos da tabela loans_outstanding por cliente, criando variáveis que indicam quantidade e tipos de empréstimos. Em seguida, incorporamos detalhes dos empréstimos e histórico de atrasos da tabela loans_detail e adicionamos a variável-alvo de inadimplência (default_flag) da tabela default. Utilizamos joins que preservam todos os clientes, mesmo aqueles sem registros em tabelas auxiliares, garantindo integridade e completude dos dados. Essa união permitiu consolidar o perfil financeiro dos clientes, facilitando análises e a modelagem preditiva de risco de crédito. 

🔧 Tratamento da `Tabela user_info`

| Ação                            | Variável            | Justificativa                                                                            |
| ------------------------------- | ------------------- | ---------------------------------------------------------------------------------------- |
| **Preencher nulos com mediana** | `last_month_salary` | Alta proporção de nulos (\~20%). Substituir pela mediana evita viés extremo.             |
| **Preencher nulos com mediana** | `number_dependents` | Proporção de nulos baixa (\~2,6%). A mediana mantém consistência e evita perda de dados. |
| **Remover variável**            | `sex`               | Informação sensível (ética/legal). Não deve ser usada em modelos de crédito.             |
| **Manter variável**             | `user_id`           | Necessária para realizar junções (`JOINs`) com outras tabelas posteriormente.            |

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

🔧 Tratamento da Tabela loans_detail

| **Ação**             | **Variável**                                   | **Justificativa**                                                                   |
| -------------------- | ---------------------------------------------- | ----------------------------------------------------------------------------------- |
| **Remover variável** | `number_times_delayed_payment_loan_30_59_days` | Correlação muito alta com outras variáveis de atraso (corr > 0,98). Redundante.     |
| **Remover variável** | `number_times_delayed_payment_loan_60_89_days` | Alta correlação com outras variáveis (corr > 0,99). Pode causar multicolinearidade. |
| **Remover variável** | `debt_ratio`                                   | Correlação quase nula com inadimplência (corr ≈ -0,007). Pouco valor preditivo.     |
| **Manter variável**  | `more_90_days_overdue`                         | Correlação moderada com inadimplência (corr ≈ 0,31). Indicador relevante de risco.  |
| **Manter variável**  | `using_lines_not_secured_personal_assets`      | Pode representar uso de crédito rotativo; útil na modelagem.                        |
| **Manter variável**  | `user_id`                                      | Necessária para junção com outras tabelas (JOIN).                                   |

🔧 Tratamento da Tabela default

- A tabela manteve as duas colunas originais, para uso posterior na análise preditiva.

### 🟪 Fazer uma análise exploratória

#### 🟣 Agrupar dados de acordo com variáveis ​​categóricas
#### 🟣 Visualizar variáveis ​​categóricas
#### 🟣 Aplicar medidas de tendência central
#### 🟣 Ver distribuição
#### 🟣 Aplicar medidas de dispersão
#### 🟣 Calcular quartis, decis ou percentis
#### 🟣 Calcular correlação entre variáveis

### 🟥 Aplicar técnica de análise

#### 🔴 Calcular o risco relativo
#### 🔴 Regressão logística
