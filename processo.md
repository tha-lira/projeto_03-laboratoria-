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

As demais colunas das outras tabelas estão completas e não exigem tratamento adicional. Ao fazer uma comparação entre as variaveis last_month_salary e default_flag os dados revelaram que A proporção de valores nulos entre inadimplentes (default_flag = 1) é: 130 / 683 ≈ 19,03% já A proporção de nulos entre bons pagadores (default_flag = 0) é: 7069 / 35317 ≈ 20,02% isso nos mosta que Os percentuais de nulos são quase iguais entre os dois grupos. Ausência de salário declarado não está mais associada à inadimplência neste caso.

✅ As ações corretivas para os campos nulos listados acima, sera a substiuição do valores nulos pela mediana (5400.0)

#### 🔵 Identificar valores duplicados

Nenhuma das tabelas apresentou registros duplicados que exigissem tratamento ou exclusão. Todos os dados estão consistentes com a estrutura esperada de cada tabela. A checagem de duplicatas foi feita com GROUP BY, HAVING, e validações específicas por tabela, garantindo a integridade das informações para as próximas etapas da análise.

✅ Nenhuma ação corretiva foi necessária para valores duplicados.

#### 🔵 Identificar e gerenciar dados fora do escopo de análise

#### 🔵 Identificar e tratar dados discrepantes em variáveis ​​categóricas
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
