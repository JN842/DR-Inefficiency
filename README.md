# 🇻🇳 Vietnam-Linked DR Inefficiency
### VN Ceiling Days → Thai VN-Linked DR Overnight Gap

> **Research Note** — Tests whether constrained price discovery in Vietnamese equities is associated with next-session moves in Thailand-listed Depository Receipts. Results are descriptive and gross of transaction costs.

---

## 📌 What Is a DR?

**Depository Receipts (DRs)** are exchange-traded products that track foreign equities (U.S., Japan, Hong Kong, etc.) and let local investors trade overseas exposure in their home market.

---

## 🇻🇳 Why Vietnam-Linked DRs Are Special

Vietnam equities trade with a **daily price limit** (~7% depending on venue/board). When a stock closes near limit-up, price cannot fully adjust to demand — leaving buy pressure unfilled. This creates a **"demand shock"** that can spill into the next session.

---

## 💡 Research Motivation

Vietnam-linked DRs in Thailand can remain **tradable** even when the underlying VN stock is constrained by the ceiling. If demand cannot fully express in the underlying, price discovery may be reflected in the DR as a gap-up or continuation. This project tests that hypothesis; it does not assume that the relationship is causal or tradable after costs.

---

## 🔭 Universe — VN-Linked DRs in Thailand

The analyzed universe contains two issuer series observed in the dataset:

| Series | Issuer |
|--------|--------|
| `11` | KSecurities |
| `19` | Yuanta Securities |

**Tickers (all `.BK`):**
`FPTVN11` · `GASVN11` · `HPG19` · `MSN11` · `MWG11` · `VCB11` · `VHM19` · `VNM19`

---

## 🧪 Hypothesis

> After an underlying closes with a ceiling-like move (proxy: **≥ +6% close-to-close**), the DR's next-day return is predictably positive.

---

## 📡 Signal (Underlying)

```python
chg_pct = Close.pct_change() * 100
# Trigger when:
chg_pct >= 6
```

---

## 📋 Trading Rules (DR)

For each underlying signal, the notebook uses the first two available DR trading sessions on or after the signal date. When multiple signals occur on the same date, their returns are equally weighted.

### Strategy A — OPEN (Close → Next Open)
| Step | Action |
|------|--------|
| Entry | Buy DR at **day-0 Close** (first available DR session) |
| Exit | Sell DR at **day-1 Open** |
| Return | `Open[t1] / Close[t0] − 1` |

### Strategy B — CLOSE (Close → Next Close)
| Step | Action |
|------|--------|
| Entry | Buy DR at **day-0 Close** (first available DR session) |
| Exit | Sell DR at **day-1 Close** |
| Return | `Close[t1] / Close[t0] − 1` |

---

## 📊 Backtest Summary *(Gross, Before Costs)*

**NAV period:** `2025-04-10` → `2026-02-06` &nbsp;|&nbsp; **Observation Days:** 216 &nbsp;|&nbsp; **Active Days:** OPEN 20 / CLOSE 21 (~9–10%)  
**Data source:** Yahoo Finance via `yfinance`. Because the notebook retrieves data live, results may change when it is rerun.  
**Portfolio convention:** Same-day signals are equally weighted; non-signal business days carry a 0% return.

### ⚡ OPEN Strategy

| Metric | Value |
|--------|-------|
| Total Return | **63.99%** |
| CAGR | 78.08% |
| Sharpe Ratio | 4.28 |
| Ann. Volatility | 13.71% |
| Max Drawdown | −0.78% |
| Win Rate (active days) | **95.0%** |
| Best Day | +4.70% |
| Worst Day | −0.78% |

### 📈 CLOSE Strategy

| Metric | Value |
|--------|-------|
| Total Return | **91.52%** |
| CAGR | 113.43% |
| Sharpe Ratio | 3.82 |
| Ann. Volatility | 20.42% |
| Max Drawdown | −1.62% |
| Win Rate (active days) | **80.95%** |
| Best Day | +8.39% |
| Worst Day | −1.62% |

---

## ⚠️ Important Notes & Limitations

- All metrics are **gross** — fees, spread, slippage, and liquidity constraints are **excluded**.
- Only **20–21 active observations** drive the reported strategy returns, so the figures should not be treated as evidence of a robust or statistically validated trading edge.
- The `+6%` trigger is a ceiling-like proxy, not the official price-limit rule for every Vietnamese listing.
- No out-of-sample test, placebo test, confidence interval, or transaction-cost model is included in this version.
- *"Day 0"* and *"Day 1"* refer to the first two available DR sessions on or after the underlying signal date; VN and TH trading calendars differ.
- Results reflect this specific issuer/product universe (series `11` / `19`) and the data available through `2026-02-06` — not all DRs or all future periods.
- Yahoo Finance data can be revised or become unavailable, so the exact metrics are not guaranteed to be identical on a later rerun.

---

## 📄 License

This repository is for **research and educational purposes only**.
