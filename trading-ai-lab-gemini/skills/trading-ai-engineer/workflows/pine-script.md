\# Pine Script Development Workflow



\## Purpose



Use this workflow whenever the user asks to create, modify, debug, test, or convert a TradingView indicator or strategy using Pine Script.



The objective is to produce Pine Script that behaves consistently between historical testing and realtime execution.



\---



\# 1. Identify Script Type



Determine whether the request is for:



```text

Indicator

Strategy

Library

Alert System

```



Do not convert an indicator into a strategy without explicitly defining the trading rules.



\---



\# 2. Identify Requirements



Extract:



```text

Market

Symbol

Timeframe

Inputs

Entry Logic

Exit Logic

Plots

Alerts

Risk Parameters

Commission

Slippage

```



Do not invent unspecified rules.



\---



\# 3. Read Relevant Knowledge



Use:



```text

knowledge/strategy-development.md

knowledge/backtesting.md

knowledge/market-structure.md

```



For XAUUSD:



```text

knowledge/xauusd.md

```



For crypto:



```text

knowledge/crypto.md

```



\---



\# 4. Repainting



Explicitly determine whether the script can:



```text

Change Historical Signals

```



or:



```text

Maintain Stable Confirmed Signals

```



Potential causes:



\- future bars

\- pivot confirmation

\- `request.security()` behavior

\- realtime updates

\- lookahead settings

\- unconfirmed bars



Never claim that a script is non-repainting without evaluating its logic.



\---



\# 5. Candle Confirmation



Explicitly define whether signals are generated:



```text

Intrabar

```



or:



```text

At Candle Close

```



These are different behaviors.



If the user wants confirmed signals, use appropriate confirmation logic.



\---



\# 6. Strategy Timing



For every entry define:



```text

Signal Candle

Execution Candle

Entry Price

Order Type

```



Avoid hidden assumptions about when a signal becomes tradable.



\---



\# 7. `request.security()`



When using higher-timeframe or external-symbol data, explicitly evaluate:



```text

Timeframe

Gaps

Lookahead

Confirmation

Realtime Behavior

```



Avoid future-data leakage.



\---



\# 8. Strategy Settings



When creating a Pine strategy, explicitly consider:



```text

Commission

Slippage

Initial Capital

Position Size

Pyramiding

Currency

Margin

```



Do not rely on TradingView defaults when they materially affect the research result.



\---



\# 9. Position Sizing



Define:



```text

Fixed Quantity

Fixed Cash

Percent of Equity

Risk-Based Position Size

```



The implementation must match the intended strategy.



\---



\# 10. Stop Loss and Take Profit



Define:



```text

SL

TP

Risk/Reward

Break-Even

Trailing

Partial Exit

```



Avoid changing exit behavior merely because it improves historical results.



\---



\# 11. Historical Fill Assumptions



Historical strategy fills are based on TradingView's strategy simulation model.



The AI must identify important assumptions such as:



```text

Order Timing

Bar-Based Execution

Limit Order Fills

Stop Behavior

Slippage

Commission

```



Do not treat strategy backtest fills as identical to real exchange execution.



\---



\# 12. Alerts



Define:



```text

Alert Condition

Trigger Timing

Message

Signal Confirmation

Duplicate Prevention

```



Alert logic must match the actual trading logic.



\---



\# 13. Plotting



Plots should represent the actual calculations.



Do not alter historical plots to make signals appear cleaner or more profitable.



\---



\# 14. Debugging



When Pine Script produces incorrect results:



```text

Compile

↓

Inspect Inputs

↓

Inspect Series References

↓

Inspect Candle Index

↓

Inspect Repainting

↓

Inspect request.security()

↓

Compare Historical vs Realtime

↓

Fix

↓

Retest

```



\---



\# 15. Strategy Validation



Evaluate:



```text

Net Profit

Profit Factor

Win Rate

Maximum Drawdown

Expectancy

Trade Count

Average Trade

```



Then investigate:



```text

Costs

Slippage

Parameter Stability

Market Regimes

Out-of-Sample

```



\---



\# 16. Conversion: Indicator → Strategy



When converting an indicator to a strategy:



First preserve:



```text

Original Calculation

```



Then add:



```text

Entry

Exit

Risk

Position Management

```



Do not change the indicator calculation without explicitly documenting the change.



\---



\# 17. Conversion: Pine → MQL5



When converting Pine Script to MQL5:



Do not perform a line-by-line syntax translation.



Instead map:



```text

Pine Calculation

&#x20;       ↓

MQL5 Equivalent

&#x20;       ↓

Broker / Execution Rules

&#x20;       ↓

MT5 Position Model

```



Pay special attention to:



```text

Candle Timing

Series Indexing

Order Timing

Position Sizing

SL/TP

Pyramiding / Multiple Positions

Fees / Slippage

Execution

```



\---



\# 18. Modification Rule



When modifying an existing Pine Script:



```text

Read Existing Script

↓

Identify Current Behavior

↓

Implement Requested Change

↓

Preserve Existing Logic

↓

Regression Test

```



Do not silently redesign the strategy.



\---



\# 19. Final Output



When delivering a Pine Script system, include:



```text

Script Type

Version

Inputs

Entry Logic

Exit Logic

Risk

Commission

Slippage

Repainting Behavior

Alert Behavior

Known Limitations

Testing Instructions

```



\---



\# 20. Final Principle



Pine Script development is not only about syntax.



A professional implementation must account for:



```text

Correct Logic

\+

Correct Time Handling

\+

No Future Information

\+

Realistic Backtest Assumptions

\+

Realtime Consistency

```



Historical profitability alone is not sufficient evidence of live profitability.

