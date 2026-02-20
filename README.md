# Vietnam-linked DR Inefficiency
Measures how Vietnam-linked DRs react the day after Vietnam underlying stocks hit a +7% “ceiling” move, using Yahoo Finance daily data.

VN Ceiling Days → Thai VN-Linked DR Overnight Gap (Research Note)

WHAT IS A DR?
Depository Receipts (DRs) are exchange-traded products that track foreign equities (U.S., Japan, Hong Kong, etc.) and let local investors trade overseas exposure in their home market.

CONTEXT (WHY VIETNAM IS SPECIAL)
Vietnam equities trade with a daily price limit (commonly referenced around ~7% depending on venue/board). When a stock closes near limit-up, price can’t fully adjust to demand, so buy pressure may remain unfilled (a “demand shock” that can spill into the next session).

WHY THIS MAY CREATE MISPRICING / OPPORTUNITY
Vietnam-linked DRs in Thailand can remain tradable and liquid even when the underlying VN stock is constrained by the ceiling. If demand can’t fully express in the underlying, price discovery may “leak” into the DR, showing up as a predictable gap-up / continuation in the DR.

UNIVERSE (VN-LINKED DRs IN THAILAND)
Only two issuers are active for VN-linked DRs currently:
• Series “11” = KSecurities
• Series “19” = Yuanta Securities
Tickers used: FPTVN11, GASVN11, HPG19, MSN11, MWG11, VCB11, VHM19, VNM19 (all with .BK)

HYPOTHESIS
After an underlying closes with a ceiling-like move (proxy: ≥ +6% close-to-close), the DR’s next-day return (Open/Close vs day-0 Close) is predictably positive.

SIGNAL (UNDERLYING)
• Compute: chg_pct = Close.pct_change() * 100
• Trigger when: chg_pct ≥ 6

TRADING RULES (DR)
OPEN Strategy (Close → Next Open)
• Buy DR at day-0 Close (signal day)
• Sell DR at day-1 Open
• Return = Open[t1] / Close[t0] − 1

CLOSE Strategy (Close → Next Close)
• Buy DR at day-0 Close
• Sell DR at day-1 Close
• Return = Close[t1] / Close[t0] − 1

BACKTEST SUMMARY (GROSS, BEFORE COSTS)
Period: 2025-04-10 → 2026-02-06 | Obs days: 216 | Active days: ~20–21 (~9–10%)

OPEN Strategy
• Total return: 63.99% | CAGR: 78.08%
• Sharpe: 4.28 | Ann. vol: 13.71%
• Max drawdown: −0.78% (plot may show −0.0078 if not ×100)
• Win rate (active days): 95.0%
• Best/Worst day: +4.70% / −0.78%

CLOSE Strategy
• Total return: 91.52% | CAGR: 113.43%
• Sharpe: 3.82 | Ann. vol: 20.42%
• Max drawdown: −1.62% (plot may show −0.0162 if not ×100)
• Win rate (active days): 80.95%
• Best/Worst day: +8.39% / −1.62%

IMPORTANT NOTES / LIMITATIONS
• Metrics exclude fees, spread, slippage, and liquidity constraints.
• “Next day” = next available DR trading session (VN vs TH calendars differ).
• Results reflect this issuer/product universe (mostly series 11/19), not “all DRs.”
