# 📘 Guia do Projeto 3 – Risco Relativo

### 🟦 Processar e preparar a base de dados

#### 🔵 Conectar/importar dados para ferramentas

##### ☁️ Projeto

- ID: **projeto-risco-relativo-470919**
- Descrição: Projeto focado na análise de risco relativo de clientes da instituição financeira fictícia SuperCaja.

---

##### 🗂️ Dataset Principal

- Nome: **bancoSuperCaja**
- Descrição: Dataset que reúne informações cadastrais dos usuários, detalhes dos empréstimos e dados de inadimplência da SuperCaja.

---

##### 📚 Tabelas no Dataset

1. **user_info**
- Informações cadastrais dos usuários (idade, gênero, salário, dependentes etc.).

2. **loans_outstanding**
- Resumo dos empréstimos ativos dos usuários (tipo, identificador, relacionamento com o cliente).

3. **loans_detail**
- Detalhes completos dos empréstimos solicitados, aprovados ou recusados (informações sobre atrasos, dívida, uso de crédito).

4. **default**
- Dados de inadimplência, indicando se o cliente deu "default" no pagamento.

---

#### 🔵 Identificar valores nulos

Foi realizada uma verificação de valores nulos nas tabelas user_info, loans_outstanding, loans_detail e default, utilizando a função COUNTIF() combinada com o operador IS NULL. Essa abordagem permitiu identificar, para cada coluna, a quantidade de registros ausentes.

A análise revelou que apenas duas variáveis da tabela user_info apresentam valores nulos:

- **last_month_salary**: 7.199 registros nulos (~20%)

- **number_dependents**: 943 registros nulos (~2.6%)

As demais colunas das outras tabelas estão completas e não exigem tratamento adicional.

Ao fazer uma comparação entre as variáveis last_month_salary e default_flag, os dados revelaram que:

- A proporção de valores nulos entre inadimplentes (default_flag = 1) é: 130 / 683 ≈ 19,03%;

- A proporção de valores nulos entre bons pagadores (default_flag = 0) é: 7.069 / 35.317 ≈ 20,02%.

Isso nos mostra que os percentuais de nulos são quase iguais entre os dois grupos. Portanto, a ausência de salário declarado não está associada à inadimplência neste caso.

✅ As ações corretivas para os campos nulos listados acima serão a substituição dos valores nulos pela mediana (5.400,0).

#### 🔵 Identificar valores duplicados

Nenhuma das tabelas apresentou registros duplicados que exigissem tratamento ou exclusão. Todos os dados estão consistentes com a estrutura esperada de cada tabela. A checagem de duplicatas foi feita com GROUP BY, HAVING, e validações específicas por tabela, garantindo a integridade das informações para as próximas etapas da análise.

✅ Nenhuma ação corretiva foi necessária para valores duplicados.

#### 🔵 Identificar e gerenciar dados fora do escopo de análise

| Variável                                       | Tabela             | Manter? ✅/❌    | Justificativa                                                                                 |
| ---------------------------------------------- | ------------------ | -------------- | --------------------------------------------------------------------------------------------- |
| `user_id`                                      | Todas              | ✅ (técnico)    | Usado para fazer **JOIN** entre tabelas. Não entra como variável preditiva.                   |
| `loan_id`                                      | loans\_outstanding | ✅ (técnico)    | Apenas identificador de empréstimo. Útil para controle ou auditoria, **não entra no modelo**. |
| `age`                                          | user\_info         | ✅              | Pode influenciar risco de crédito.                                                            |
| `sex`                                          | user\_info         | ❌              | Proibida por critérios éticos/legais (discriminação).                                         |
| `last_month_salary`                            | user\_info         | ✅              | Pode afetar capacidade de pagamento.                                                          |
| `number_dependents`                            | user\_info         | ✅              | Impacta compromissos financeiros do cliente.                                                  |
| `loan_type`                                    | loans\_outstanding | ✅ (como dummy) | Tipo de empréstimo pode afetar risco (real estate vs outros).                                 |
| `more_90_days_overdue`                         | loans\_detail      | ✅              | Forte indicador de risco (atrasos graves).                                                    |
| `number_times_delayed_payment_loan_30_59_days` | loans\_detail      | ❌              | Altamente correlacionada com `more_90_days_overdue` (r ≈ 0.98).                               |
| `number_times_delayed_payment_loan_60_89_days` | loans\_detail      | ❌              | Altamente correlacionada com `more_90_days_overdue` (r ≈ 0.99).                               |
| `debt_ratio`                                   | loans\_detail      | ✅              | Mede o nível de endividamento.                                                                |
| `using_lines_not_secured_personal_assets`      | loans\_detail      | ✅              | Mostra uso de crédito não garantido, relevante para risco.                                    |
| `default_flag`                                 | default            | ✅              | Variável alvo — indica inadimplência (usada no modelo de classificação).                      |


#### 🔵 Identificar e tratar dados discrepantes em variáveis ​​categóricas

Na análise inicial das variáveis categóricas, foram identificadas inconsistências nos valores registrados. Por exemplo, na variável loan_type, foram encontradas variações como "OTHER", "Other", "others" que foram unificadas em "other", e "REAL ESTATE", "Real Estate", "real estate" padronizadas para "real_estate". Na variável sex, os valores "F" e "M" foram mantidos, considerados consistentes para este conjunto de dados. 

✅ A padronização desses valores foi realizada para garantir a uniformidade dos dados, evitar duplicidades e permitir análises e modelagens mais precisas e confiáveis.

#### 🔵 Identificar e tratar dados discrepantes em variáveis ​​numéricas
#### 🔵 Verificar e alterar o tipo de dados
#### 🔵 Unir tabelas
#### 🔵 Criar novas variáveis
#### 🔵 Construir tabelas auxiliares

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
