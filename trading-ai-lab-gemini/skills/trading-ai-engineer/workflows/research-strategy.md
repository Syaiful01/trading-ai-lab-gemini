\# Strategy Research Workflow



\## Purpose



Use this workflow whenever the user proposes, modifies, compares, or asks to research a trading strategy.



The objective is to transform an idea into a falsifiable and testable trading hypothesis.



\---



\# 1. Identify the Request



Extract:



\- Market

\- Symbol

\- Market Type

\- Timeframe

\- Strategy Type

\- Trading Objective

\- Intended Holding Period

\- Platform

\- User Constraints



Do not invent missing facts.



Clearly separate:



```text

Known

Assumed

Unknown

```



\---



\# 2. Read Relevant Knowledge



Load only the relevant resources.



Possible resources:



```text

knowledge/market-structure.md

knowledge/strategy-development.md

knowledge/risk-management.md

knowledge/backtesting.md

knowledge/xauusd.md

knowledge/crypto.md

```



For XAUUSD + MT5, prioritize:



```text

market-structure

strategy-development

risk-management

backtesting

xauusd

mql5

```



For crypto, prioritize:



```text

market-structure

strategy-development

risk-management

backtesting

crypto

```



\---



\# 3. Form the Hypothesis



Write:



```text

Hypothesis:

```



Then explain:



\- What market behavior is being exploited?

\- Why might the behavior exist?

\- Under what conditions should it work?

\- Under what conditions should it fail?



The hypothesis must be falsifiable.



\---



\# 4. Challenge the Idea



Before coding, inspect:



\### Logical Risk



Is the hypothesis actually plausible?



\### Market Risk



Could it depend on a specific regime?



\### Data Risk



Could historical data create misleading evidence?



\### Execution Risk



Could spread, slippage, latency, or fees destroy the edge?



\### Overfitting Risk



Does the idea require too many parameters or filters?



\### Look-Ahead Risk



Could the proposed signal accidentally use future information?



\---



\# 5. Define Market Context



Document:



```text

Market Structure:

Market Regime:

Volatility Condition:

Important Price Location:

Session:

Higher-Timeframe Context:

```



Do not force a market-structure interpretation when the required data is unavailable.



\---



\# 6. Define Setup



Describe exactly what must happen before a trade is considered.



Separate:



```text

Context

\+

Setup

\+

Trigger

```



Example:



```text

Context:

Bullish higher-timeframe structure



Setup:

Pullback toward support



Trigger:

Confirmed bullish candle close

```



\---



\# 7. Define Entry



Specify:



```text

Direction:

Entry Condition:

Entry Trigger:

Entry Timing:

Entry Price:

Order Type:

```



Avoid discretionary language.



Replace:



```text

"Strong breakout"

```



with measurable rules.



\---



\# 8. Define Exit



Specify:



```text

Stop Loss:

Take Profit:

Break-Even:

Trailing:

Partial Exit:

Time Exit:

Signal-Reversal Exit:

```



Every exit must have a clear rule.



\---



\# 9. Define Invalidation



Specify exactly when the hypothesis becomes invalid.



Examples:



```text

Structural level broken

Setup expires

Volatility condition invalid

Session closes

Maximum risk condition reached

```



\---



\# 10. Define No-Trade Conditions



Explicitly determine when the system must remain inactive.



Examples:



```text

Spread too wide

Volatility unsuitable

Maximum positions reached

Daily loss reached

Outside trading session

Conflicting structure

Invalid signal

Execution conditions unsafe

```



\---



\# 11. Define Risk



Specify:



```text

Risk Per Trade:

Position Sizing:

Maximum Positions:

Maximum Exposure:

Maximum Daily Loss:

Maximum Drawdown:

Emergency Protection:

```



Risk must be defined before optimization.



\---



\# 12. Define Units



Explicitly identify:



```text

Price Units:

Points:

Pips:

Ticks:

Percent:

ATR Units:

Currency:

Lots / Contracts:

```



Never mix units without conversion.



\---



\# 13. Define Baseline Version



Create the simplest testable strategy version.



Example:



```text

V0.1

Basic Entry

\+

Basic Stop

\+

Basic Target

```



Avoid adding unnecessary indicators.



\---



\# 14. Determine Implementation



Decide whether the first prototype should use:



```text

MQL5

Pine Script

Python

Other

```



Choose the simplest platform that can answer the current research question.



\---



\# 15. Define Backtest Plan



Specify:



```text

Data Source:

Symbol:

Timeframe:

Period:

Initial Capital:

Risk:

Spread:

Commission:

Slippage:

Execution Model:

```



Separate:



```text

In-Sample

Out-of-Sample

```



where practical.



\---



\# 16. Baseline Test



The first test should answer:



```text

Does the basic hypothesis show enough evidence

to justify further investigation?

```



Do not begin with aggressive optimization.



\---



\# 17. Analyze Results



Review:



```text

Net Profit

Profit Factor

Win Rate

Maximum Drawdown

Expectancy

Trade Count

Average Trade

Consecutive Losses

```



Then investigate:



```text

Market Regime

Session

Long vs Short

Volatility

Time Period

Profit Concentration

```



\---



\# 18. Diagnose Failure



Classify failure as:



```text

Strategy

Risk

Execution

Data

Implementation

Overfitting

Market Regime

```



Do not modify strategy logic before identifying the likely failure layer.



\---



\# 19. Controlled Improvement



When improving:



```text

Observed Problem

↓

Hypothesis

↓

One Meaningful Change

↓

New Version

↓

New Test

```



Avoid changing many unrelated components at once.



\---



\# 20. Compare Versions



Compare the new version against the baseline.



Do not judge only by net profit.



Compare:



```text

Profit Factor

Drawdown

Expectancy

Trade Count

Average Trade

Parameter Stability

Cost Sensitivity

Out-of-Sample

```



\---



\# 21. Robustness



Before calling a strategy promising, test:



```text

Parameter Sensitivity

Spread Stress

Slippage Stress

Cost Stress

Market Regimes

Time Periods

Out-of-Sample

```



\---



\# 22. Research Decision



Classify the result:



```text

Continue

Modify

Retest

Out-of-Sample

Forward Test

Validate

Reject

Archive

```



Give a factual reason.



\---



\# 23. Documentation



Create an experiment record using:



```text

templates/experiment.md

```



For the strategy definition use:



```text

templates/strategy-spec.md

```



For the final backtest analysis use:



```text

templates/backtest-report.md

```



\---



\# 24. Final Output Format



When reporting strategy research, use:



```text

\## Strategy

Name and version



\## Hypothesis

What is being tested



\## Rules

Exact deterministic rules



\## Risk

Risk framework



\## Backtest

Test configuration



\## Results

Key metrics



\## Findings

What worked and failed



\## Weaknesses

Known limitations



\## Robustness

Stress-test findings



\## Classification

Current research status



\## Next Test

Most useful next experiment

```



\---



\# 25. Final Rule



The goal is not to make the idea look profitable.



The goal is to determine whether the hypothesis survives objective testing.



The AI must prefer:



```text

Evidence

>

Narrative



Robustness

>

Historical Perfection



Simple Rules

>

Unnecessary Complexity

```

