# Crypto vs Global Markets — Multivariate Regression

Projeto final do MBA em Business Analytics & Big Data (FGV): análise e modelagem para **explicar/prever o preço (Y)** de 5 criptomoedas usando **mercados globais e moedas (X)**, com **engenharia de features temporais (lags e médias móveis)** e **Regressão Linear Múltipla** com seleção de variáveis.

---

## 1) Objetivo

Investigar em que medida indicadores de mercados globais (bolsas) e câmbio (moedas em USD) ajudam a explicar/predizer as seguintes criptomoedas (variáveis dependentes **Y**): **BTC, ETH, XRP, SOL, BNB**.

---

## 2) Variáveis

### Targets (Y)
- `BTC_USD`, `ETH_USD`, `XRP_USD`, `SOL_USD`, `BNB_USD`

### Exógenas (X)

**Bolsas globais (X):** Bombaim (Índia), DAX (Alemanha), Euronext (Europa), Hang Seng (China), Ibovespa (Brasil), London Stock (Reino Unido), Milão (Itália), NASDAQ (EUA), Nikkei (Japão), NYSE (EUA), Shanghai SSE (China), Shenzhen (China).

**Moedas (X, em USD):** BRL, CNY, EUR, GBP, INR, JPY.

---

## 3) Dados e Feature Engineering

### Linha do tempo e preenchimento
Para manter a série temporal consistente, foi adotada a regra de **replicar o fechamento do dia anterior** quando não havia dado disponível (por exemplo, feriados e fins de semana).

### Features temporais
- **Lags (atrasos temporais):** variando por versão da base (por exemplo, `lag_7` até `lag_50`, ou o conjunto compacto `lag_7/14/21/28`).
- **Médias móveis (MM):** `MM_10`, `MM_20`, `MM_30`.

**Definição simples**
- **Lag** é o valor de X em dias passados (ex.: 7 dias atrás).
- **Média móvel** é a média dos últimos `n` dias em uma janela “andante”.

---

## 4) Bases utilizadas (datasets)

Este repositório inclui como base principal a versão “pós-pandemia” com conjunto compacto de features:

- **A partir de 2022 (pós-pandemia)** com **lags 7/14/21/28** e **MMs 10/20/30** (total de **132 variáveis**).

### Arquivos recomendados neste repo
- `data/processed/all_lags_mms_2022.csv` (base canônica)
- `data/processed/all_lags_mms_2020.csv` (benchmark / comparação)
- `reports/results_best_all_times.csv` (tabela de “melhores modelos”)
- `docs/data_catalog.csv` (catálogo e descrição das bases)

---

## 5) Metodologia

### Fluxo analítico
1. **Análise exploratória (EDA):** matriz de correlação e dispersão para entender relações lineares/não lineares entre X e Y.
2. **Checagens para regressão linear múltipla:**
   - p-valor ≤ 0,10
   - VIF ≤ 10
   - avaliação de distribuição/normalidade das variáveis (quando aplicável)
3. **Regressão Linear Múltipla** com seleção **Stepwise Forward** (critérios p-valor e VIF).
4. **Avaliação:** comparação entre valores previstos e reais + análise de erro e explicabilidade.

---

## 6) Resultados e “case de explicabilidade”

O projeto contém diversos resultados por cripto e por base. Como destaque de explicabilidade, o **BTC** apresentou um dos melhores desempenhos com um modelo simplificado usando apenas a **NASDAQ** com lags, alcançando **R² mediano de teste ~0,58** e **RMSE% ~13,64**.

### Equação reportada no estudo de caso
```text
BTC_USD_prev = -74205.38
             + 5.2930 * NASDAQ_EUA_lag_7
             + 2.9192 * NASDAQ_EUA_lag_50
