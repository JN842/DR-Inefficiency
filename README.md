# 🇻🇳 Vietnam-Linked DR Inefficiency
### VN Ceiling Days → Thai VN-Linked DR Overnight Gap

> **Research Note** — Backtested strategy exploiting price discovery leakage from Vietnamese equity price limits into Thailand-listed Depository Receipts.

---

## 📌 What Is a DR?

**Depository Receipts (DRs)** are exchange-traded products that track foreign equities (U.S., Japan, Hong Kong, etc.) and let local investors trade overseas exposure in their home market.

---

## 🇻🇳 Why Vietnam-Linked DRs Are Special

Vietnam equities trade with a **daily price limit** (~7% depending on venue/board). When a stock closes near limit-up, price cannot fully adjust to demand — leaving buy pressure unfilled. This creates a **"demand shock"** that can spill into the next session.

---

## 💡 Why This May Create Mispricing / Opportunity

Vietnam-linked DRs in Thailand can remain **tradable and liquid** even when the underlying VN stock is constrained by the ceiling. If demand can't fully express in the underlying, price discovery may **"leak" into the DR**, showing up as a predictable gap-up or continuation in the DR.

---

## 🔭 Universe — VN-Linked DRs in Thailand

Only two issuers are currently active for VN-linked DRs:

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

### Strategy A — OPEN (Close → Next Open)
| Step | Action |
|------|--------|
| Entry | Buy DR at **day-0 Close** (signal day) |
| Exit | Sell DR at **day-1 Open** |
| Return | `Open[t1] / Close[t0] − 1` |

### Strategy B — CLOSE (Close → Next Close)
| Step | Action |
|------|--------|
| Entry | Buy DR at **day-0 Close** |
| Exit | Sell DR at **day-1 Close** |
| Return | `Close[t1] / Close[t0] − 1` |

---

## 📊 Backtest Summary *(Gross, Before Costs)*

**Period:** `2025-04-10` → `2026-02-06` &nbsp;|&nbsp; **Observation Days:** 216 &nbsp;|&nbsp; **Active Days:** ~20–21 (~9–10%)

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
- *"Next day"* refers to the next available DR trading session; VN and TH trading calendars differ.
- Results reflect this specific issuer/product universe (series `11` / `19`) — not all DRs.

---

## 📄 License

This repository is for **research and educational purposes only**.
