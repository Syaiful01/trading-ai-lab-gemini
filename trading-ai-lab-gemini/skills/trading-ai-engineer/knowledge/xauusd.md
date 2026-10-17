\# XAUUSD Trading \& Engineering Framework



\## Purpose



This document defines the research and engineering standards for trading XAUUSD through MetaTrader 5.



XAUUSD must be treated as an instrument with broker-specific specifications and execution conditions.



Never assume that every broker, account type, or XAUUSD symbol uses identical trading parameters.



\---



\## 1. Primary Environment



Primary development platform:



```text

MetaTrader 5

```



Primary instrument:



```text

XAUUSD

```



Possible broker-specific symbol names include:



```text

XAUUSD

XAUUSDc

XAUUSDm

XAUUSD.s

```



The EA must use the actual symbol available in the terminal instead of assuming a universal symbol name.



\---



\## 2. Symbol Specification



Before trading, an EA should inspect the active symbol specification.



Important properties include:



```text

SYMBOL\_BID

SYMBOL\_ASK

SYMBOL\_POINT

SYMBOL\_DIGITS

SYMBOL\_TRADE\_TICK\_SIZE

SYMBOL\_TRADE\_TICK\_VALUE

SYMBOL\_TRADE\_TICK\_VALUE\_PROFIT

SYMBOL\_TRADE\_TICK\_VALUE\_LOSS

SYMBOL\_TRADE\_CONTRACT\_SIZE

SYMBOL\_VOLUME\_MIN

SYMBOL\_VOLUME\_MAX

SYMBOL\_VOLUME\_STEP

SYMBOL\_TRADE\_STOPS\_LEVEL

SYMBOL\_TRADE\_FREEZE\_LEVEL

SYMBOL\_TRADE\_MODE

SYMBOL\_TRADE\_EXEMODE

SYMBOL\_FILLING\_MODE

SYMBOL\_SPREAD

```



MQL5 exposes these values through symbol-information functions such as `SymbolInfoDouble()` and `SymbolInfoInteger()`.



Do not hard-code these values unless there is a documented reason.



\---



\## 3. Point, Tick Size, and Price Distance



Never assume that:



```text

1 point = 1 pip

```



or that the same point/price relationship exists between brokers.



Use the actual symbol properties:



```text

SYMBOL\_POINT

SYMBOL\_TRADE\_TICK\_SIZE

SYMBOL\_DIGITS

```



When converting a strategy's stop-loss or take-profit distance into a trade request, calculate from the actual symbol specification.



\---



\## 4. Monetary Risk



Lot size must not be chosen blindly.



When risk-based position sizing is used, calculate the monetary loss associated with the stop-loss distance using the symbol's actual tick size and tick value.



Conceptually:



```text

Risk Amount

&#x20;   ↓

Stop Distance

&#x20;   ↓

Tick Size / Tick Value

&#x20;   ↓

Required Position Size

```



The final volume must respect:



```text

SYMBOL\_VOLUME\_MIN

SYMBOL\_VOLUME\_MAX

SYMBOL\_VOLUME\_STEP

```



The EA should normalize the calculated volume to the broker's permitted volume step.



\---



\## 5. Spread



XAUUSD strategies must account for spread.



Spread can materially affect short-term strategies.



The EA should be capable of measuring current spread and, where appropriate, restricting new entries when spread becomes excessive.



Possible rule:



```text

Current Spread > Maximum Allowed Spread

&#x20;       ↓

No New Entry

```



The maximum spread should be configurable.



Do not assume that historical spread is constant.



\---



\## 6. Execution



The EA must distinguish between:



```text

Trade Signal

```



and:



```text

Successfully Executed Trade

```



A signal does not guarantee execution.



The execution layer must handle:



\- rejected orders

\- invalid volume

\- invalid stops

\- unsupported filling mode

\- market closure

\- trade permissions

\- insufficient margin

\- execution restrictions

\- broker-specific conditions



\---



\## 7. Filling Mode



An EA must not blindly force a single filling mode.



MQL5 exposes the allowed filling policies through `SYMBOL\_FILLING\_MODE`.



Depending on execution mode and symbol settings, supported policies may include:



```text

ORDER\_FILLING\_FOK

ORDER\_FILLING\_IOC

ORDER\_FILLING\_RETURN

```



However, `ORDER\_FILLING\_RETURN` is not allowed for market execution.



The EA should select a filling policy that is actually supported by the symbol.



When using the standard `CTrade` class, `SetTypeFillingBySymbol()` can configure the filling type according to the symbol settings.



This is especially important because forcing an unsupported filling mode can cause order rejection.



\---



\## 8. Execution Mode



The broker determines the symbol's execution mode.



Possible execution modes include:



```text

SYMBOL\_TRADE\_EXECUTION\_REQUEST

SYMBOL\_TRADE\_EXECUTION\_INSTANT

SYMBOL\_TRADE\_EXECUTION\_MARKET

SYMBOL\_TRADE\_EXECUTION\_EXCHANGE

```



The EA should inspect the actual execution mode instead of assuming it.



Execution mode influences how trade requests and filling policies should be handled.



\---



\## 9. Stop-Level Restrictions



Before placing or modifying SL/TP, verify:



```text

SYMBOL\_TRADE\_STOPS\_LEVEL

SYMBOL\_TRADE\_FREEZE\_LEVEL

```



The EA must ensure that requested stop levels comply with the broker's restrictions.



A signal should not result in an invalid trade request merely because the SL/TP distance is too close to the market.



\---



\## 10. Price Normalization



Prices should be normalized using the symbol's actual digit and tick-size requirements.



The EA should avoid assumptions such as:



```text

NormalizeDouble(price, 2)

```



without verifying the symbol's actual precision.



The correct price increment should be derived from symbol information.



\---



\## 11. Slippage



XAUUSD can move quickly.



Backtests and live systems should account for the difference between:



```text

Expected Entry

```



and:



```text

Actual Fill

```



The strategy must not assume zero slippage.



Where supported, define an explicit maximum deviation or equivalent execution tolerance.



\---



\## 12. Trading Sessions



Session filters may be used when a strategy depends on specific market behavior.



Possible sessions:



```text

Asian

London

New York

London-New York overlap

```



The session logic must account for:



\- broker server time

\- daylight-saving changes where relevant

\- actual market activity

\- strategy-specific behavior



Do not hard-code local Indonesian time and assume it is equivalent to broker server time.



\---



\## 13. Volatility



XAUUSD volatility changes significantly across market regimes.



Strategies should consider whether they are designed for:



```text

Low Volatility

Normal Volatility

High Volatility

Extreme Volatility

```



Possible volatility measurements include:



\- ATR

\- average range

\- standard deviation

\- normalized candle range



Volatility filters should be justified by the strategy hypothesis.



Do not add an ATR filter merely because it improves historical profit.



\---



\## 14. Spread and Volatility Interaction



High volatility can coincide with increased spread.



Therefore:



```text

High Volatility

\+

Wide Spread

=

Higher Execution Risk

```



A robust EA can optionally use both volatility and spread conditions before allowing new entries.



Example:



```text

Spread <= MaxSpread

AND

Volatility within acceptable range

&#x20;       ↓

Entry Allowed

```



\---



\## 15. Scalping Considerations



XAUUSD scalping strategies require additional scrutiny.



Evaluate:



\- average trade size in price

\- average holding time

\- spread-to-target ratio

\- commission

\- slippage

\- trade frequency

\- latency

\- execution rejection rate



A strategy with very small targets may be highly sensitive to transaction costs.



\---



\## 16. Risk/Reward



When using a fixed risk/reward model:



```text

Risk = Entry to Stop Loss

Reward = Entry to Take Profit

```



For example:



```text

SL = 50

TP = 100

```



represents a nominal 1:2 relationship only when the distances are measured consistently according to the instrument's price units.



The AI must verify how the strategy defines points, pips, ticks, or absolute price distance.



Do not mix these units.



\---



\## 17. Break-Even



Break-even logic must define exactly:



```text

When BE activates

How far price must move

Where SL is moved

Whether spread is considered

Whether commission is considered

Whether BE is applied once or repeatedly

```



Example concept:



```text

Profit >= BreakEvenTrigger

&#x20;       ↓

Move SL to

Entry + BreakEvenOffset

```



Break-even should not automatically be assumed to improve a strategy.



It must be tested.



\---



\## 18. Trailing Stop



Trailing logic should specify:



```text

Activation Distance

Trailing Distance

Trailing Step

Update Frequency

```



Avoid excessive stop updates on every tick when unnecessary.



Trailing rules should be tested independently because aggressive trailing can reduce average winner size.



\---



\## 19. Multiple Positions



When an EA supports multiple positions, define:



```text

Maximum Positions

Maximum Total Volume

Maximum Risk Exposure

Same-Direction Limit

Opposite-Direction Policy

Layering Distance

```



Never allow unlimited position accumulation.



If the strategy forbids simultaneous BUY and SELL positions, enforce this explicitly.



\---



\## 20. Daily Protection



XAUUSD systems should support configurable protection such as:



```text

Maximum Daily Loss

Maximum Daily Drawdown

Maximum Consecutive Losses

Trading Cutoff

Maximum Number of Trades

```



When a daily limit is reached:



```text

Stop Opening New Trades

```



Existing positions should continue to be managed according to the predefined position-management rules unless an emergency protection rule explicitly requires otherwise.



\---



\## 21. Equity Protection



For production systems, consider an equity protection mechanism.



Example:



```text

Starting Balance = B



Maximum Allowed Drawdown = D



Protection Level = B - D

```



When equity reaches the protection threshold:



```text

Disable New Entries

```



A more severe breach may optionally trigger position closure and system shutdown.



The exact behavior must be configurable and documented.



\---



\## 22. Broker Compatibility



Before testing or deploying an EA, collect a broker specification snapshot.



Recommended information:



```text

Broker

Account Type

Currency

Leverage

Symbol

Digits

Point

Tick Size

Tick Value

Contract Size

Minimum Volume

Maximum Volume

Volume Step

Spread

Execution Mode

Filling Mode

Stop Level

Freeze Level

Trade Mode

Trading Permissions

```



This snapshot should be stored with the experiment.



\---



\## 23. Common MT5 Failure Categories



When an order fails, investigate the actual trade result and retcode.



Potential categories include:



```text

Unsupported Filling Mode

Invalid Stops

Invalid Volume

Market Closed

Trade Disabled

AutoTrading Disabled

No Trading Permission

Insufficient Margin

Invalid Price

Invalid Request

```



Do not respond to every failed order by changing strategy logic.



First determine whether the problem is:



```text

Strategy Problem

```



or:



```text

Execution / Broker Problem

```



\---



\## 24. Debugging Standard



When an EA has execution problems, log enough information to reproduce the problem.



At minimum log:



```text

Symbol

Bid

Ask

Spread

Volume

SL

TP

Execution Mode

Filling Mode

Trade Mode

Retcode

Retcode Description

Margin Information

```



The debugging process should identify the exact rejected parameter rather than blindly changing settings.



\---



\## 25. Backtesting XAUUSD



Every XAUUSD backtest should record:



```text

Broker / Data Source

Symbol Name

Timeframe

Period

Initial Balance

Account Currency

Leverage if relevant

Spread Model

Commission

Slippage

Strategy Version

Parameters

Risk Per Trade

```



Do not compare two XAUUSD backtests without checking whether the testing conditions are comparable.



\---



\## 26. XAUUSD Strategy Research



Possible strategy categories include:



```text

Trend Following

Breakout

Pullback

Mean Reversion

Momentum

Session-Based Scalping

Volatility Expansion

Market Structure

Price Action

Multi-Timeframe

```



The AI should evaluate the market hypothesis before choosing indicators.



Indicators must support a defined hypothesis rather than being added simply to generate more signals.



\---



\## 27. Indicator Development



When developing a custom XAUUSD indicator, define:



```text

Input Data

Calculation

Signal Condition

Plot

Alert Condition

Repainting Behavior

Candle Confirmation

Timeframe Dependency

```



Explicitly distinguish between:



```text

Intrabar Signal

```



and:



```text

Confirmed Candle-Close Signal

```



\---



\## 28. EA Architecture



A production-quality XAUUSD EA should conceptually separate:



```text

Market Data

&#x20;     ↓

Signal Engine

&#x20;     ↓

Filters

&#x20;     ↓

Risk Manager

&#x20;     ↓

Position Sizing

&#x20;     ↓

Execution Engine

&#x20;     ↓

Position Manager

&#x20;     ↓

Protection

&#x20;     ↓

Logging / Diagnostics

```



This separation makes it easier to test individual components.



\---



\## 29. Development Priority



For XAUUSD EAs:



```text

Correct Execution

&#x20;       ↓

Risk Management

&#x20;       ↓

Correct Strategy Logic

&#x20;       ↓

Robustness

&#x20;       ↓

Optimization

&#x20;       ↓

Performance

```



Do not optimize a strategy before confirming that the execution layer is correct.



\---



\## 30. AI Rules for XAUUSD



When asked to build or modify a Gold EA, the AI should verify:



1\. What is the exact symbol?

2\. What is the point size?

3\. What is the tick size?

4\. What is the tick value?

5\. What is the volume step?

6\. What execution mode is used?

7\. What filling mode is supported?

8\. What are the stop/freeze restrictions?

9\. What account currency is used?

10\. What trading costs exist?

11\. What session is being traded?

12\. What is the expected spread?

13\. What is the maximum risk?

14\. What happens when execution fails?



If these details are unknown, the AI should clearly identify the assumption or request the required terminal data.



\---



\## 31. Final Principle



XAUUSD strategy development has two separate problems:



```text

Problem A:

Does the strategy have an edge?



Problem B:

Can the strategy execute that edge correctly?

```



Both problems must be solved.



A profitable strategy with broken execution is not a production system.



A perfectly engineered EA with no statistical edge is also not a production system.



The objective is:



```text

Statistical Edge

\+

Realistic Risk

\+

Correct Execution

\+

Robust Engineering

=

Production Candidate

```



Never treat this equation as a guarantee of profitability.

