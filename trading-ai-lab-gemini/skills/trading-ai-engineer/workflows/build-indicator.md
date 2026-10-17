\# Indicator Development Workflow



\## Purpose



Use this workflow whenever the user asks to create, modify, debug, or improve a custom indicator for MetaTrader 5 or TradingView.



The indicator must have a clearly defined calculation and signal behavior.



An indicator is not automatically a trading strategy.



\---



\# 1. Identify Requirements



Determine:



```text

Platform:

MQL5 / TradingView



Market:



Symbol:



Timeframe:



Inputs:



Calculation:



Output:



Visual Components:



Alerts:



Signal Conditions:



Repainting Behavior:

```



Do not invent unspecified requirements.



\---



\# 2. Indicator vs Strategy



Clearly distinguish:



```text

Indicator

=

Calculates / visualizes information

```



from:



```text

Strategy

=

Uses information to define trading decisions

```



Do not add trade execution to an indicator unless explicitly requested.



\---



\# 3. Read Relevant Knowledge



For MQL5 indicators:



```text

knowledge/mql5.md

knowledge/market-structure.md

knowledge/xauusd.md

```



For strategy-related indicators:



```text

knowledge/strategy-development.md

knowledge/backtesting.md

```



\---



\# 4. Define the Calculation



Before coding, document:



```text

Input Data

Calculation

Lookback

Smoothing

Thresholds

Output

```



The calculation must be deterministic.



\---



\# 5. Repainting Analysis



Determine whether the indicator can change historical values after additional market data becomes available.



Potential causes:



\- future-bar confirmation

\- centered calculations

\- future swing confirmation

\- dynamic historical recalculation

\- higher-timeframe synchronization issues



Explicitly classify:



```text

Non-Repainting

```



or:



```text

Repainting / Conditional Recalculation

```



or:



```text

Behavior Not Yet Verified

```



Never claim non-repainting without checking the actual implementation.



\---



\# 6. Current Candle vs Closed Candle



Explicitly define whether calculations use:



```text

Current Forming Candle

```



or:



```text

Last Closed Candle

```



Signal behavior must match the intended usage.



\---



\# 7. Buffer Design



For MQL5 custom indicators, define:



```text

Buffer

Index

Purpose

Data Type

Plot Type

```



Avoid creating unnecessary buffers.



\---



\# 8. Initialization



Validate during initialization:



```text

Inputs

Buffers

Plots

Indicator Handles

Required Data

```



If initialization fails:



```text

Log

↓

Return Initialization Failure

```



\---



\# 9. Calculation Efficiency



Avoid unnecessarily recalculating all historical bars on every update.



Where practical:



```text

Detect Recalculation Range

↓

Calculate Required Bars

↓

Update Buffers

```



The implementation should remain correct before optimization is attempted.



\---



\# 10. Multi-Timeframe Indicators



When using another timeframe:



Define:



```text

Source Timeframe

Chart Timeframe

Synchronization

Bar Confirmation

Data Availability

```



Do not assume a higher-timeframe bar is complete when it is still forming.



\---



\# 11. Multi-Symbol Indicators



When using another symbol:



Define:



```text

Source Symbol

Chart Symbol

Data Availability

Synchronization

```



Do not assume another symbol has identical timestamps or data availability.



\---



\# 12. Alerts



If alerts are required, define:



```text

Alert Condition

Alert Timing

Candle Confirmation

Repeat Behavior

Notification Type

```



Avoid sending repeated alerts on every tick for the same signal unless explicitly intended.



\---



\# 13. Signal Definition



When the indicator produces trading signals, define:



```text

BUY Condition

SELL Condition

Neutral Condition

Signal Timing

Confirmation

Invalidation

```



Do not use vague conditions such as:



```text

Strong trend

Strong rejection

Good momentum

```



unless they are translated into measurable rules.



\---



\# 14. Visual Testing



Verify:



```text

Plots

Colors

Labels

Arrows

Levels

Signals

Alerts

```



The visual result must represent the actual calculation.



Do not modify visual output to make historical signals appear better.



\---



\# 15. Historical Integrity



A historical signal must represent what would have been known at that point in time.



Check for:



```text

Future Data

Look-Ahead

Repainting

Future Swing Confirmation

Data Leakage

```



\---



\# 16. Indicator → EA Compatibility



If an EA will later consume the indicator:



Document:



```text

Buffer Index

Buffer Meaning

Signal Value

Empty Value

Shift / Candle Reference

Update Timing

```



The EA must read the correct buffer and candle.



\---



\# 17. Debugging



When an indicator behaves incorrectly:



```text

Observe

↓

Inspect Calculation

↓

Inspect Buffers

↓

Inspect Bar Index

↓

Inspect Inputs

↓

Identify Repainting

↓

Fix

↓

Retest

```



Do not immediately rewrite the entire indicator.



\---



\# 18. MQL5 Testing



For an MQL5 indicator:



```text

Compile

↓

Check Errors

↓

Attach to Chart

↓

Inspect Visual Output

↓

Compare With Expected Calculation

↓

Test Different Timeframes

↓

Test Different Symbols

```



\---



\# 19. Pine Script Testing



For TradingView:



```text

Compile

↓

Check Historical Signals

↓

Check Realtime Behavior

↓

Check Confirmed Candle Behavior

↓

Check Alerts

↓

Inspect Repainting

```



\---



\# 20. Modification Rule



When modifying an existing indicator:



```text

Existing Logic

\+

Requested Feature

=

Updated Indicator

```



Preserve unrelated functionality.



Document:



```text

What Changed

What Was Preserved

```



\---



\# 21. Output Documentation



When delivering an indicator, include:



```text

Indicator Name

Platform

Version

Inputs

Calculation

Signals

Repainting Behavior

Alerts

Usage

Known Limitations

```



\---



\# 22. Final Rule



The objective is not to create a visually impressive indicator.



The objective is to create an indicator whose calculations are:



```text

Correct

\+

Deterministic

\+

Understandable

\+

Testable

\+

Consistent With Real-Time Behavior

```



A perfect-looking historical chart is not sufficient evidence that an indicator works in real time.

