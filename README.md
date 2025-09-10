# 📊 Projeto 3: Risco Relativo

### 🔷 Caso

Com a queda das taxas de juros, houve um aumento expressivo na demanda por crédito no banco **Super Caja**. Esse movimento trouxe duas consequências principais:

Sobrecarga da equipe de análise de crédito, que ainda depende de avaliações manuais.

Preocupação crescente com a inadimplência, que afeta diretamente a sustentabilidade financeira da instituição.

Diante disso, o banco busca automatizar a análise de crédito, utilizando técnicas avançadas de análise de dados. O objetivo é construir um sistema de score de crédito que permita classificar clientes em diferentes níveis de risco, reduzindo inadimplência e aumentando a eficiência operacional.


### 🎯 Objetivo

- Identificar perfis de clientes com maior risco de inadimplência.

- Calcular o risco relativo de inadimplência em diferentes segmentos (ex.: idade, renda, atrasos).

- Montar uma pontuação de crédito (score) com base nos dados.

- Construir dashboard e apresentação executiva com os resultados da análise.

### 🛠️ Ferramentas, Linguagens e Insumos

1. Ferramentas

Google BigQuery: processamento e unificação de dados (SQL).

Google Colab: análise em Python (limpeza, cálculos, modelagem).

Google Apresentações: criação de slides para comunicação dos resultados.

Google Looker Studio: visualização interativa em dashboards.

2. Linguagens

SQL para consultas no BigQuery.

Python para análises e cálculos complementares no Colab.

3. Insumos

O conjunto de dados contém informações de clientes e empréstimos, dividido em 4 tabelas:

### 📂 user_info

| Variável            | Descrição                      |
| ------------------- | ------------------------------ |
| user\_id            | Identificação única do cliente |
| age                 | Idade do cliente               |
| sex                 | Gênero do cliente              |
| last\_month\_salary | Último salário informado       |
| number\_dependents  | Número de dependentes          |

### 📂 loans_outstanding

| Variável   | Descrição                                                   |
| ---------- | ----------------------------------------------------------- |
| loan\_id   | Identificação única do empréstimo                           |
| user\_id   | Identificação do cliente                                    |
| loan\_type | Tipo de empréstimo (real estate = imóveis, others = outros) |

### 📂 loans_detail

| Variável                                            | Descrição                                 |
| --------------------------------------------------- | ----------------------------------------- |
| user\_id                                            | Identificação do cliente                  |
| more\_90\_days\_overdue                             | Nº de vezes com atraso superior a 90 dias |
| using\_lines\_not\_secured\_personal\_assets        | Uso de linhas de crédito não garantidas   |
| number\_times\_delayed\_payment\_loan\_30\_59\_days | Nº de atrasos entre 30 e 59 dias          |
| debt\_ratio                                         | Relação dívidas / ativos (endividamento)  |
| number\_times\_delayed\_payment\_loan\_60\_89\_days | Nº de atrasos entre 60 e 89 dias          |

### 📂 default

| Variável      | Descrição                                                           |
| ------------- | ------------------------------------------------------------------- |
| user\_id      | Identificação do cliente                                            |
| default\_flag | Histórico de inadimplência (1 = inadimplente, 0 = não inadimplente) |

[Processo de Limpeza de Dados](https://github.com/tha-lira/projeto_03-laboratoria-/blob/main/processo.md)