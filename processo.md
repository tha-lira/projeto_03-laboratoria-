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

Foi utilizado a função DISTINCT, para encontar inconsistencias de escritacnas variaveis catecoricas. com isso foram identificadas inconsistências nos valores registrados. Por exemplo, na variável **loan_type**, foram encontradas variações como "OTHER", "Other", "others" que foram unificadas em "other", e "REAL ESTATE", "Real Estate", "real estate". A padronização desses valores foi realizada para garantir a uniformidade dos dados, evitar duplicidades e permitir análises e modelagens mais precisas e confiáveis.

#### 🔵 Identificar e tratar dados discrepantes em variáveis ​​numéricas

A etapa seguinte focou na detecção de valores extremos (outliers) nas variáveis numéricas presentes nas tabelas. Para isso, foram utilizadas as funções APPROX_QUANTILES(), AVG() e STDDEV() no Google BigQuery, permitindo o cálculo dos quartis, média e desvio padrão para cada variável.  A partir desses cálculos, foram definidos os limites inferior e superior por meio do Intervalo Interquartílico (IQR = Q3 – Q1), sendo considerados outliers os valores acima de Q3 + 1,5*IQR ou abaixo de Q1 – 1,5*IQR.

[Consulta detalhada das variáveis](https://github.com/tha-lira/projeto_03-laboratoria-/blob/main/dados_discrepantes.md)

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

[Tratamento individual das tabelas](https://github.com/tha-lira/projeto_03-laboratoria-/blob/main/tratamento.md)

Tabela `base_unificada`

| Variável                 | Tipo    | Descrição                                                                        |
| ------------------------ | ------- | -------------------------------------------------------------------------------- |
| `user_id`                | INTEGER | Identificador único do cliente.                                                  |
| `age`                    | INTEGER | Idade do cliente.                                                                |
| `salary_last_month`      | FLOAT   | Salário do cliente no último mês (valores nulos tratados pela mediana).          |
| `dependents`             | INTEGER | Número de dependentes do cliente (nulos tratados como 0).                        |
| `loan_count`             | INTEGER | Quantidade total de empréstimos ativos do cliente.                               |
| `count_real_estate`      | INTEGER | Quantidade de empréstimos do tipo **real estate**.                               |
| `count_other`            | INTEGER | Quantidade de empréstimos do tipo **other**.                                     |
| `has_real_estate_loan`   | INTEGER | Indicador binário (0/1) se o cliente possui ao menos 1 empréstimo imobiliário.   |
| `has_other_loan`         | INTEGER | Indicador binário (0/1) se o cliente possui ao menos 1 empréstimo de outro tipo. |
| `overdue_90_days`        | INTEGER | Quantidade de vezes que o cliente atrasou mais de 90 dias no pagamento.          |
| `unsecured_credit_lines` | FLOAT   | Proporção de linhas de crédito não garantidas por ativos pessoais.               |
| `debt_ratio`             | FLOAT   | Relação entre dívidas e renda mensal do cliente.                                 |
| `default_flag`           | INTEGER | Indicador binário (0/1) se o cliente entrou em default.                          |

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
