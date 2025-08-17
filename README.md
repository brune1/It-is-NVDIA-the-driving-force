# Stock Analysis — Big Tech (AAPL, MSFT, AMZN, GOOG, NVDA)

Analisi quantitativa dei Big Tech USA (2018→oggi) con Python: raccolta dati via `yfinance`, creazione di indicatori (log-returns, SMA50/200, volatilità annualizzata, CAGR, max drawdown) e **regressione lineare** per misurare la dipendenza tra i rendimenti dei titoli.

> Obiettivo didattico & professionale: imparare metodologie reali di data science in finanza e presentare un progetto chiaro su GitHub/LinkedIn.

---

## ✨ Risultati principali (tl;dr)
- **NVIDIA (NVDA)**: CAGR più alto (~60% annuo sul periodo) ma anche **volatilità** e **drawdown** più elevati → crescita esplosiva ma rischiosa.
- **Microsoft (MSFT) & Apple (AAPL)**: miglior **trade-off rischio/rendimento** (volatilità più bassa, drawdown contenuti, trend stabile).
- **Google (GOOG) & Amazon (AMZN)**: più ciclici; hanno sofferto nel 2022, poi recupero.
- **Regressione (esempio Y=NVDA, X=AAPL,MSFT,AMZN,GOOG)**: R² test ≈ **0.33** → gli altri big tech spiegano circa un terzo della variabilità giornaliera di NVDA; **MSFT** emerge spesso come predittore più influente. Residui centrati a 0 ma con outlier in giornate di news/shock.

---

## 📦 Contenuti del repository
.
├── data/
│ ├── raw/ # dati grezzi (IGNORATI da git)
│ └── processed/ # output intermedi (es. adj_close_wide.csv, metrics_summary.csv)
├── notebooks/
│ ├── phase1_data_collection.ipynb # download + salvataggi
│ ├── phase2_features.ipynb # feature engineering + KPI
│ └── phase3_regression.ipynb # regressione lineare tra titoli
├── README.md
├── requirements.txt
└── .gitignore


**Nota dati:** per policy, i dati grezzi non sono versionati su GitHub. I notebook ricreano i dataset usando `yfinance`.

---

## 🧠 Metodologia

1. **Raccolta dati (Fase 1)**
   - `yfinance.download()` per AAPL, MSFT, AMZN, GOOG, NVDA (2018→oggi).
   - Salvataggio **raw** (OHLCV multi-index) e viste **processed**:
     - `adj_close_wide.csv` (Date × Ticker)
     - `volume_wide.csv`
     - CSV per-ticker (Date, Adj Close, Volume)

2. **Feature & KPI (Fase 2)**
   - **Log-returns** giornalieri; **SMA50/200**; **volatilità annualizzata**, **CAGR**, **max drawdown**.
   - Output: `features_daily_long.csv`, `returns_monthly.csv`, `metrics_summary.csv`.

3. **Regressione lineare (Fase 3B)**
   - Target: rendimento giornaliero di un titolo (es. **NVDA**).
   - Predittori: rendimenti degli altri (AAPL, MSFT, AMZN, GOOG).
   - Split temporale (train: fino al 2023, test: 2024+), standardizzazione, fit `LinearRegression`.
   - Valutazione: **R²** train/test, analisi **residui**, **correlazioni** tra predittori (e VIF se disponibile).
   - Output: `outputs/regression_summary_<TICKER>.txt` + grafici diagnostici (scatter y vs ŷ, residui).

---

## ▶️ Come eseguire il progetto (riproducibile)

1. **Clona il repo e crea l’ambiente**
   ```bash
   git clone <URL_DEL_REPO>
   cd <NOME_REPO>
   python -m venv .venv
   source .venv/bin/activate   # su Windows: .venv\Scripts\activate
   pip install -r requirements.txt


