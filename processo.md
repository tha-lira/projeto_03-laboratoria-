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

Foi realizada uma análise detalhada para identificar valores nulos nas tabelas `user_info`, `loans_outstanding`, `loans_detail` e `default`. Para isso, foram aplicadas consultas SQL utilizando a função `COUNTIF()` com o operador `IS NULL` para quantificar os registros ausentes em cada coluna.

Essa análise revelou que apenas duas variáveis da tabela **user_info** apresentam valores nulos relevantes:

- **last_month_salary**: 7.199 registros nulos, aproximadamente 20% do total;

- **number_dependents**: 943 registros nulos, cerca de 2,6%.

As demais colunas nas outras tabelas analisadas não apresentaram registros nulos, indicando dados completos que não necessitam de tratamento adicional.

---

##### Análise da proporção de valores nulos em `last_month_salary` segmentada pelo perfil de inadimplência

Para entender se a ausência de informações no campo `last_month_salary` está associada à inadimplência, foi realizada uma junção com a tabela default utilizando `LEFT JOIN` para agrupar os dados por `default_flag` e calcular a proporção de valores nulos em cada grupo.

Os resultados indicaram que:

- A proporção de valores nulos entre inadimplentes (default_flag = 1) é de aproximadamente 19,03%;
- Entre bons pagadores (default_flag = 0), o percentual é de cerca de 20,02%.

Essa distribuição semelhante sugere que a ausência do dado de salário não está correlacionada com a inadimplência.

---

##### Cálculo da mediana para imputação dos valores nulos

Para substituir os valores ausentes de forma adequada, foi calculada a mediana da variável `last_month_salary` considerando apenas os registros não nulos:
Com base nessa mediana, os valores nulos serão imputados, preservando a distribuição original dos dados e garantindo a integridade da análise.

#### 🔵 Identificar e tratar valores duplicados

Nenhuma das tabelas apresentou registros duplicados que exigissem tratamento ou exclusão. Todos os dados estão consistentes com a estrutura esperada de cada tabela. A checagem de duplicatas foi feita com GROUP BY, HAVING, e validações específicas por tabela, garantindo a integridade das informações para as próximas etapas da análise.

🛠️ Nenhuma ação corretiva foi necessária para valores duplicados.

#### 🔵 Identificar e gerenciar dados fora do escopo de análise

Para compreender melhor as relações entre variáveis numéricas e auxiliar na seleção das variáveis mais relevantes para a modelagem, foram realizadas análises de correlação utilizando a função CORR().

| Variáveis Comparadas                                        | Correlação | Interpretação                            |
| ----------------------------------------------------------- | ---------- | ---------------------------------------- |
| `more_90_days_overdue` × `number_times_delayed_30_59`       | **0.98**   | Altamente correlacionadas                |
| `more_90_days_overdue` × `number_times_delayed_60_89`       | **0.99**   | Altamente correlacionadas                |
| `number_times_delayed_30_59` × `number_times_delayed_60_89` | **0.99**   | Altamente correlacionadas                |
| `debt_ratio` × `using_lines_not_secured_personal_assets`    | **0.015**  | Sem correlação                           |

🛠️  Variáveis Eliminadas

| Variável                                       | Motivo da Remoção                                                               |
| ---------------------------------------------- | ------------------------------------------------------------------------------- |
| `sex`                                          | Informação sensível – exclusão por **ética/legalidade** em modelos de crédito   |
| `number_times_delayed_payment_loan_30_59_days` | Alta correlação com outras variáveis de atraso – **redundância** (corr > 0,98)  |
| `number_times_delayed_payment_loan_60_89_days` | Alta correlação com outras variáveis de atraso – **redundância** (corr > 0,99)  |

#### 🔵 Identificar e tratar dados discrepantes em variáveis ​​categóricas

Foi utilizado a função DISTINCT, para encontar inconsistencias de escritacnas variaveis catecoricas. com isso foram identificadas inconsistências nos valores registrados. Por exemplo, na variável **loan_type**, foram encontradas variações como "OTHER", "Other", "others" que foram unificadas em "other", e "REAL ESTATE", "Real Estate", "real estate". A padronização desses valores foi realizada para garantir a uniformidade dos dados, evitar duplicidades e permitir análises e modelagens mais precisas e confiáveis.

#### 🔵 Identificar e tratar dados discrepantes em variáveis ​​numéricas

A etapa seguinte focou na detecção de valores extremos (outliers) nas variáveis numéricas presentes nas tabelas. Para isso, foram utilizadas as funções:

- APPROX_QUANTILES() – para cálculo dos quartis (Q1, Q2 e Q3),
- AVG() – para obter a média,
- STDDEV() – para o desvio padrão.

Com base nos quartis, foi calculado o Intervalo Interquartílico (IQR), definido como:

- IQR = Q3 – Q1

A partir disso, foram estabelecidos os limites para detecção de outliers:

- Limite inferior = Q1 – 1,5 × IQR
- Limite superior = Q3 + 1,5 × IQR

Qualquer valor fora desse intervalo foi considerado um outlier.

📊 Tabela user_info

| Variável            | Tratamento aplicado                                            |
| ------------------- | -------------------------------------------------------------- |
| `age`               | Valores acima de 96 foram **winsorizados para 96**             |
| `last_month_salary` | Valores acima de 15.510,5 foram **winsorizados para 15.510,5** |
| `number_dependents` | Valores acima de 2,5 foram **winsorizados para 2**             |

📊 Tabela loans_detail

| Variável                                       | Tratamento aplicado                                      |
| ---------------------------------------------- | -------------------------------------------------------- |
| `more_90_days_overdue`                         | Valores acima de 0 foram **winsorizados para 0**         |
| `using_lines_not_secured_personal_assets`      | Valores acima de 1,319 foram **winsorizados para 1,319** |
| `number_times_delayed_payment_loan_30_59_days` | Valores acima de 0 foram **winsorizados para 0**         |
| `number_times_delayed_payment_loan_60_89_days` | Valores acima de 0 foram **winsorizados para 0**         |
| `debt_ratio`                                   | Valores acima de 1,968 foram **winsorizados para 1,968** |

Essa abordagem permitiu a padronização da análise estatística das variáveis e a identificação objetiva dos outliers, apoiando decisões de tratamento como winsorização (substituição de valores extremos pelos limites superiores/inferiores).

[Consulta detalhada das variáveis](https://github.com/tha-lira/projeto_03-laboratoria-/blob/main/dados_discrepantes.md)

#### 🔵 Criar novas variáveis

Com base na tabela loans_outstanding, foi observado que cada cliente (user_id) pode possuir múltiplos empréstimos registrados, o que gera repetição de linhas para o mesmo indivíduo. Para transformar essa estrutura em algo mais útil para modelagem preditiva, foram criadas novas variáveis agregadas por cliente, permitindo capturar comportamentos relevantes de contratação de crédito.

🆕 Novas variáveis criadas:

| Variável               | Descrição                                                              |
| ---------------------- | ---------------------------------------------------------------------- |
| `loan_count`           | Quantidade total de empréstimos realizados por um cliente              |
| `count_real_estate`    | Quantidade de empréstimos do tipo **"real estate"**                    |
| `count_other`          | Quantidade de empréstimos do tipo **"other"**                          |
| `has_real_estate_loan` | Indicador binário: **1** se o cliente possui ao menos um "real estate" |
| `has_other_loan`       | Indicador binário: **1** se o cliente possui ao menos um "other"       |


Essas variáveis foram derivadas utilizando funções de agregação como COUNT(), SUM() com cláusulas CASE WHEN, e foram salvas em uma nova tabela chamada loans_features. O objetivo é enriquecer o conjunto de dados com características comportamentais dos clientes em relação ao uso de crédito, as quais serão utilizadas na modelagem de risco de inadimplência.

#### 🔵 Unir tabelas

Foi criada a tabela base_unificada, consolidando os dados tratados das tabelas user_info_tratada, loans_outstanding_tratada, loans_detail_tratada e default. Essa união teve como objetivo montar uma base única, consistente e preparada para a análise de risco de crédito.

A tabela **user_info_tratada** foi utilizada como base principal, à qual foram agregadas:

- Variáveis agregadas de empréstimos (da loans_outstanding_tratada), como contagem total, tipos e indicadores binários por cliente;
- Informações detalhadas de uso de crédito e atrasos (da loans_detail_tratada);
- Variável-alvo de inadimplência (default_flag, da tabela default).

Foram utilizados LEFT JOINs para preservar todos os clientes, mesmo aqueles sem registros em tabelas auxiliares. Valores nulos gerados pela ausência de correspondência foram tratados com funções como IFNULL() ou substituição por medianas/lógicos padrão.

Essa consolidação permitiu criar um retrato mais completo do comportamento financeiro dos clientes, facilitando análises estatísticas e a modelagem preditiva de inadimplência.

[Tratamento individual das tabelas](https://github.com/tha-lira/projeto_03-laboratoria-/blob/main/tratamento.md)

Tabela `base_unificada`

| Variável                 | Tipo    | Descrição                                                                     |
| ------------------------ | ------- | ----------------------------------------------------------------------------- |
| `user_id`                | INTEGER | Identificador único do cliente.                                               |
| `age`                    | INTEGER | Idade do cliente (valores extremos tratados por winsorização).                |
| `salary_last_month`      | FLOAT   | Salário no último mês (nulos imputados pela mediana).                         |
| `dependents`             | INTEGER | Número de dependentes (nulos tratados como 0, outliers winsorizados).         |
| `loan_count`             | INTEGER | Quantidade total de empréstimos ativos do cliente.                            |
| `count_real_estate`      | INTEGER | Número de empréstimos do tipo **real estate**.                                |
| `count_other`            | INTEGER | Número de empréstimos do tipo **other**.                                      |
| `has_real_estate_loan`   | INTEGER | Indicador binário se possui empréstimo **real estate** (0 = não, 1 = sim).    |
| `has_other_loan`         | INTEGER | Indicador binário se possui empréstimo **other** (0 = não, 1 = sim).          |
| `overdue_90_days`        | INTEGER | Quantidade de atrasos superiores a 90 dias (valores > 0 tratados como 1).     |
| `unsecured_credit_lines` | FLOAT   | Proporção de linhas de crédito não garantidas por ativos pessoais (tratados). |
| `debt_ratio`             | FLOAT   | Relação entre dívida total e renda mensal do cliente.                         |
| `default_flag`           | INTEGER | Indicador binário se o cliente entrou em inadimplência (1 = sim, 0 = não).    |
