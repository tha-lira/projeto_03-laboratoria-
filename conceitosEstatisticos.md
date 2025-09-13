# 📊 Conceitos Estatísticos: Risco Relativo, Segmentação por Score e Regressão Logística

---

## 🔹 1. Risco Relativo (RR)

### ✅ O que é?

O **risco relativo** compara a **probabilidade de um evento acontecer em dois grupos diferentes**. Muito usado em estudos de epidemiologia, crédito e fraude.

### ✅ Fórmula:

RR = P(evento | grupo 1) / P(evento | grupo 2)

### ✅ Exemplo:

Suponha que estamos avaliando dois grupos de clientes:

- **Grupo 1**: Clientes com score baixo  
- **Grupo 2**: Clientes com score alto

| Grupo        | Inadimplentes | Total | Probabilidade |
|--------------|---------------|-------|----------------|
| Score Baixo  | 30            | 100   | 30%           |
| Score Alto   | 10            | 100   | 10%           |

RR = 0,30 / 0,10 = 3,0

**Interpretação:** clientes com score baixo têm **3x mais risco de inadimplência** comparado aos com score alto.

---

## 🔹 2. Segmentação por Score

### ✅ O que é?

É a divisão de uma população em **grupos (ou segmentos)** com base em uma **pontuação preditiva (score)**.

Aplicações:
- **Crédito:** Score de risco de inadimplência
- **Marketing:** Score de propensão à compra
- **Saúde:** Score de risco de doença

### ✅ Como fazer?

1. **Construir um score preditivo** (ex: com regressão logística)
2. **Ordenar a base** de acordo com esse score
3. **Dividir em grupos (faixas, decis, quintis, etc.)**

### ✅ Exemplo:

Você pode dividir os clientes em 5 grupos de score (quintis):

| Faixa de Score | % Inadimplência |
|----------------|------------------|
| 0–200          | 35%             |
| 201–400        | 25%             |
| 401–600        | 15%             |
| 601–800        | 8%              |
| 801–1000       | 2%              |

**Interpretação:** à medida que o score aumenta, o risco diminui.

---

## 🔹 3. Regressão Logística

### ✅ O que é?

É um modelo estatístico usado para prever **probabilidades de eventos binários** (ex: "vai pagar" ou "não vai pagar").

### ✅ Fórmula:

A saída da regressão logística é a **probabilidade de ocorrência do evento**:

P(y = 1) = 1 / [1 + exp(-(b0 + b1*x1 + b2*x2 + ... + bn*xn))]

Onde:

- P(y = 1) é a probabilidade do evento ocorrer (por exemplo, inadimplência)

- b0 é o intercepto

- b1, b2, ..., bn são os coeficientes do modelo

- x1, x2, ..., xn são as variáveis independentes

### ✅ Exemplo:

Modelo hipotético:

P(inadimplência) = 1 / [1 + exp(-(-2 + 0.01*renda - 0.5*idade))]


Para um cliente com:
- **renda = 3000**
- **idade = 30**

Substituindo:

P = 1 / [1 + exp(-(-2 + 0.01*3000 - 0.5*30))]
P = 1 / [1 + exp(-13)] ≈ 0.9999977


**Resultado:** altíssima chance de inadimplência.

### ✅ Como calcular na prática?

Usando Python com `scikit-learn`:

```
python
from sklearn.linear_model import LogisticRegression
model = LogisticRegression()
model.fit(X, y)  # X = variáveis preditoras, y = 0 ou 1 (ex: inadimplente ou não)
```

---

✅ Resumo:

| Conceito                  | Para que serve                          | Como calcular                                               |
| ------------------------- | --------------------------------------- | ----------------------------------------------------------- |
| **Risco Relativo (RR)**   | Comparar riscos entre dois grupos       | $RR = P1 / P2$                                              |
| **Segmentação por Score** | Dividir população por faixa de risco    | Criar score, ordenar e segmentar em grupos (quintis, decis) |
| **Regressão Logística**   | Estimar probabilidade de evento binário | Fórmula logística / modelo com Python, R ou Excel           |

