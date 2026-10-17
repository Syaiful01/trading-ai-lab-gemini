\# Strategy Development Framework



\## Purpose



This document defines the standard process for transforming a trading idea into a testable, implementable, and potentially deployable trading system.



The AI must follow this workflow instead of immediately producing code.



The objective is to turn:



```text

Trading Idea

&#x20;   ↓

Explicit Hypothesis

&#x20;   ↓

Deterministic Rules

&#x20;   ↓

Implementation

&#x20;   ↓

Backtest

&#x20;   ↓

Validation

&#x20;   ↓

Robustness

&#x20;   ↓

Forward Test

```



\---



\## 1. Core Philosophy



Every trading strategy begins as a hypothesis.



A hypothesis is not a fact.



Example:



```text

Hypothesis:

Gold tends to continue moving after a confirmed breakout

during elevated but controlled volatility.

```



This statement must then be converted into measurable rules.



The AI must distinguish:



```text

Idea

Hypothesis

Rule

Implementation

Evidence

Conclusion

```



Do not collapse these into one step.



\---



\## 2. Strategy Lifecycle



Every strategy should progress through:



```text

01\. Idea

02\. Hypothesis

03\. Specification

04\. Prototype

05\. Backtest

06\. Diagnosis

07\. Revision

08\. Out-of-Sample Test

09\. Robustness Test

10\. Forward Test

11\. Validation

12\. Deployment

```



A strategy can move backward in this process.



For example:



```text

Backtest

&#x20;  ↓

Problem discovered

&#x20;  ↓

Return to Hypothesis

```



This is normal research behavior.



\---



\## 3. Stage 1 — Idea



An idea may come from:



\- chart observation

\- market behavior

\- indicator behavior

\- price action

\- research paper

\- existing strategy

\- personal trading experience

\- quantitative experiment



At this stage, the idea does not need to be complete.



Example:



```text

"Maybe XAUUSD breakouts work better after volatility compression."

```



The AI should not yet assume that the idea is profitable.



\---



\## 4. Stage 2 — Hypothesis



Convert the idea into a falsifiable statement.



Bad:



```text

"This setup looks strong."

```



Better:



```text

"After volatility contraction, a breakout beyond the recent range

may generate positive expectancy when confirmed by a candle close."

```



A good hypothesis must be capable of being disproven.



\---



\## 5. Stage 3 — Strategy Specification



Before coding, define:



```text

Market

Symbol

Market Type

Timeframe

Trading Session

Entry

Exit

Stop Loss

Take Profit

Position Sizing

Filters

Maximum Positions

Cooldown

No-Trade Conditions

Risk Limits

```



Example:



```text

Market:

XAUUSD



Timeframe:

M5



Entry:

EMA trend alignment

\+

range breakout

\+

confirmed candle close



Stop:

Structural level



Target:

2R



Risk:

1% per trade

```



The exact values depend on the hypothesis.



\---



\## 6. Deterministic Rules



Every automated strategy must eventually become deterministic.



Avoid:



```text

"Enter when momentum looks strong."

```



Prefer:



```text

Fast EMA > Slow EMA

AND

Close > Range High

AND

ATR percentile > threshold

AND

Spread <= maximum allowed spread

```



The AI must identify any remaining discretionary element.



\---



\## 7. Define Units



Every strategy must clearly distinguish:



```text

Price

Points

Pips

Ticks

Percentage

ATR Units

Currency

Lots / Contracts

```



Do not mix units.



For example:



```text

SL = 50 points

```



is not necessarily equivalent to:



```text

SL = 50 pips

```



The actual conversion depends on the instrument's specification.



\---



\## 8. Define Entry



Entry rules should identify:



```text

Signal Condition

Confirmation

Entry Timing

Entry Price

Order Type

```



Example:



```text

Setup:

Bullish trend



Trigger:

M5 candle closes above previous swing high



Execution:

Market order on next available tick

```



This is much more testable than a visual description.



\---



\## 9. Define Exit



Every strategy should explicitly define how positions terminate.



Possible exits:



```text

Stop Loss

Take Profit

Trailing Stop

Break-Even

Partial Close

Time Exit

Signal Reversal

Structural Invalidation

Emergency Protection

```



The strategy may use one or more.



\---



\## 10. Define Invalidation



A trading hypothesis should state when it is considered wrong.



Example:



```text

Bullish breakout hypothesis



Invalidation:

Price closes back below the breakout level

within the defined confirmation window.

```



This prevents arbitrary decision-making.



\---



\## 11. Define No-Trade Conditions



A good strategy defines when it should stay out.



Examples:



```text

Spread Too Wide

Daily Loss Limit Reached

Maximum Positions Reached

Invalid Market Regime

Outside Trading Session

Insufficient Volatility

Extreme Volatility

Conflicting Structure

Execution Conditions Unsafe

```



No-trade conditions are part of the strategy itself.



\---



\## 12. Define Risk Before Optimization



Risk parameters must be defined before attempting to maximize performance.



At minimum:



```text

Risk Per Trade

Maximum Positions

Maximum Daily Loss

Maximum Drawdown

Maximum Exposure

```



Do not tune risk parameters solely to produce a better equity curve.



\---



\## 13. Build the Simplest Prototype



The first implementation should contain the minimum logic required to test the hypothesis.



Example:



```text

Version 0.1



EMA Trend

\+

Breakout

\+

Fixed SL

\+

Fixed TP

```



Do not immediately add:



```text

EMA

RSI

MACD

ATR

ADX

Volume

Session

News

Fractals

Bollinger Bands

Machine Learning

```



unless each component has a specific research purpose.



\---



\## 14. Prototype Before Production



Separate:



```text

Research Prototype

```



from:



```text

Production EA

```



The prototype exists to answer:



```text

"Does the hypothesis deserve further testing?"

```



Production engineering comes after evidence.



\---



\## 15. Backtest Baseline



The first backtest should establish a baseline.



Record:



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



Also record:



```text

Spread

Commission

Slippage

Data Source

Testing Period

Parameters

```



\---



\## 16. Analyze the Baseline



Do not immediately optimize.



First ask:



```text

Why did the strategy make money?

Why did it lose?

When did it perform best?

When did it fail?

Where is profit concentrated?

```



Analyze:



\- market regimes

\- sessions

\- volatility

\- long vs short

\- time periods

\- trade duration

\- entry location



\---



\## 17. Failure Analysis



When a strategy fails, classify the failure.



\### Signal Failure



The underlying hypothesis appears weak.



\### Risk Failure



The strategy may have an edge but unacceptable risk.



\### Execution Failure



Costs, spread, slippage, or broker constraints damage performance.



\### Data Failure



The historical data is unreliable or inappropriate.



\### Implementation Failure



The code does not represent the intended strategy.



\### Overfitting



The strategy depends too heavily on historical parameters.



The AI should diagnose before modifying.



\---



\## 18. Strategy Revision



When changing a strategy:



```text

Observed Problem

&#x20;     ↓

Hypothesis About Problem

&#x20;     ↓

Specific Modification

&#x20;     ↓

New Version

&#x20;     ↓

New Test

```



Do not change five unrelated components simultaneously.



Otherwise it becomes difficult to determine what caused the result.



\---



\## 19. One Change at a Time



During research, prefer controlled experiments.



Example:



```text

V1:

EMA + RSI



V2:

EMA + RSI + ATR Filter



V3:

EMA + RSI + ATR Filter + Session Filter

```



This makes the contribution of each component easier to analyze.



Multiple simultaneous changes may still be appropriate for major redesigns, but they must be documented clearly.



\---



\## 20. Baseline Comparison



Every modification should be compared against the previous version.



Example:



```text

Metric              V1      V2

\--------------------------------

Profit Factor       1.31    1.38

Drawdown            14%     11%

Trades              842     561

Expectancy          ...     ...

```



Do not declare V2 superior merely because net profit increased.



\---



\## 21. Complexity Penalty



When two strategies perform similarly, prefer the simpler one.



Example:



```text

Strategy A

5 rules

PF = 1.42



Strategy B

18 rules

PF = 1.45

```



Strategy A may be preferable because it has fewer assumptions and may be easier to maintain and validate.



The complexity of a strategy should require justification.



\---



\## 22. Indicator Addition Rule



Before adding an indicator, answer:



```text

What information does this indicator provide?



Why is this information not already represented?



What hypothesis does the indicator support?



How will we test whether it adds value?

```



Do not add an indicator simply because it improves historical profit.



\---



\## 23. Parameter Selection



Choose parameters from logical ranges.



Example:



```text

EMA Fast:

5–15



EMA Slow:

15–50

```



rather than searching thousands of arbitrary combinations without a hypothesis.



The goal is to find useful parameter regions, not a magical number.



\---



\## 24. Parameter Stability



After optimization, examine neighboring parameters.



Example:



```text

EMA Fast:

7

8

9



EMA Slow:

20

21

22

```



A robust strategy should preferably remain reasonably functional across nearby values.



An isolated peak deserves skepticism.



\---



\## 25. Out-of-Sample



Once a strategy is sufficiently developed:



```text

Lock Strategy

&#x20;     ↓

Run Unseen Data

&#x20;     ↓

Evaluate

```



Do not continue tuning the strategy based on the same out-of-sample result.



If the strategy fails OOS, return to research and explicitly record the failure.



\---



\## 26. Robustness Testing



Test the strategy against:



```text

Parameter Changes

Spread Changes

Slippage

Commission

Different Periods

Different Market Regimes

Execution Delay

Trade Order Randomization

```



A robust strategy should not depend on perfect conditions.



\---



\## 27. Walk-Forward Testing



Where appropriate:



```text

Historical Training

&#x20;      ↓

Optimization

&#x20;      ↓

Forward Test

&#x20;      ↓

Move Window

&#x20;      ↓

Repeat

```



This evaluates whether a strategy can adapt without relying on the entire future dataset.



\---



\## 28. Forward Testing



After historical validation:



```text

Backtest

&#x20;  ↓

Out-of-Sample

&#x20;  ↓

Forward / Demo

```



Track differences between:



```text

Expected

vs

Actual

```



including:



\- entry price

\- exit price

\- spread

\- slippage

\- signal timing

\- execution success



\---



\## 29. Production Readiness



Before deployment, verify:



```text

Strategy Logic

Risk

Execution

Broker/Exchange Compatibility

Error Handling

Logging

Protection

Restart Behavior

```



The production system should be tested independently from the research prototype.



\---



\## 30. Deployment Stages



Recommended progression:



```text

Research

&#x20;  ↓

Backtest

&#x20;  ↓

Out-of-Sample

&#x20;  ↓

Paper / Demo

&#x20;  ↓

Small Capital

&#x20;  ↓

Monitored Live

```



Avoid full-scale deployment immediately after optimization.



\---



\## 31. Strategy Versioning



Use explicit versions.



Example:



```text

XAU\_EMA\_RSI\_v1.0

XAU\_EMA\_RSI\_v1.1

XAU\_EMA\_RSI\_v2.0

```



Suggested meaning:



```text

Major Version

=

Major strategy logic change



Minor Version

=

Small rule or implementation change

```



The exact convention may vary but must remain consistent.



\---



\## 32. Experiment Documentation



Every major version should have a corresponding research record.



Recommended location:



```text

journal/experiments/

```



Record:



```text

Experiment ID

Strategy Version

Date

Hypothesis

Change

Backtest

Metrics

Observations

Weaknesses

Conclusion

Next Action

```



\---



\## 33. Failed Experiment Policy



If a strategy fails:



```text

DO NOT DELETE IT.

```



Store it in:



```text

journal/failed\_strategies/

```



Document:



```text

Why It Failed

What Was Learned

What Should Not Be Repeated

```



Research knowledge includes failures.



\---



\## 34. Validated Strategy Policy



A strategy should not move into:



```text

journal/validated\_strategies/

```



until it has met the project's validation criteria.



Validation should include relevant combinations of:



```text

Positive Backtest

Out-of-Sample Evidence

Parameter Stability

Cost Stress Test

Market-Regime Testing

Forward Testing

Acceptable Risk

```



There is no universal profit-factor or drawdown threshold.



Acceptance criteria should depend on the strategy's objective and risk profile.



\---



\## 35. Research Integrity



The AI must never:



\- fabricate data

\- fabricate backtest results

\- hide losing periods

\- ignore transaction costs

\- silently alter rules

\- claim OOS when data was used for optimization

\- present assumptions as facts



Research records must remain auditable.



\---



\## 36. Reproducibility



Every important experiment should record enough information to reproduce it.



Include:



```text

Code Version

Git Commit

Dataset

Symbol

Timeframe

Date Range

Initial Capital

Account Currency

Fees

Spread

Slippage

Parameters

Execution Model

```



Use Git commits as the primary code-version reference.



\---



\## 37. Research Questions



For every strategy, the AI should be able to answer:



```text

What is the market hypothesis?



Why should the edge exist?



Under what conditions should it work?



When should it fail?



What is the expected risk?



How sensitive is it to costs?



How sensitive is it to parameters?



Does it survive unseen data?



Can it be executed reliably?

```



If these questions cannot be answered, the strategy is not fully specified.



\---



\## 38. Strategy Review Template



Before implementation:



```text

Strategy Name:



Market:



Symbol:



Market Type:



Timeframe:



Hypothesis:



Market Regime:



Entry:



Trigger:



Stop Loss:



Take Profit:



Risk Per Trade:



Filters:



No-Trade Conditions:



Maximum Positions:



Execution Type:



Expected Costs:



Primary Failure Mode:



Validation Plan:

```



\---



\## 39. AI Operating Procedure



When the user proposes a new strategy, follow this order:



```text

1\. Understand the idea.

2\. Identify the hypothesis.

3\. Challenge weak assumptions.

4\. Formalize deterministic rules.

5\. Define risk.

6\. Define no-trade conditions.

7\. Identify implementation requirements.

8\. Build the simplest prototype.

9\. Test the baseline.

10\. Analyze failures.

11\. Improve systematically.

12\. Test out-of-sample.

13\. Perform robustness testing.

14\. Forward test.

15\. Document the result.

```



The AI should not skip directly from step 1 to step 8 unless the user explicitly requires a prototype.



\---



\## 40. User Request Interpretation



When the user says:



\### "Buatkan EA"



Interpret as:



```text

Understand Strategy

\+

Specify Logic

\+

Implement

\+

Validate

```



not merely:



```text

Generate Code

```



\### "Tambahkan fitur"



Interpret as:



```text

Preserve Existing Logic

\+

Add Requested Feature

\+

Regression Test

```



\### "Optimalkan"



Interpret as:



```text

Improve Robustness

```



not simply:



```text

Maximize Historical Profit

```



\### "Backtest bagus"



Interpret as:



```text

Investigate Why

\+

Check Robustness

\+

Check OOS

\+

Check Costs

```



\---



\## 41. Research Priority



When choosing between improvements, prioritize:



```text

Correctness

&#x20;   ↓

Risk

&#x20;   ↓

Robustness

&#x20;   ↓

Execution

&#x20;   ↓

Simplicity

&#x20;   ↓

Performance

```



A small increase in historical profit is less important than removing a major source of fragility.



\---



\## 42. Final Principle



The purpose of Trading AI Lab is not to manufacture impressive-looking trading systems.



It is to discover whether a trading hypothesis can survive increasingly realistic tests.



The preferred process is:



```text

Idea

&#x20;↓

Hypothesis

&#x20;↓

Rules

&#x20;↓

Prototype

&#x20;↓

Baseline

&#x20;↓

Diagnosis

&#x20;↓

Controlled Improvement

&#x20;↓

OOS

&#x20;↓

Robustness

&#x20;↓

Forward Test

&#x20;↓

Validation

```



The AI should act as a skeptical research partner.



Its job is not to agree with the user.



Its job is to help determine whether the idea is actually supported by evidence.

