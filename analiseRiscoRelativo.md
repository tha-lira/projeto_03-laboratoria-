### 🟥 Aplicar técnica de análise

#### 🔴 Calcular risco relativo

A seguir, apresentamos o cálculo do Risco Relativo (RR) de inadimplência com base em diferentes variáveis socioeconômicas e comportamentais. Esta análise permite identificar perfis de clientes com maior ou menor probabilidade de inadimplência.

✅ 1. Faixa Salarial

| Faixa Salarial    | Total de Clientes | Inadimplentes | Taxa de Inadimplência | Risco Relativo |
| ----------------- | ----------------- | ------------- | --------------------- | -------------- |
| Média             | 10.692            | 298           | 2,79%                 | 1,98           |
| Baixa             | 2.634             | 65            | 2,47%                 | 1,75           |
| Alta (referência) | 22.674            | 320           | 1,41%                 | 1,00           |

- Clientes com renda média têm quase 2x mais risco de inadimplência do que os de alta renda.
- Clientes com renda baixa também apresentam risco significativamente maior.
- Renda é um fator relevante para segmentação de risco.

✅ 2. Faixa Etária (age)

| Faixa Etária | Inadimplência | Risco Relativo |
| ------------ | ------------- | -------------- |
| 25-34        | 3,91%         | **4,77x**      |
| 35-44        | 3,02%         | 3,68x          |
| 18-24        | 2,14%         | 2,61x          |
| 45-54        | 2,01%         | 2,45x          |
| 55+ (ref)    | 0,82%         | 1,00x          |

- Jovens de 25 a 34 anos têm 4,77x mais chance de inadimplência que clientes com 55+.
- Risco cai com o aumento da idade.
- Idade deve ser considerada na modelagem de risco.

✅ 3. Debt Ratio (debt_ratio)
| Faixa Debt Ratio | Inadimplência | Risco Relativo |
| ---------------- | ------------- | -------------- |
| Alto             | 2,27%         | **1,23x**      |
| Baixo (ref)      | 1,84%         | 1,00x          |
| Médio            | 1,53%         | 0,83x          |

- Clientes com alto debt ratio apresentam risco 23% maior.
- Pode indicar dificuldade de equilíbrio financeiro.

✅ 4. Imóvel Financiado (has_real_estate_loan)

| Grupo                 | Inadimplência | Risco Relativo  |
| --------------------- | ------------- | --------------- |
| Sem imóvel financiado | 2,87%         | **1,00x (ref)** |
| Com imóvel financiado | 1,31%         | **0,46x**       |

- Quem tem imóvel financiado apresenta menor risco.
- Pode indicar estabilidade financeira e maior vínculo com a instituição.

✅ 5. Quantidade de Empréstimos (loan_count)

| Grupo Empréstimos | Inadimplência | Risco Relativo  |
| ----------------- | ------------- | --------------- |
| Sem empréstimos   | 14,35%        | **1,00x (ref)** |
| Poucos            | 3,29%         | **0,23x**       |
| Muitos            | 1,62%         | **0,11x**       |

- Clientes sem empréstimos têm o maior risco, o que é contraintuitivo.
- Pode indicar clientes novos, sem histórico, ou com crédito recusado anteriormente.


---

✅ Conclusão da Análise de Risco Relativo

As variáveis com maior poder de discriminação de risco são:

Faixa Etária: risco 4,77x maior em jovens adultos.

Quantidade de Empréstimos: clientes sem empréstimos têm risco 14,35% — valor mais alto observado.

Faixa Salarial: correlação clara entre menor renda e maior risco.

Essas informações são valiosas para:

Definir políticas de concessão de crédito

Construir modelos preditivos

Segmentar clientes por risco

---

#### 🔴 Aplicar segmentação por Score

O **objetivo** desta etapa foi criar uma pontuação de risco (risk_score) para cada cliente, utilizando variáveis socioeconômicas e comportamentais previamente analisadas, e classificar os clientes entre bons e maus pagadores. A validação da classificação foi realizada com a variável default_flag e métricas de performance de modelos, além de avaliação financeira do impacto de cada decisão.

Metodologia

✅ 1. Criação do Risk Score

- Variáveis com influência significativa na inadimplência foram transformadas em dummies binárias:

- Idade jovem (18–34 anos)

- Salário baixo (< 5.000)

- Debt ratio alto (> 0,5)

- Não possuir imóvel financiado

- Poucos empréstimos (≤ 2)

- O risk_score foi calculado como a soma dessas dummies, atribuindo 1 ponto de risco para cada característica de maior risco.

✅ 2. Testes de Cutoffs

- Foram avaliados todos os cutoffs possíveis do risk_score (0 a 5), classificando clientes com score ≥ cutoff como mau pagador.

- Para cada cutoff, foram calculadas as métricas: acurácia, precisão, recall, F1-score e custo financeiro total considerando:

- Falso positivo: custo de negar crédito a bom pagador

- Falso negativo: custo de conceder crédito a inadimplente

✅ 3. Tabela Resumida de Cutoffs, Métricas e Custo

| Cutoff | VN     | FP     | FN  | VP  | Total  | Acurácia | Precisão | Recall | F1-score | Custo Total |
| ------ | ------ | ------ | --- | --- | ------ | -------- | -------- | ------ | -------- | ----------- |
| 0      | 0      | 35.317 | 0   | 683 | 36.000 | 0,019    | 0,019    | 1,0    | 0,037    | 1.765.850   |
| 1      | 9.162  | 26.155 | 52  | 631 | 36.000 | 0,272    | 0,024    | 0,924  | 0,046    | 2.607.750   |
| 2      | 21.135 | 14.182 | 237 | 446 | 36.000 | 0,599    | 0,030    | 0,653  | 0,058    | 6.634.100   |
| 3      | 30.651 | 4.666  | 463 | 220 | 36.000 | 0,858    | 0,045    | 0,322  | 0,079    | 11.808.300  |
| 4      | 34.286 | 1.031  | 612 | 71  | 36.000 | 0,954    | 0,064    | 0,104  | 0,080    | 15.351.550  |
| 5      | 35.266 | 51     | 682 | 1   | 36.000 | 0,980    | 0,019    | 0,001  | 0,003    | 17.052.550  |


🔴 Observação: VN = Verdadeiro Negativo, FP = Falso Positivo, FN = Falso Negativo, VP = Verdadeiro Positivo

---

Interpretação das Métricas

- Recall (sensibilidade): indica a capacidade do modelo em identificar corretamente os maus pagadores.

- Cutoffs baixos (0–1) apresentam recall alto (>90%), mas custo financeiro elevado devido a muitos falsos positivos.

- Cutoffs altos (4–5) reduzem o custo, mas quase eliminam o recall, deixando inadimplentes sem classificação.

- Precisão e F1-score: métricas baixas em todos os cutoffs devido ao baixo número de inadimplentes na base (classe desbalanceada), reforçando a necessidade de considerar trade-off entre custo e recall.

- Acurácia: aumenta com cutoffs maiores, mas não é a métrica mais relevante neste contexto de risco de crédito desbalanceado.

--- 

Escolha do Cutoff Ótimo

- A escolha profissional do cutoff deve equilibrar recall mínimo aceitável (capacidade de identificar mau pagador) e custo financeiro total.

- Cutoff = 3 foi selecionado como ótimo:

- Recall: 32,2% (identificação relevante de inadimplentes)

- Custo total: 11.808.300 (redução significativa comparado a cutoffs menores)

- Este cutoff representa o melhor trade-off entre risco de inadimplência e impacto financeiro, mantendo capacidade de segmentação sem comprometer demasiadamente os recursos do banco.
