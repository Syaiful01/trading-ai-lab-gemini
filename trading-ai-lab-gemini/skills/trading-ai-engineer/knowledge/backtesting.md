\# Backtesting Framework



\## Purpose



This document defines the standards for evaluating algorithmic trading strategies.



The purpose of backtesting is not to prove that a strategy will make money.



The purpose is to determine whether a strategy shows evidence of a potentially repeatable edge under realistic assumptions.



A positive backtest is evidence for further research, not proof of future profitability.



\---



\## 1. Backtesting Workflow



Every serious strategy should follow this process:



```text id="w8zq9m"

Strategy Hypothesis

&#x20;       ↓

Formal Rules

&#x20;       ↓

Historical Backtest

&#x20;       ↓

In-Sample / Out-of-Sample Separation

&#x20;       ↓

Robustness Testing

&#x20;       ↓

Walk-Forward Testing

&#x20;       ↓

Forward / Demo Testing

&#x20;       ↓

Live Evaluation

```



Do not skip directly from:



```text

Backtest → Live Trading

```



\---



\## 2. Strategy Formalization



Before running a backtest, the strategy must be expressed as deterministic rules.



The AI must define:



\- market

\- symbol

\- timeframe

\- entry conditions

\- exit conditions

\- stop loss

\- take profit

\- position sizing

\- filters

\- trading sessions

\- maximum positions

\- cooldown rules

\- risk limits



Avoid vague statements such as:



```text id="epapd9"

"Buy when the trend looks strong."

```



Replace them with measurable conditions.



Example:



```text id="k1pymj"

Fast EMA > Slow EMA

AND

RSI > threshold

AND

Close > previous swing high

```



\---



\## 3. Data Quality



Before trusting any result, verify:



\- symbol

\- timeframe

\- historical period

\- data source

\- missing data

\- duplicated data

\- timezone

\- session handling

\- spread assumptions

\- contract specifications



The AI must identify the data source used by the backtest.



Do not present results without knowing what data generated them.



\---



\## 4. In-Sample and Out-of-Sample



Whenever practical, historical data should be separated into:



```text id="lm2q48"

In-Sample

&#x20;   ↓

Used for strategy development



Out-of-Sample

&#x20;   ↓

Used for independent evaluation

```



The out-of-sample period must not be repeatedly used for parameter optimization.



Do not optimize on the complete historical dataset and then call the same dataset "out-of-sample."



\---



\## 5. Parameter Optimization



Optimization may be used to understand parameter sensitivity.



It must not be used only to find the single most profitable parameter combination.



Example:



```text id="j7p0bd"

EMA Fast = 8

EMA Slow = 21

```



should not automatically be preferred over:



```text id="11s8lx"

EMA Fast = 7

EMA Slow = 20

EMA Fast = 8

EMA Slow = 22

```



when neighboring parameter values show similar performance.



Stable regions are generally more informative than isolated peaks.



\---



\## 6. Parameter Sensitivity



A robust strategy should not collapse when a parameter changes slightly.



Test neighboring values.



For example:



```text id="tuh6d8"

EMA Fast:

7 / 8 / 9



EMA Slow:

20 / 21 / 22



RSI:

45 / 50 / 55

```



Look for a reasonably stable performance region.



Warning sign:



```text id="6exv8x"

Parameter 21 = excellent

Parameter 20 = terrible

Parameter 22 = terrible

```



This may indicate curve fitting.



\---



\## 7. Look-Ahead Bias



The strategy must never use information that would not have been available at the time of the trade.



Potential sources include:



\- future candles

\- future highs/lows

\- future indicators

\- improperly shifted data

\- future timestamps

\- improperly calculated signals

\- centered indicators

\- data leakage



The AI must explicitly inspect strategy logic for look-ahead bias.



\---



\## 8. Repainting



For indicators and signal systems, determine whether the signal can change after the candle closes.



Distinguish between:



```text id="nb9xaq"

Intrabar Signal

```



and:



```text id="g0m4bn"

Confirmed Candle-Close Signal

```



A backtest using confirmed historical values can look substantially better than a live system if the original indicator repaints.



Whenever relevant, explicitly state whether the indicator or strategy repaints.



\---



\## 9. Trading Costs



Backtests should account for realistic costs whenever the framework supports them.



Consider:



\- spread

\- commission

\- slippage

\- swap

\- funding

\- exchange fees

\- market impact



For scalping systems, transaction costs can materially affect profitability.



A strategy that is profitable before costs but unprofitable after costs should not be considered validated.



\---



\## 10. Slippage Testing



The baseline backtest should not be the only scenario.



Test multiple execution assumptions.



Example:



```text id="c6bbba"

Baseline

&#x20;   ↓

Normal Slippage



Stress Test

&#x20;   ↓

Adverse Slippage

```



The AI should report whether the strategy remains viable under reasonable execution degradation.



\---



\## 11. Spread Stress Testing



For instruments such as XAUUSD, spread should be treated as an important variable.



Test:



```text id="5ir1jv"

Normal Spread

Wider Spread

Abnormal Spread

```



Short-term strategies must receive particular scrutiny because a small increase in spread can materially reduce expectancy.



\---



\## 12. Performance Metrics



Never judge a strategy using net profit alone.



Record at minimum:



```text id="9kmm4c"

Net Profit

Total Trades

Win Rate

Profit Factor

Maximum Drawdown

Average Win

Average Loss

Expectancy

Average Trade

Largest Win

Largest Loss

Maximum Consecutive Wins

Maximum Consecutive Losses

Trade Frequency

```



Where available, also evaluate:



```text id="iz7i5y"

Sharpe Ratio

Sortino Ratio

Calmar Ratio

Recovery Factor

Ulcer Index

```



Metrics must be interpreted in context rather than treated as universal pass/fail thresholds.



\---



\## 13. Equity Curve



Inspect the equity curve, not just the final balance.



Look for:



\- long flat periods

\- sudden jumps

\- concentrated profit periods

\- large drawdowns

\- unstable returns

\- regime-specific performance



A strategy whose entire profit comes from a small number of trades requires additional scrutiny.



\---



\## 14. Trade Distribution



Analyze the distribution of trades.



Evaluate:



\- average trade

\- median trade

\- win/loss distribution

\- profit concentration

\- consecutive losses

\- consecutive wins

\- monthly returns

\- session performance

\- weekday performance

\- volatility-regime performance



Do not assume that aggregate results describe every market condition.



\---



\## 15. Market-Regime Testing



Test the strategy across different conditions where possible:



```text id="k46atc"

Strong Trend

Weak Trend

Range

High Volatility

Low Volatility

Fast Market

Slow Market

```



For XAUUSD, also consider different trading sessions.



For crypto, consider different volatility and liquidity regimes.



A strategy that only works in one environment should be classified accordingly.



\---



\## 16. Walk-Forward Testing



Walk-forward testing evaluates whether strategy parameters remain useful when applied to unseen future segments.



Conceptually:



```text id="6qf0sz"

Training Window

&#x20;     ↓

Optimization

&#x20;     ↓

Testing Window

&#x20;     ↓

Move Forward

&#x20;     ↓

Repeat

```



Example:



```text id="d2of31"

Train: 2023

Test: 2024 Q1



Train: 2023 Q2–2024 Q1

Test: 2024 Q2



Train: ...

Test: ...

```



The exact windows depend on the strategy and available data.



\---



\## 17. Monte Carlo / Trade-Order Testing



Historical trade order is only one possible sequence.



Where practical, perform randomized or Monte Carlo analysis on:



\- trade ordering

\- returns

\- slippage

\- execution delay

\- win rate

\- loss size



The goal is to estimate how bad the drawdown could become under unfavorable sequences.



Do not treat Monte Carlo output as a prediction.



It is a robustness tool.



\---



\## 18. Stress Scenarios



Every strategy should eventually be stress-tested.



Examples:



```text id="cpkk7q"

Lower Win Rate

Higher Spread

Higher Slippage

Higher Commission

Smaller Average Win

Larger Average Loss

Delayed Entry

Delayed Exit

Reduced Liquidity

```



A strategy that completely fails under minor unfavorable assumptions requires further investigation.



\---



\## 19. Sample Size



A small number of trades is weak evidence.



The AI must report the number of trades alongside performance metrics.



Be cautious with statements such as:



```text id="51n4gc"

"Strategy has a 75% win rate."

```



when the result comes from only a small number of trades.



Statistical confidence depends heavily on sample size and trade independence.



\---



\## 20. Profit Concentration



Check how much of total profit comes from:



\- one trade

\- a few trades

\- one month

\- one market regime

\- one session

\- one specific parameter region



A strategy may appear profitable while being dependent on a tiny portion of the historical sample.



\---



\## 21. Robustness Classification



Classify strategies using a research status.



\### Experimental



Promising hypothesis with insufficient evidence.



\### Backtest Positive



Historical backtest is positive but not yet sufficiently validated.



\### Robust Candidate



Shows relatively stable behavior under multiple tests.



\### Forward Testing



Currently being evaluated on unseen or live-market data.



\### Validated



Has passed the project's predefined validation process.



\### Rejected



Evidence suggests the strategy is not robust enough for further development.



The classification must be based on evidence, not enthusiasm.



\---



\## 22. Optimization Rules



The AI must not optimize solely for:



```text id="ez3g5l"

Maximum Net Profit

```



Consider multiple objectives:



\- drawdown

\- profit factor

\- expectancy

\- consistency

\- trade count

\- parameter stability

\- robustness

\- execution costs



Multi-objective reasoning is preferred over single-metric optimization.



\---



\## 23. Avoiding Data Snooping



Do not repeatedly modify a strategy after seeing the test result and continue calling the same dataset "unseen."



A dataset becomes contaminated when it is repeatedly used to make strategy decisions.



Keep a clear distinction between:



```text id="q4gx8l"

Research Data

Validation Data

Final Holdout Data

```



The final holdout should remain untouched until the strategy is sufficiently finalized.



\---



\## 24. Reproducibility



Every meaningful backtest should record:



```text id="g4q1f5"

Strategy Version

Code Version / Commit

Symbol

Timeframe

Data Source

Start Date

End Date

Initial Capital

Position Sizing

Risk Per Trade

Commission

Spread Assumption

Slippage Assumption

Parameters

Execution Model

Results

```



Another person should be able to reproduce the test from the recorded information.



\---



\## 25. Backtest Comparison



When comparing two strategy versions, do not compare only total profit.



Compare:



```text id="e8m7dh"

Profit Factor

Maximum Drawdown

Expectancy

Trade Count

Average Trade

Stability

Parameter Sensitivity

Cost Sensitivity

Out-of-Sample Performance

```



A strategy with lower profit but much better robustness may be preferable.



\---



\## 26. Strategy Changes



When changing a strategy after testing, record:



```text id="p8v6sd"

Previous Version

&#x20;       ↓

Observed Problem

&#x20;       ↓

Hypothesis

&#x20;       ↓

Modification

&#x20;       ↓

New Test

```



Do not silently overwrite previous research.



Preserve failed versions.



\---



\## 27. AI Backtest Reporting Standard



Whenever the AI reports backtest results, it should clearly separate:



\### Facts



What the test actually measured.



\### Assumptions



What costs, execution conditions, and data assumptions were used.



\### Interpretation



What the metrics may indicate.



\### Limitations



What the test cannot prove.



\### Next Test



What should be evaluated next.



Never present a backtest as a guarantee of future performance.



\---



\## 28. Minimum Acceptance Questions



Before recommending further development, the AI should answer:



1\. Is the strategy deterministic?

2\. Is the data appropriate?

3\. Is look-ahead bias absent?

4\. Does the strategy repaint?

5\. Are trading costs included?

6\. Is the sample size reasonable?

7\. Does the strategy survive out-of-sample testing?

8\. Are parameters reasonably stable?

9\. Does performance survive stress testing?

10\. Is the drawdown acceptable?

11\. Is performance concentrated in one regime?

12\. Can the strategy actually be executed?



If several answers are unknown, the strategy remains experimental.



\---



\## 29. Final Principle



Backtesting is a filtering mechanism.



It should eliminate weak ideas before real money is exposed.



The objective is:



```text id="0hs9v6"

Not:

"Find the perfect backtest."



But:

"Find a strategy whose edge remains plausible

after realistic scrutiny."

```

