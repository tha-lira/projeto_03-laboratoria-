# 📘 Guia do Projeto 3 – Risco Relativo

### 🔹 1. Contexto e Objetivo

O banco Super Caja enfrenta aumento da demanda por crédito e precisa automatizar a análise de risco.

- Identificar perfis de clientes com maior risco de inadimplência.

- Calcular o risco relativo de grupos de clientes.

- Criar uma pontuação de crédito (score).

- Validar suas classificações com matriz de confusão.

- Montar visualizações e apresentação final.

### 🟦 Processar e preparar a base de dados

#### 🔵 Conectar/importar dados para ferramentas

☁️ ID do projeto: projeto-risco-relativo-470919
- **Descrição:** Projeto voltado para análise de risco relativo de clientes de uma instituição financeira fictícia chamada *SuperCaja*.

🗂️ Dataset: bancoSuperCaja
- **Descrição:** Dataset principal contendo informações de usuários e dados de empréstimos da SuperCaja.

📚 user_info
- Descrição: Informações cadastrais dos usuários.

📚 loans_outstanding
- Descrição: Informações resumidas sobre os empréstimos ativos dos usuários.

📚 loans_detail
- Descrição: Detalhes completos dos empréstimos solicitados, aprovados ou recusados.

📚 default
- Descrição: Informações sobre inadimplência (clientes que deram "default").

#### 🔵 Identificar e tratar valores nulos

🗂️  user_info
| Variável            | Valores Nulos | % Aprox.    | Observação                                      |
| ------------------- | ------------- | ----------- | ----------------------------------------------- |
| `last_month_salary` | 7.199         | \~20%       | Variável importante para análise de risco.      |
| `number_dependents` | 943           | Menos grave | Pode afetar avaliação de capacidade de crédito. |

- As demais colunas não possuem valores nulos (user_id, age, sex).

✅ 

🗂️  loans_outstanding
- Nenhuma coluna com valores nulos.
✅ Nenhuma ação necessária.

🗂️  loans_detail
- Nenhuma coluna com valores nulos.
✅ Nenhuma ação necessária.

🗂️  default
- Nenhuma coluna com valores nulos.
✅ Nenhuma ação necessária.

#### 🔵 Identificar e tratar valores duplicados
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
