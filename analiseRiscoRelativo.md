## Documentação Técnica — Análise de Risco

#### 📊 Calcular risco relativo

A seguir, apresentamos o cálculo do Risco Relativo (RR) de inadimplência com base em variáveis socioeconômicas e comportamentais. A análise permite identificar perfis com maior propensão à inadimplência.

✅ 1. Idade (age)

| Quartil | Inadimplentes | Não Inadimplentes | Total | % Inadimplência | Risco Relativo |
|---------|-------------- |------------------|--------|-----------------|--------------- |
| 1       | 308           | 8692             | 9000   | 3.42%           | 1.00           |
| 2       | 207           | 8793             | 9000   | 2.30%           | 0.67           |
| 3       | 119           | 8881             | 9000   | 1.32%           | 0.39           |
| 4       | 49            | 8951             | 9000   | 0.54%           | 0.16           |

🔍 Insight: Risco relativo cai conforme aumenta a idade. Indica que clientes mais velhos têm menor chance de inadimplência. Isso é consistente com perfis de crédito: maior estabilidade, renda consolidada.

✅ 2. Salário do último mês (salary_last_month)

| Quartil | Inadimplentes | Não Inadimplentes | Total | % Inadimplência | Risco Relativo |
| ------- | ------------- | ----------------- | ----- | --------------- | -------------- |
| 1       | 268           | 8.732             | 9.000 | 2,98%           | 1,00           |
| 2       | 208           | 8.792             | 9.000 | 2,31%           | 0,78           |
| 3       | 132           | 8.868             | 9.000 | 1,47%           | 0,49           |
| 4       | 75            | 8.925             | 9.000 | 0,83%           | 0,28           |

🔍 Insight: Clientes com salários mais baixos têm maior risco de inadimplência.

✅ 3. Proporção dívida/renda (debt_ratio)

| Quartil | Inadimplentes | Não Inadimplentes | Total | % Inadimplência | Risco Relativo |
| ------- | ------------- | ----------------- | ----- | --------------- | -------------- |
| 1       | 160           | 8.840             | 9.000 | 1,78%           | 1,00           |
| 2       | 142           | 8.858             | 9.000 | 1,58%           | 0,89           |
| 3       | 202           | 8.798             | 9.000 | 2,24%           | 1,26           |
| 4       | 179           | 8.821             | 9.000 | 1,99%           | 1,12           |

🔍 Insight: Risco relativo aumenta no 3º e 4º quartis. Há um ponto de inflexão, sugerindo que dívida moderada ou alta aumenta risco.

✅ 4. Quantidade de Empréstimos (loan_count)

| Quartil | Inadimplentes | Não Inadimplentes | Total | % Inadimplência | Risco Relativo |
| ------- | ------------- | ----------------- | ----- | --------------- | -------------- |
| 1       | 321           | 8.679             | 9.000 | 3,57%           | 1,00           |
| 2       | 152           | 8.848             | 9.000 | 1,69%           | 0,47           |
| 3       | 105           | 8.895             | 9.000 | 1,17%           | 0,33           |
| 4       | 105           | 8.895             | 9.000 | 1,17%           | 0,33           |

🔍 Insight: Clientes com menos empréstimos são mais inadimplentes — pode ser perfil de entrada ou primeiro financiamento.

✅ 5. Linhas de crédito sem garantia (unsecured_credit_lines)

| Quartil | Inadimplentes | Não Inadimplentes | Total | % Inadimplência | Risco Relativo |
| ------- | ------------- | ----------------- | ----- | --------------- | -------------- |
| 1       | 8             | 8.992             | 9.000 | 0,09%           | 1,00           |
| 2       | 1             | 8.999             | 9.000 | 0,01%           | 0,11           |
| 3       | 34            | 8.966             | 9.000 | 0,38%           | 4,22           |
| 4       | 640           | 8.360             | 9.000 | 7,11%           | 79,00          |

🔍 Insight: Extremamente forte! Clientes com mais linhas de crédito sem garantia têm risco 80x maior. Esta variável é altamente discriminante.

✅ 6. Possui Imóvel Financiado (has_real_estate_loan)

| Quartil | Inadimplentes | Não Inadimplentes | Total | % Inadimplência | Risco Relativo |
| ------- | ------------- | ----------------- | ----- | --------------- | -------------- |
| 1       | 307           | 8693              | 9000  | 3,41%           | 1,00           |
| 2       | 168           | 8832              | 9000  | 1,87%           | 0,55           |
| 3       | 117           | 8883              | 9000  | 1,30%           | 0,38           |
| 4       | 91            | 8909              | 9000  | 1,01%           | 0,30           |

🔍 Insight: Clientes com empréstimo imobiliário (quartil 4) têm inadimplência muito menor (1,01%) comparado aos que não possuem (3,41%). O risco relativo diminui conforme aumenta o quartil, sugerindo que possuir um imóvel financiado está associado a menor risco de inadimplência.

✅ 7. Possui Outro Empréstimo (has_other_loan)

| Quartil | Inadimplentes | Não Inadimplentes | Total | % Inadimplência | Risco Relativo |
| ------- | ------------- | ----------------- | ----- | --------------- | -------------- |
| 1       | 308           | 8692              | 9000  | 3.42%           | 1.00           |
| 2       | 170           | 8830              | 9000  | 1.89%           | 0.55           |
| 3       | 115           | 8885              | 9000  | 1.28%           | 0.37           |
| 4       | 90            | 8910              | 9000  | 1.00%           | 0.29           |

🔍 Insight: O risco relativo diminui à medida que o quartil aumenta, indicando que clientes com mais empréstimos (ou maior classificação no indicador other_loan) apresentam menor taxa de inadimplência. Isso pode indicar que clientes mais experientes ou com perfil financeiro mais estável têm menor risco de inadimplência.

✅ 8. Atrasos > 90 dias (overdue_90_days)

| Linha | Quartil | Inadimplentes | Não Inadimplentes | Total | % Inadimplência | Risco Relativo |
| ----- | ------- | ------------- | ----------------- | ----- | --------------- | -------------- |
| 1     | 1       | 9             | 8991              | 9000  | 0,10%           | 1,00           |
| 2     | 2       | 15            | 8985              | 9000  | 0,17%           | 1,70           |
| 3     | 3       | 13            | 8987              | 9000  | 0,14%           | 1,40           |
| 4     | 4       | 646           | 8354              | 9000  | 7,18%           | 71,80          |

🔍 Insight: Clientes com histórico de atraso superior a 90 dias (quartil 4) apresentam inadimplência muito maior (7,18%) em comparação com aqueles sem atrasos (0,10%). O risco relativo no último quartil é altíssimo (71,8x), mostrando que atrasos passados são um forte preditor de inadimplência futura. Nos quartis 2 e 3, o risco relativo é ligeiramente maior que 1, mas o salto ocorre claramente no último grupo.

📊 Tabela Resumo – Análise de Risco Relativo

| Variável                     | Maior Risco Relativo | Interpretação / Insight Principal                             |
| ---------------------------- | -------------------- | ------------------------------------------------------------- |
| **unsecured\_credit\_lines** | **79,00x**           | Risco dispara com muitas linhas sem garantia                  |
| **overdue\_90\_days**        | **71,80x**           | Histórico de atraso > 90 dias é altamente preditivo           |
| **loan\_count**              | 1,00x (baixo)        | Poucos empréstimos = maior risco (possível perfil de entrada) |
| **age**                      | **1,00 → 0,16**      | Risco diminui fortemente com a idade                          |
| **salary\_last\_month**      | 2,98x → 0,83x        | Salários baixos estão associados a maior inadimplência        |
| **debt\_ratio**              | 1,26x                | Risco aumenta em níveis médios/altos de dívida                |
| **has\_real\_estate\_loan**  | 1,00x → 0,30x        | Quem tem imóvel financiado tem menor risco                    |
| **has\_other\_loan**         | 1,00x → 0,29x        | Ter outros empréstimos reduz risco (perfil mais confiável)    |


Com base em uma segmentação por quartis para as variáveis idade, faixa salarial, debt ratio, quantidade de linhas de crédito sem garantia (unsecured_credit_lines) e quantidade de empréstimos (loan_count), calculamos as taxas de inadimplência e o risco relativo, tomando sempre o primeiro quartil como referência

| Variável                           | Quartil/Grupo         | Taxa de Inadimplência | Risco Relativo | Insight Principal                                                    |
| ---------------------------------- | --------------------- | --------------------- | -------------- | -------------------------------------------------------------------- |
| **Idade**                          | 1 (mais jovem)        | 3,42%                 | 1,00           | Risco maior entre os mais jovens.                                    |
|                                    | ...                   | ...                   | ...            | ...                                                                  |
| **Faixa Salarial**                 | 1 (mais baixa)        | 2,98%                 | 1,00           | Risco diminui com aumento da faixa salarial.                         |
|                                    | ...                   | ...                   | ...            | ...                                                                  |
| **Debt Ratio**                     | 1 (mais baixo)        | 1,78%                 | 1,00           | Risco aumenta a partir do 3º quartil.                                |
|                                    | ...                   | ...                   | ...            | ...                                                                  |
| **Linhas de Crédito Sem Garantia** | 1 (menos linhas)      | 0,09%                 | 1,00           | Risco 80x maior no último quartil; variável altamente discriminante. |
|                                    | ...                   | ...                   | ...            | ...                                                                  |
| **Quantidade de Empréstimos**      | 1 (menos empréstimos) | 3,57%                 | 1,00           | Menos empréstimos associados a maior inadimplência.                  |
|                                    | ...                   | ...                   | ...            | ...                                                                  |
| **Possui Imóvel Financiado**       | Não                   | 2,87%                 | 1,00           | Ter imóvel financiado reduz risco.                                   |
|                                    | Sim                   | 1,31%                 | 0,46           |                                                                      |
| **Possui Outro Empréstimo**        | Não                   | 12,43%                | 1,00           | Ter outros empréstimos reduz risco.                                  |
|                                    | Sim                   | 1,74%                 | 0,14           |                                                                      |
| **Atraso > 90 dias**               | Não                   | 0,16%                 | 1,00           | Atrasos prévios elevam risco drasticamente.                          |
|                                    | Sim                   | 32,22%                | 195,93         |                                                                      |

#### 📊 Aplicar segmentação por Score

✅ Score de Risco

Regras de Pontuação: A pontuação (score_risco) varia de 0 a 9. Cada variável de risco adiciona 1 ponto, com exceção de overdue_90_days, que adiciona 2 pontos por ser mais crítica.

Distribuição do Score

| Score | Nº Clientes |
| ----- | ----------- |
| 0     | 1           |
| 1     | 302         |
| 2     | 9.247       |
| 3     | 15.619      |
| 4     | 7.819       |
| 5     | 1.934       |
| 6     | 879         |
| 7     | 174         |
| 8     | 24          |
| 9     | 1           |

✅ Matriz de Confusão (Corte Score ≥ 5)

|                      | Default = 1 | Default = 0 |
| -------------------- | ----------- | ----------- |
| Score ≥ 5 (Positivo) | 587 (TP)    | 2.425 (FP)  |
| Score < 5 (Negativo) | 96 (FN)     | 32.892 (TN) |

✅ Métricas

| Métrica  | Valor  |
| -------- | ------ |
| Acurácia | 92.99% |
| Precisão | 19.49% |
| Recall   | 85.94% |
| F1 Score | 31.77% |

✅ Análise de Pontos de Corte

| Corte | Precision | Recall | F1 Score |
| ----- | --------- | ------ | -------- |
| 3     | 2.58%     | 99.85% | 5.03%    |
| 4     | 6.24%     | 98.97% | 11.74%   |
| 5 ✅   | 19.49%    | 85.94% | 31.77%   |
| 6     | 37.29%    | 58.86% | 45.66%   |
| 7     | 49.24%    | 14.35% | 22.22%   |

Corte 5 escolhido como ponto de equilíbrio entre recall alto e precisão aceitável.

✅ Faixas de Risco

| Faixa de Risco | Total de Clientes | Total Inadimplentes | % Inadimplência |
| -------------- | ----------------- | ------------------- | --------------- |
| 1 - Seguro     | 9.550             | 1                   | 0,01%           |
| 2 - Moderado   | 15.619            | 6                   | 0,04%           |
| 3 - Alto       | 7.819             | 89                  | 1,14%           |
| 4 - Critico    | 3.012             | 587                 | 19,49%          |

🔍 Insights

1. Forte correlação entre risco e inadimplência

A taxa de inadimplência aumenta exponencialmente conforme a faixa de risco sobe. De 0,01% na faixa "Baixo" para 19,49% na faixa "Alto" — um aumento de quase 2.000 vezes.

2. Grande parte da base está concentrada em perfis de baixo risco

Faixas "Baixo" e "Médio" somam 25.169 clientes, ou seja, quase 70% da base total analisada. Estas faixas apresentam inadimplência praticamente nula (juntas, apenas 7 inadimplentes em 25 mil).

3. Faixa "Alto" representa risco desproporcional

Embora represente apenas 8,4% da base (3.012 clientes), a faixa "Alto" concentra 86% de todos os inadimplentes (587 de 683). Isso indica uma alta concentração de perdas em uma minoria da carteira.

4. Faixa "Moderado" merece atenção

Com inadimplência de 1,14%, essa faixa tem um risco muito maior do que "Médio" (0,04%), mas ainda bem inferior ao "Alto" (19,49%). Pode ser uma oportunidade para ajustar políticas de crédito ou monitoramento, sem excluir totalmente esses clientes.

📌[Documentação Técnica — Preparação Base de Dados](https://github.com/tha-lira/projeto_03-laboratoria-/blob/main/analiseBase.md)
🔗[Consultas SQL - Preparação Base de Dados](https://github.com/tha-lira/projeto_03-laboratoria-/blob/main/BigQuery-prepararBase.md)

📌[Documentação Técnica — Análise Exploratórias](https://github.com/tha-lira/projeto_03-laboratoria-/blob/main/analiseExploratoria.md)
🔗[Consultas SQL - Análise Exploratórias](https://github.com/tha-lira/projeto_03-laboratoria-/blob/main/BigQuery-analiseExploratoria.md)

📌[Documentação Técnica — Análise de Risco](https://github.com/tha-lira/projeto_03-laboratoria-/blob/main/analiseRiscoRelativo.md)
🔗 [Consultas SQL - Análise de Risco](https://github.com/tha-lira/projeto_03-laboratoria-/blob/main/BigQuery-analiseDeRisco.md)