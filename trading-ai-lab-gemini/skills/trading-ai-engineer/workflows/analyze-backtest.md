\# Backtest Analysis Workflow



\## Purpose



Use this workflow whenever the user provides backtest results, screenshots, reports, CSV data, Strategy Tester output, or asks whether a strategy is good enough.



The objective is to determine what the evidence actually supports.



\---



\# 1. Identify the Test



Extract:



```text

Strategy

Version

Symbol

Market

Timeframe

Period

Initial Capital

Final Capital

Data Source

Execution Model

Spread

Commission

Slippage

```



If any important information is missing, mark it as:



```text

Unknown

```



Do not invent the missing value.



\---



\# 2. Verify the Strategy



Before interpreting the results, establish:



```text

Entry Rules

Exit Rules

Stop Loss

Take Profit

Risk

Filters

Maximum Positions

```



A performance result cannot be properly interpreted without knowing what was tested.



\---



\# 3. Verify Sample Size



Record:



```text

Total Trades

```



Then determine whether the result is based on:



```text

Small Sample

Moderate Sample

Large Sample

```



Do not make strong conclusions from a very small number of trades.



\---



\# 4. Primary Metrics



Inspect at minimum:



```text

Net Profit

Profit Factor

Win Rate

Maximum Drawdown

Expectancy

Total Trades

Average Trade

Average Win

Average Loss

Maximum Consecutive Losses

```



Do not rank a strategy using net profit alone.



\---



\# 5. Risk Analysis



Check:



```text

Maximum Drawdown

Daily Loss

Consecutive Losses

Maximum Exposure

Average Risk

Potential Risk of Ruin

```



Ask:



```text

Can the account realistically tolerate the observed drawdown?

```



\---



\# 6. Profit Distribution



Determine whether profit comes from:



```text

Many Trades

```



or:



```text

A Few Exceptional Trades

```



Also inspect:



```text

Best Month

Worst Month

Best Session

Worst Session

Best Regime

Worst Regime

```



A concentrated result deserves additional scrutiny.



\---



\# 7. Equity Curve



Inspect:



```text

Trend

Drawdowns

Flat Periods

Recovery

Profit Concentration

Abrupt Jumps

Instability

```



Do not judge only the final balance.



\---



\# 8. Long vs Short



Where applicable compare:



```text

BUY Performance

SELL Performance

```



Check whether one direction is responsible for most of the strategy's edge.



If so, determine whether there is a logical reason.



\---



\# 9. Time Analysis



Analyze:



```text

Monthly

Weekly

Daily

Session

Hour

```



where the data permits.



The objective is to determine whether the strategy's edge is broad or concentrated.



\---



\# 10. Market-Regime Analysis



Evaluate performance across:



```text

Trend

Range

High Volatility

Low Volatility

Breakout

Post-Breakout

Transition

```



If regime labels are not available, state the limitation.



\---



\# 11. Cost Analysis



Determine whether the backtest includes:



```text

Spread

Commission

Slippage

Swap

Funding

Other Costs

```



If costs are absent, classify the result as:



```text

Incomplete Cost Model

```



Do not treat the reported profit as realistic until costs are understood.



\---



\# 12. Execution Realism



Check:



```text

Order Type

Fill Assumption

Slippage

Liquidity

Latency

Stop Execution

Limit Fill Assumption

```



Potentially unrealistic assumptions must be documented.



\---



\# 13. Overfitting Review



Inspect for:



```text

Too Many Parameters

Excessive Filters

Isolated Parameter Peak

Extremely High Win Rate

Perfect Historical Fit

Very Narrow Valid Period

```



The AI should explicitly state the evidence for any overfitting concern.



\---



\# 14. Look-Ahead Review



Check whether the strategy may use information that was not available at decision time.



Potential examples:



```text

Future Candle

Future High / Low

Future Indicator State

Future Volume

Improperly Shifted Data

Repainting

Data Leakage

```



If the test methodology cannot rule this out, mark it as:



```text

Unverified

```



\---



\# 15. Parameter Stability



Determine whether nearby parameter values remain reasonably functional.



Example:



```text

Original:

EMA Fast = 8



Neighbors:

7

9

```



and:



```text

Original:

EMA Slow = 21



Neighbors:

20

22

```



Look for stable regions rather than a single historical optimum.



\---



\# 16. Stress Test



Where data allows, analyze:



```text

Higher Spread

Higher Slippage

Higher Commission

Reduced Win Rate

Reduced Average Win

Increased Average Loss

Execution Delay

```



Determine whether the strategy remains viable.



\---



\# 17. Out-of-Sample



Ask:



```text

Was unseen data used?

```



If yes:



```text

Was it protected from optimization?

```



Do not call a repeatedly tuned dataset "out-of-sample."



\---



\# 18. Walk-Forward



Where available, inspect:



```text

Training Period

Testing Period

Rolling Performance

```



Determine whether performance remains reasonably stable as the testing window moves through time.



\---



\# 19. Monte Carlo / Trade-Order Analysis



Where available, evaluate:



```text

Trade Order Randomization

Drawdown Distribution

Worst-Case Sequence

Return Variability

```



This is a robustness exercise, not a prediction of future performance.



\---



\# 20. Compare Against Baseline



Always compare the candidate against:



```text

Previous Version

Simpler Version

Buy-and-Hold where meaningful

Relevant Benchmark where appropriate

```



The benchmark must be logically relevant to the strategy.



\---



\# 21. Diagnose the Result



Classify the evidence:



\### Stronger Evidence



Results remain reasonably stable under realistic changes.



\### Weak Evidence



Positive result exists but important validation is missing.



\### Fragile Evidence



Results depend heavily on specific settings.



\### Negative Evidence



Strategy fails under realistic conditions.



\---



\# 22. Separate Facts From Interpretation



Use:



```text

\## Facts

What the test actually reports.



\## Interpretation

What those results may suggest.



\## Limitations

What the test cannot establish.



\## Unknowns

What information is missing.



\## Next Test

What would most improve confidence.

```



\---



\# 23. Classification



Choose:



```text

Experimental

Backtest Positive

Robust Candidate

Forward Testing

Validated

Rejected

```



Do not classify as "Validated" solely from a profitable historical backtest.



\---



\# 24. Recommendation



The AI should recommend one clear next action:



```text

Continue Research

Modify

Retest

Out-of-Sample

Robustness Test

Forward Test

Reject

Archive

```



The recommendation must be evidence-based.



\---



\# 25. Backtest Report



When a complete analysis is performed, use:



```text

templates/backtest-report.md

```



to document the final result.



\---



\# 26. Screenshot Analysis



If the user provides a Strategy Tester screenshot:



Extract only information that is actually visible.



Do not invent values hidden outside the screenshot.



Clearly identify:



```text

Visible

Not Visible

Cannot Verify

```



If necessary, request the raw report or CSV rather than guessing.



\---



\# 27. CSV / Raw Data Analysis



When raw trade data is available, analyze:



```text

Trade Distribution

Time Distribution

PnL Distribution

Drawdown

Consecutive Losses

Holding Time

Entry / Exit

Long / Short

Session

```



Prefer reproducible calculations over manual estimation.



\---



\# 28. Backtest Comparison



When comparing versions:



```text

Version A

vs

Version B

```



use a structured comparison:



```text

Metric

A

B

Difference

Interpretation

```



Do not automatically prefer the version with higher net profit.



\---



\# 29. Final Assessment Format



Use:



```text

\## Verdict



Current Classification:



\## What The Data Shows



\## What Looks Good



\## Main Weaknesses



\## Robustness Concerns



\## Missing Evidence



\## Risk Assessment



\## Recommended Next Test

```



\---



\# 30. Final Rule



The AI must never say:



```text

"This EA is profitable, so it is ready for live trading."

```



based only on historical backtest results.



Instead say what the evidence actually supports.



The purpose of backtest analysis is:



```text

Measure

↓

Question

↓

Stress

↓

Validate

```



not:



```text

Measure

↓

Celebrate

```

