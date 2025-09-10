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