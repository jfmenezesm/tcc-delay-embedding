# TCC: Reconstrução de Espaço de Fase (Delay Embedding) em Séries Temporais

Código do trabalho de conclusão de curso de João Fernando Menezes Mauro (Engenharia Elétrica, UFMG), orientado pelo Prof. Uriel Moreira Silva (DEST/UFMG).

Selecionamos hiperparâmetros (atraso τ, dimensão m e penalização λ) e comparamos o desempenho preditivo de variantes de regressão por delay embedding (MQO, lags significativos, LASSO, Ridge e Elastic Net) com o SARIMA, que entra apenas como referência. Nos sistemas caóticos, também comparamos com τ e m escolhidos por AMI + Cao e ACF + Cao.

## Arquivos

| Arquivo | O que faz |
|---|---|
| `Resultados_Embedding_Sistemas_v9.qmd` | Sistemas de Lorenz, Rössler e Hénon. Troque `sistema` no topo do arquivo (`"lorenz"`, `"rossler"` ou `"henon"`) e renderize uma vez por sistema. |
| `Resultados_Embedding_SARIMA_v16.qmd` | Série real: variantes de embedding, SARIMA, figuras e intervalos por bootstrap. |
| `Resultados_serie_Real_Nicolas_v4.qmd` | Busca do SARIMA da série real pelo EQMP de validação de 1 passo. |
| `resumo_lorenz.txt`, `resumo_rossler.txt`, `resumo_henon.txt` | Tabelas finais de cada sistema (τ, m, EQMP de validação e de teste, EQMP%). |

## Protocolo

- Divisão em estimação, validação e teste: 229/12/12 na série real e 4600/200/200 nos sistemas.
- τ, m e λ são escolhidos pelo menor EQMP de previsão a 1 passo na validação.
- O ajuste final usa estimação + validação. O teste é previsto 1 passo à frente com defasagens reais, sem recursão e sem reestimação.
- EQMP% = 100 · EQMP / variância da janela de teste.
- Regimes dos sistemas: Lorenz (σ=10, β=8/3, ρ=28, passo 0,1); Rössler (a=0,2, b=0,2, w=11, passo 0,5, integrado a 0,01 e subamostrado); Hénon (a=1,12, b=0,3). Todos com n=5000 e H=200.
- Cao com dimensão máxima 100 em ambas as chamadas de `estimateEmbeddingDim`.

## Como executar

1. Instale o R e o [Quarto](https://quarto.org).
2. Instale os pacotes:
   ```r
   install.packages(c("astsa", "dplyr", "forecast", "ggplot2", "glmnet", "here",
                      "kableExtra", "lmtest", "nonlinearTseries", "performance",
                      "see", "tidyverse", "zoo", "knitr", "scales", "zip"))
   ```
3. Para os sistemas, edite `sistema` no topo de `Resultados_Embedding_Sistemas_v9.qmd` e renderize:
   ```
   quarto render Resultados_Embedding_Sistemas_v9.qmd
   ```
4. Para a série real, rode primeiro `Resultados_serie_Real_Nicolas_v4.qmd` (gera o modelo SARIMA escolhido) e depois `Resultados_Embedding_SARIMA_v16.qmd`.

As execuções são demoradas (de horas, no caso dos sistemas e do bootstrap). Cada uma salva um checkpoint da sessão (`sessao_<sistema>_final.RData`), que permite reabrir os resultados sem rodar de novo.

## Dados

A série real não está incluída neste repositório. Os sistemas caóticos são simulados pelo próprio código (pacote `nonlinearTseries`, com semente fixa).
