Got it — here’s a **concise and clean README.md** tailored to your exact setup (these 13 ETFs in `/data2`, raw code scripts, minimal infra):

---

# Adaptive Risk Reduction with Regime-Aware Models

Scripts and data for *Adaptive Risk Reduction with Regime-Aware Models* — comparing four regime-aware allocators:

* **M1**: PCA–HMM (hard regimes)
* **M2**: Prob–HMM + Momentum (Hybrid)
* **M3**: RA–HRP (prob-blend & mixture-cov)
* **M4**: IA+HMM (tempered fusion)

All models share the same rolling HMM framework (K=3, 500-day fit, refit every 3–5 days) and are risk-matched to **10% annualized volatility (ex-post)**.

---

## Repo Structure

```
Repo/
├─ data2/           # input data (13 ETFs)
│  ├─ ACWI.csv  BIL.csv  GLD.csv  GOVT.csv
│  ├─ IEF.csv   LQD.csv  QQQ.csv  SHV.csv
│  ├─ SPY.csv   TIP.csv  TLT.csv  URTH.csv  VTI.csv
├─ raw Code/        # main scripts
│  ├─ m1_pcahmm_hard.py
│  ├─ m2_hybrid.py
│  ├─ m3_rahrp.py
│  └─ m4_iahmm_fused.py
└─ out1/ out2/ out3/ out4/   # model outputs
```

Each CSV must contain:

* `Date`
* `Close` or `Adj Close` column
  (others are ignored; the filename becomes the ticker symbol.)

---

## Quickstart

```bash
python -m venv .venv
source .venv/bin/activate
pip install numpy pandas matplotlib hmmlearn scikit-learn torch
```

Run a model:

```bash
python "raw Code/m2_hybrid.py"
```

(defaults: `DATA_DIR=./data2`, output in `./out2`)

---

## Outputs

Each model exports:

* `*_kpi_native.csv` — native Sharpe, MaxDD, ES95
* `*_kpi_matchedVol.csv` — 10% vol (ex-post) KPIs
* `*_turnover_stats.csv` — mean / p90 turnover
* PNGs: cumulative wealth, drawdowns, turnover density, regime probabilities.

---

## Summary

| Model   | Ann.Return | Sharpe   | MaxDD     | ES95      |
| ------- | ---------- | -------- | --------- | --------- |
| Hybrid  | **0.245**  | **2.45** | −0.13     | 0.014     |
| IA+HMM  | 0.222      | 2.22     | −0.19     | **0.013** |
| PCA–HMM | 0.200      | 2.00     | **−0.11** | 0.014     |
| RA–HRP  | 0.159      | 1.59     | −0.28     | **0.011** |

Turnover remains **extremely low** across models, confirming that differences in Sharpe are **not cost-driven**.

---

## Transparency

This is a **non-causal**, pre-cost backtest meant for **relative model comparison**, not deployment.
Under identical settings, results remain **informative for risk management**.

---

## Citation

```
Duriez, J. (2025). Adaptive Risk Reduction with Regime-Aware Models:
PCA–HMM (Hard), Prob–HMM + Momentum, RA–HRP, and IA + HMM Fusion.
Working Paper.
```

---

**License:** MIT
**Contact:** Jeremy Duriez — jeremy.duriez285@gmail.com
