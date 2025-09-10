### 🟪 Fazer uma análise exploratória

O uso de ferramentas de visualização na etapa de análise exploratória nos ajuda a entender os dados com mais facilidade e de forma visual. Neste projeto utilizaremos a ferramenta Looker Studio para realizar esta tarefa.

#### 🟣 Agrupar dados de acordo com variáveis ​​categóricas

Objetivo: Use tabelas no Looker Studio para resumir dados em variáveis ​​categóricas

✅ Empréstimos imobiliários (has_real_estate_loan)

| Possui Empréstimo? | Clientes | Inadimplência (%) | Salário Médio | Debt Ratio Médio |
| ------------------ | -------- | ----------------- | ------------- | ---------------- |
|   Não              | 13.544   | **2,87%**         | R\$ 4.755,56  | 86,96            |
|   Sim              | 22.456   | **1,31%**         | R\$ 6.767,06  | 511,19           |

Clientes com empréstimo imobiliário apresentam inadimplência menor e maior renda média. Apesar do alto debt ratio, esses clientes demonstram maior capacidade e histórico de pagamento, o que pode indicar perfis mais confiáveis.

✅ Outros tipos de empréstimos (has_other_loan)

| Possui Empréstimo? | Clientes | Inadimplência (%) | Salário Médio | Debt Ratio Médio |
| ------------------ | -------- | ----------------- | ------------- | ---------------- |
|   Não              | 515      | **12,43%**        | R\$ 4.249,74  | 133,48           |
|   Sim              | 35.485   | **1,74%**         | R\$ 6.035,84  | 354,75           |

A inadimplência entre quem não tem outros empréstimos é quase 7 vezes maior, possivelmente por serem clientes com menor acesso a crédito ou maior risco percebido. O tamanho reduzido da amostra (apenas 515 clientes) também deve ser considerado.

✅ Número de dependentes (dependents)

| Dependentes | Clientes | Inadimplência (%) | Salário Médio | Debt Ratio Médio |
| ----------- | -------- | ----------------- | ------------- | ---------------- |
| 0           | 21.855   | **1,59%**         | R\$ 5.446,66  | 435,07           |
| 1           | 6.198    | **2,18%**         | R\$ 6.620,14  | 173,62           |
| 2           | 7.947    | **2,53%**         | R\$ 7.084,67  | 260,78           |

À medida que o número de dependentes cresce, a inadimplência também aumenta, apesar da renda maior. Isso pode estar associado a maiores pressões financeiras em famílias maiores, impactando sua capacidade de honrar dívidas.

✅ Quantidade de Empréstimos (loan_count)

| Faixa de Empréstimos  | Total de Clientes | Inadimplência (%) | Salário Médio | Debt Ratio Médio |
| --------------------- | ----------------- | ----------------- | ------------- | ---------------- |
| 0 empréstimos         | 425               | **14,35%**        | R\$ 4.093,96  | 20,25            |
| 1 empréstimo          | 1.065             | **2,82%**         | R\$ 4.056,58  | 75,37            |
| 2 empréstimos         | 1.581             | **3,61%**         | R\$ 4.326,36  | 155,79           |
| 3 ou mais empréstimos | 32.929            | **1,62%**         | R\$ 6.179,06  | 374,19           |

A inadimplência entre quem não possui empréstimos é extremamente alta (14,35%), sugerindo ausência de histórico de crédito ou perfil de risco elevado. Por outro lado, clientes com 3 ou mais empréstimos têm melhor desempenho, indicando que acesso ao crédito é um sinal de confiabilidade.

#### 🟣 Ver variáveis ​​categóricas

Objetivo: Use gráficos de barras no Looker Studio para visualizar variáveis ​​categóricas.

✅ 1. Inadimplência (%) por Empréstimo Imobiliário

| Possui Empréstimo? | Clientes | Inadimplência (%) |
| ------------------ | -------- | ----------------- |
| Não                | 13.544   | 2,87              |
| Sim                | 22.456   | 1,31              |

✅ 2. Inadimplência (%) por Outros Empréstimos

| Possui Empréstimo? | Clientes | Inadimplência (%) |
| ------------------ | -------- | ----------------- |
| Não                | 515      | 12,43             |
| Sim                | 35.485   | 1,74              |

✅ 3. Inadimplência (%) por Número de Dependentes

| Número de Dependentes | Clientes | Inadimplência (%) |
| --------------------- | -------- | ----------------- |
| 0                     | 21.855   | 1,59              |
| 1                     | 6.198    | 2,18              |
| 2                     | 7.947    | 2,53              |

✅ 4. Inadimplência (%) por Faixa de Empréstimos

| Faixa de Empréstimos  | Clientes | Inadimplência (%) |
| --------------------- | -------- | ----------------- |
| 0 empréstimos         | 425      | 14,35             |
| 1 empréstimo          | 1.065    | 2,82              |
| 2 empréstimos         | 1.581    | 3,61              |
| 3 ou mais empréstimos | 32.929   | 1,62              |

#### 🟣 Aplicar medidas de tendência central (moda, média, mediana)

Objetivo: Use as opções da tabela para calcular estatísticas descritivas para ajudar a compreender a distribuição dos dados

✅ 1. Empréstimo Imobiliário (has_real_estate_loan)

| Possui Empréstimo Imobiliário? | Média Salarial (R\$) | Mediana Salarial (R\$) | Moda Dependentes |
| ------------------------------ | -------------------- | ---------------------- | ---------------- |
| Não                            | 4.755,56             | 5.000,00               | 0                |
| Sim                            | 6.767,06             | 5.400,00               | 0                |

Clientes com empréstimo imobiliário têm salário mais alto e inadimplência mais baixa. A moda dos dependentes sendo 0 indica que a maioria dos clientes não tem filhos ou dependentes, reforçando um perfil de menor carga financeira.

✅ 2. Moda, Média e Mediana por Outros Tipos de Empréstimos (has_other_loan)

| Possui Outros Empréstimos? | Média Salarial (R\$) | Mediana Salarial (R\$) | Média Debt Ratio | Mediana Debt Ratio | Moda Dependentes |
| -------------------------- | -------------------- | ---------------------- | ---------------- | ------------------ | ---------------- |
| Não                        | R\$ 4.249,74         | R\$ 5.400,00           | 133,48           | **0,0051**         | 0                |
| Sim                        | R\$ 6.035,84         | R\$ 5.400,00           | 354,75           | **0,3640**         | 0                |

Clientes com outros empréstimos possuem salários significativamente maiores e maior comprometimento da renda, mas ainda assim são menos inadimplentes. Isso reforça que acesso a crédito e capacidade de manter múltiplas dívidas pode ser sinal de confiança do mercado

✅ 3. Moda, Média e Mediana por Faixa de Empréstimos (loan_count)

| Faixa de Empréstimos  | Média Salarial (R\$) | Mediana Salarial (R\$) | Média Debt Ratio | Mediana Debt Ratio | Moda Dependentes |
| --------------------- | -------------------- | ---------------------- | ---------------- | ------------------ | ---------------- |
| 0 empréstimos         | R\$ 4.093,96         | R\$ 4.700,00           | 20,25            | **0,0000**         | 0                |
| 1 empréstimo          | R\$ 4.056,58         | R\$ 4.346,00           | 75,37            | **0,0632**         | 0                |
| 2 empréstimos         | R\$ 4.326,36         | R\$ 4.790,00           | 155,79           | **0,1308**         | 0                |
| 3 ou mais empréstimos | R\$ 6.179,06         | R\$ 5.400,00           | 374,19           | **0,3858**         | 0                |

Conforme o número de empréstimos aumenta, a renda média também aumenta, assim como o debt ratio. No entanto, clientes com mais empréstimos têm melhor desempenho em inadimplência, mostrando que múltiplos empréstimos podem indicar perfil financeiro mais sólido e confiável.

✅ 4. Moda, Média e Mediana por Número de Dependentes

| Número de Dependentes | Total de Clientes | Média Salarial (R\$) | Mediana Salarial (R\$) | Média Debt Ratio | Mediana Debt Ratio |
| --------------------- | ----------------- | -------------------- | ---------------------- | ---------------- | ------------------ |
| 0                     | 21.855            | R\$ 5.446,66         | R\$ 5.400,00           | 435,07           | 0,3954             |
| 1                     | 6.198             | R\$ 6.620,14         | R\$ 5.523,00           | 173,62           | 0,3306             |
| 2                     | 7.947             | R\$ 7.084,67         | R\$ 6.000,00           | 260,78           | 0,3519             |

Clientes com mais dependentes têm renda mais alta, o que pode refletir maior responsabilidade familiar. Entretanto, também apresentam maior inadimplência, sugerindo que a pressão financeira de sustentar mais pessoas impacta negativamente a capacidade de pagamento. A mediana do debt ratio é menor nos grupos com dependentes, indicando que a maioria desses clientes mantém comprometimento moderado da renda, apesar de casos extremos no grupo sem dependentes.

#### 2.2.4 🟣 Ver distribuição

Objetivo: Use histograma e boxplot no LookerStudio para exibir variáveis ​​numéricas.

✅ 1. Histograma com faixas fixas (bin de R$1000)

| Faixa Salarial (R\$) | Frequência | Percentual (%) |
| -------------------- | ---------- | -------------- |
| 0 – 999              | 1.047      | 2,91%          |
| 1.000 – 1.999        | 1.587      | 4,41%          |
| 2.000 – 2.999        | 2.999      | 8,33%          |
| 3.000 – 3.999        | 3.560      | 9,89%          |
| 4.000 – 4.999        | 3.492      | 9,70%          |
| 5.000 – 5.999        | 10.577     | 29,38%         |
| 6.000 – 6.999        | 2.710      | 7,53%          |
| 7.000 – 7.999        | 2.163      | 6,01%          |
| 8.000 – 8.999        | 1.855      | 5,15%          |
| 9.000 – 9.999        | 1.268      | 3,52%          |
| 10.000 – 10.999      | 1.314      | 3,65%          |
| 11.000 – 11.999      | 729        | 2,02%          |
| 12.000 – 12.999      | 643        | 1,79%          |
| 13.000 – 13.999      | 393        | 1,09%          |
| 14.000 – 14.999      | 279        | 0,78%          |
| 15.000+              | 1.384      | 3,84%          |

Maior concentração entre R$5.000 e R$6.000, que representa quase 30% dos clientes. A distribuição tem cauda longa à direita (distribuição assimétrica positiva).

✅ 2. Histograma por Quantis (aproximadamente 10 faixas com clientes equilibrados)

| Faixa Salarial (Quantil) | Nº Clientes |
| ------------------------ | ----------- |
| \[0, 2.300)              | 3.591       |
| \[2.300, 3.364)          | 3.626       |
| \[3.364, 4.333)          | 3.585       |
| \[4.333, 5.400)          | 10.941      |
| \[5.400, 5.400)          | 14.548      |
| \[5.400, 6.600)          | 10.850      |
| \[6.600, 8.240)          | 3.614       |
| \[8.240, 10.804)         | 3.691       |
| \[10.804, 15.510,5)      | 3.582       |

A faixa exatamente em R$5.400 concentra 14.548 clientes, sugerindo que muitos salários estão "congelados" ou "padronizados" neste valor.

✅ 3. Estatísticas para Boxplot (salário)

| Mínimo | Q1       | Mediana  | Q3       | Máximo       |
| ------ | -------- | -------- | -------- | ------------ |
| R\$0   | R\$3.900 | R\$5.400 | R\$7.358 | R\$15.510,50 |

A mediana está mais próxima do Q1 → distribuição ligeiramente assimétrica à direita O valor máximo é quase 4x a mediana, indicando presença de outliers/salários bem acima da média.

✅ 4. Distribuição de Salário por Quantis (8 Faixas)

| Faixa Salarial (R\$) | Número de Clientes | Percentual (%) |
| -------------------- | ------------------ | -------------- |
| (-∞, 2.300)          | 3.508              | 9,75%          |
| \[2.300, 3.400)      | 3.681              | 10,23%         |
| \[3.400, 4.340)      | 3.547              | 9,86%          |
| \[4.340, 5.400)      | 3.596              | 10,00%         |
| \[5.400, 6.612)      | 10.856             | 30,19%         |
| \[6.612, 8.275)      | 3.583              | 9,97%          |
| \[8.275, 10.788)     | 3.626              | 10,08%         |
| \[10.788, +∞)        | 3.603              | 10,01%         |

A distribuição revela forte concentração na faixa entre R$5.400 e R$6.612, sugerindo que muitos clientes têm salários padronizados — possivelmente por atuarem no setor público ou por convenções corporativas. As demais faixas apresentam distribuição equilibrada, com cerca de 10% cada, o que mostra diversidade na base de clientes.

#### 2.2.5 🟣 Aplicar medidas de dispersão (desvio padrão)

Objetivo: Use tabelas no Looker Studio para calcular o desvio padrão

✅ 1. Por Empréstimo Imobiliário (has_real_estate_loan)

| Possui Empréstimo Imobiliário? | Desvio Padrão Salário (R\$) | Desvio Padrão Debt Ratio |
| ------------------------------ | --------------------------- | ------------------------ |
| Não                            | 2.702,63                    | 716,77                   |
| Sim                            | 3.504,27                    | 2.471,82                 |

Clientes com empréstimo imobiliário têm maior variação salarial e muito maior dispersão no debt ratio. Isso indica que, embora sejam em média mais estáveis, há perfis muito diversos nesse grupo — possivelmente pessoas com grande capacidade de crédito convivendo com perfis mais arriscados.

✅ 2. Por Outros Tipos de Empréstimos (has_other_loan)

| Possui Outros Empréstimos? | Desvio Padrão Salário (R\$) | Desvio Padrão Debt Ratio |
| -------------------------- | --------------------------- | ------------------------ |
| Não                        | 2.252,95                    | 449,72                   |
| Sim                        | 3.376,82                    | 2.025,28                 |

Clientes com outros empréstimos têm maior dispersão em ambos os indicadores, sugerindo que o grupo é mais heterogêneo — com perfis de alto e baixo risco misturados. Já quem não tem outros empréstimos tende a ter renda mais concentrada em uma faixa específica e menor variação no comprometimento da renda.

✅ 3. Por Número de Dependentes

| Número de Dependentes | Desvio Padrão Salário (R\$) | Desvio Padrão Debt Ratio |
| --------------------- | --------------------------- | ------------------------ |
| 0                     | 2.975,26                    | 1.328,45                 |
| 1                     | 3.629,58                    | 856,12                   |
| 2                     | 3.809,84                    | 3.585,51                 |

À medida que o número de dependentes aumenta, a dispersão salarial cresce, indicando maior desigualdade de renda entre clientes com dependentes. O desvio padrão do debt ratio salta drasticamente em quem tem 2 dependentes, o que pode apontar para um subgrupo com grande dificuldade financeira.

✅ 4. Por Faixa de Empréstimos (loan_count)
| Faixa de Empréstimos  | Desvio Padrão Salário (R\$) | Desvio Padrão Debt Ratio |
| --------------------- | --------------------------- | ------------------------ |
| 0 empréstimos         | 2.163,06                    | 92,06                    |
| 1 empréstimo          | 2.477,22                    | 282,94                   |
| 2 empréstimos         | 2.552,92                    | 475,81                   |
| 3 ou mais empréstimos | 3.392,60                    | 2.098,64                 |

Conforme aumenta o número de empréstimos, a variabilidade do perfil financeiro cresce bastante — especialmente em debt ratio. Isso mostra que, apesar da maior confiança do mercado nesses clientes (como visto em análises anteriores), há também mais riscos embutidos, com perfis extremos tanto positivos quanto negativos.

#### 🟣 Calcular quartis, decis ou percentis

Objetivo: Calcular quartis para variáveis ​​de risco relativo no BigQuery

1. Tabela de Decis de Salário vs Inadimplência

| Decil Salário | Total Clientes | Inadimplentes | Taxa Inadimplência (%) | Salário Mínimo | Salário Máximo |
| ------------- | -------------- | ------------- | ---------------------- | -------------- | -------------- |
| 1             | 3600           | 96            | 2.67                   | 0.0            | 2306.0         |
| 2             | 3600           | 126           | 3.5                    | 2308.0         | 3400.0         |
| 3             | 3600           | 87            | 2.42                   | 3400.0         | 4369.0         |
| 4             | 3600           | 88            | 2.44                   | 4369.0         | 5400.0         |
| 5             | 3600           | 79            | 2.19                   | 5400.0         | 5400.0         |
| 6             | 3600           | 38            | 1.06                   | 5400.0         | 5400.0         |
| 7             | 3600           | 67            | 1.86                   | 5400.0         | 6623.0         |
| 8             | 3600           | 53            | 1.47                   | 6623.0         | 8300.0         |
| 9             | 3600           | 27            | 0.75                   | 8300.0         | 10800.0        |
| 10            | 3600           | 22            | 0.61                   | 10800.0        | 15510.5        |

Insights Decis de Salário: O risco de inadimplência é mais alto nos primeiros decis (principalmente no 2º decil, 3,5%). Conforme o salário aumenta, a inadimplência diminui consistentemente, chegando a 0,61% no último decil. Isso sugere que salários mais altos indicam menor risco de inadimplência.

2. Tabela de Percentis de Debt Ratio vs Inadimplência

| Percentil Debt Ratio | Total Clientes | Inadimplentes | Taxa Inadimplência (%) | Debt Ratio Mínimo | Debt Ratio Máximo |
| -------------------- | -------------- | ------------- | ---------------------- | ----------------- | ----------------- |
| 1                    | 360            | 32            | 8.89                   | 0.0               | 0.0               |
| 2                    | 360            | 1             | 0.28                   | 0.0               | 0.0               |
| 3 a 69               | 360            | 0 a 14        | 0.0 a 3.89             | Varia             | Varia             |
| 70                   | 360            | 14            | 3.89                   | 0.626             | 0.651             |
| 79                   | 360            | 17            | 4.72                   | 1.309             | 2.0               |
| 76                   | 360            | 16            | 4.44                   | 0.874             | 0.959             |
| 100                  | 360            | 3             | 0.83                   | 4930              | 307001            |

O grupo com debt_ratio zero (1º percentil) tem a maior taxa de inadimplência (8,89%). Percentis intermediários apresentam taxas baixíssimas ou zero. A partir do percentil 70, a inadimplência começa a subir novamente, atingindo até 4,72% no percentil 79. Indica uma relação não linear: tanto dívida quase zero quanto dívida muito alta implicam maior risco. Clientes com dívida muito alta (últimos percentis) não necessariamente têm inadimplência alta (ex: percentil 100 tem 0,83%).

📊 Insights a partir dos decis de salário e inadimplência

| Decil | Faixa Salarial (mín - máx) | Taxa de Inadimplência (%) | Insight                                                               |
| ----- | -------------------------- | ------------------------- | --------------------------------------------------------------------- |
| 1     | 0.0 - 2306.0               | 2.67                      | Taxa mais alta, público com salário baixo tem maior inadimplência.    |
| 2     | 2308.0 - 3400.0            | 3.50                      | Aumenta ainda mais, possível vulnerabilidade alta nesta faixa.        |
| 3     | 3400.0 - 4369.0            | 2.42                      | Queda na inadimplência, renda começa a proteger contra inadimplência. |
| 4     | 4369.0 - 5400.0            | 2.44                      | Quase estável em relação ao decil anterior.                           |
| 5     | 5400.0 - 5400.0            | 2.19                      | Pequena queda, salário constante.                                     |
| 6     | 5400.0 - 5400.0            | 1.06                      | Queda significativa, indica melhora da capacidade de pagamento.       |
| 7     | 5400.0 - 6623.0            | 1.86                      | Pequeno aumento, avaliar fatores externos.                            |
| 8     | 6623.0 - 8300.0            | 1.47                      | Tendência de queda, maior renda tende a proteger.                     |
| 9     | 8300.0 - 10800.0           | 0.75                      | Baixa inadimplência, perfil mais seguro.                              |
| 10    | 10800.0 - 15510.5          | 0.61                      | Menor inadimplência, grupo mais estável financeiramente.              |

💡 Possíveis ações: Foco nas faixas 1 e 2: A inadimplência é maior, sinal que estratégias específicas de prevenção de risco devem ser aplicadas para esses grupos. Segmentação por faixa salarial: Poder criar políticas diferenciadas de crédito e acompanhamento para os grupos com maior risco. Monitoramento da faixa 7: Pequena alta de inadimplência após queda, vale investigar causas (ex: perfil de empréstimos, dívidas anteriores, variáveis externas). Uso de faixas para modelagem de risco: O decil salarial pode ser uma variável categórica importante para modelos preditivos de inadimplência.

#### 2.2.7🟣 Calcular correlação entre variáveis ​​numéricas

Objetivo: Compreender a relação que existe entre variáveis ​​numéricas através de correlações. Use gráficos de dispersão e linhas de tendência. Você também pode usar o comando CORR no BigQuery




SELECT
  user_id,
  debt_ratio,
  CASE
    WHEN debt_ratio BETWEEN 0.0 AND 0.1 THEN 'Muito baixo (0-10%)'
    WHEN debt_ratio > 0.1 AND debt_ratio <= 0.3 THEN 'Baixo (10-30%)'
    WHEN debt_ratio > 0.3 AND debt_ratio <= 0.5 THEN 'Moderado (30-50%)'
    WHEN debt_ratio > 0.5 AND debt_ratio <= 0.7 THEN 'Alto (50-70%)'
    WHEN debt_ratio > 0.7 AND debt_ratio <= 1.0 THEN 'Muito alto (70-100%)'
    ELSE 'Extremo (>100%)'
  END AS categoria_debt_ratio
FROM
  `projeto-risco-relativo-470919.bancoSuperCaja.base_unificada