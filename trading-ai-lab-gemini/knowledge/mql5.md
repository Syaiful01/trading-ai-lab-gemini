\# MQL5 Engineering Framework



\## Purpose



This document defines the coding, architecture, debugging, and execution standards for MetaTrader 5 Expert Advisors and custom indicators.



The objective is to produce MQL5 systems that are:



\- Correct

\- Robust

\- Broker-compatible

\- Testable

\- Maintainable

\- Debuggable

\- Risk-controlled



A compiling EA is not automatically a correct EA.



\---



\## 1. Program Types



MQL5 development may involve:



\- Expert Advisors

\- Custom Indicators

\- Scripts

\- Libraries



This repository primarily focuses on:



```text id="q5t1rq"

Expert Advisors

Custom Indicators

Supporting Libraries

```



Use the correct program type for the intended task.



\---



\## 2. EA Lifecycle



A standard EA should conceptually separate:



```text id="r4bgtr"

OnInit()

&#x20;  ↓

Initialization



OnTick()

&#x20;  ↓

Market-event processing



OnTradeTransaction()

&#x20;  ↓

Trade/account event processing where required



OnDeinit()

&#x20;  ↓

Cleanup

```



Initialization should validate required resources and configuration before the EA begins normal operation.



Prefer:



```text id="x4a3pd"

int OnInit()

```



over the legacy `void OnInit()` form when the EA needs to report initialization status.



MQL5 documents `int OnInit()` as the recommended form and allows initialization failure to be reported through an initialization return code.



\---



\## 3. OnTick Responsibility



`OnTick()` is an event handler for new ticks received by the EA's chart symbol.



Do not put unnecessarily expensive repeated operations inside `OnTick()`.



Separate:



```text id="3wqq3r"

Signal Calculation

Risk Calculation

Position Management

Entry Execution

Diagnostics

```



into dedicated functions where practical.



The AI should avoid turning `OnTick()` into one massive block of code.



MQL5's `OnTick()` documentation provides examples of EAs processing price events and accessing symbol/position information inside the handler.



\---



\## 4. Initialization Checklist



During `OnInit()`, verify where relevant:



```text id="d90fla"

Symbol

Timeframe

Indicator Handles

Trade Object

Magic Number

Volume Constraints

Tick Size

Point

Digits

Execution Mode

Filling Mode

Input Validity

```



If a required initialization step fails:



```text id="s0c7tw"

Print Diagnostic

&#x20;       ↓

Return Initialization Failure

```



Do not allow an EA to continue operating in a partially initialized state.



\---



\## 5. Indicator Handles



For built-in indicators such as:



\- EMA

\- RSI

\- ATR

\- MACD

\- Bollinger Bands



prefer creating indicator handles during initialization.



Conceptually:



```text id="p5kqws"

OnInit()

&#x20;   ↓

Create Indicator Handle

&#x20;   ↓

Validate Handle

```



Then retrieve data during operation.



Do not repeatedly recreate indicator handles on every tick unless there is a specific reason.



\---



\## 6. CopyBuffer



When using indicator handles, use `CopyBuffer()` to obtain indicator values.



The AI must explicitly consider:



\- requested buffer index

\- requested number of values

\- starting position

\- array direction

\- sufficient historical bars

\- return value of `CopyBuffer()`



Never assume indicator data was successfully retrieved.



Conceptual pattern:



```text id="0j0sj9"

CopyBuffer()

&#x20;   ↓

Check Returned Count

&#x20;   ↓

If insufficient:

&#x20;   Stop Signal Calculation

```



Do not generate a trade signal from invalid or incomplete indicator data.



\---



\## 7. Closed Candle vs Current Candle



The EA must explicitly determine whether a strategy uses:



```text id="u7m7im"

Current Candle

```



or:



```text id="vfh4v3"

Last Closed Candle

```



For example:



```text id="yqcc1s"

shift = 0

```



normally refers to the current forming bar.



While:



```text id="v0v7j0"

shift = 1

```



is commonly used for the most recently closed bar.



The strategy specification must state which one is intended.



Never silently change a candle-confirmation rule.



\---



\## 8. New-Bar Detection



When a strategy is intended to execute once per candle rather than on every tick, implement explicit new-bar detection.



Conceptually:



```text id="f9o5y0"

Current Bar Time

&#x20;       ≠

Stored Bar Time

&#x20;       ↓

New Bar

```



Then update the stored bar timestamp.



This prevents repeated entries from the same candle when the strategy is designed for one signal per bar.



\---



\## 9. Tick-Based Strategies



Some strategies intentionally operate on every tick.



For those systems, the AI must explicitly document:



\- why tick-level processing is required

\- whether duplicate entries are possible

\- cooldown behavior

\- position limits

\- signal persistence

\- intrabar behavior



Do not accidentally convert a candle strategy into a tick-triggered strategy.



\---



\## 10. Symbol Information



Never assume that every broker provides identical instrument specifications.



Important properties include:



```text id="w9xq1b"

SYMBOL\_DIGITS

SYMBOL\_POINT

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



For position sizing, price normalization, SL/TP validation, and execution, use actual symbol properties.



\---



\## 11. Price Normalization



Do not blindly use:



```text id="h0fh79"

NormalizeDouble(price, 2)

```



unless the symbol specification actually requires that precision.



Price calculations should respect:



```text id="jrrg4s"

SYMBOL\_DIGITS

SYMBOL\_TRADE\_TICK\_SIZE

```



The EA should normalize requested prices appropriately.



\---



\## 12. Volume Normalization



Calculated position volume must respect:



```text id="5hv0si"

SYMBOL\_VOLUME\_MIN

SYMBOL\_VOLUME\_MAX

SYMBOL\_VOLUME\_STEP

```



Example conceptual process:



```text id="th39mq"

Calculated Risk-Based Volume

&#x20;       ↓

Clamp to Min/Max

&#x20;       ↓

Normalize to Volume Step

&#x20;       ↓

Validate

&#x20;       ↓

Send Order

```



Never send arbitrary lot sizes.



\---



\## 13. Risk-Based Lot Calculation



When risk-based sizing is required, the AI should calculate position size from:



```text id="v1ar71"

Account Risk

\+

Stop-Loss Distance

\+

Tick Size

\+

Tick Value

\+

Volume Constraints

```



Do not assume:



```text id="1v6ub3"

0.01 lot

```



always represents the same monetary risk across symbols.



\---



\## 14. Account Currency



Risk calculations must consider account currency.



Examples:



```text id="4vpwqf"

USD Account

IDR Account

USC Account

```



The monetary value of a trade must be calculated consistently with the actual account environment.



Do not blindly copy position-sizing formulas between account types.



\---



\## 15. CTrade



For ordinary EA development, `CTrade` is a preferred abstraction for common trading operations.



Include:



```mql5

\#include <Trade\\Trade.mqh>

```



Then:



```mql5

CTrade trade;

```



Configure important parameters during initialization, such as:



```text id="on9dm2"

Expert Magic Number

Allowed Deviation

Filling Policy

Margin Mode where required

```



MQL5 documents `CTrade` as a standard-library class providing convenient access to trading operations, including magic number, deviation, filling configuration, and trade-result checks.



\---



\## 16. Magic Number



Every EA strategy should normally use an explicit magic number.



Example:



```mql5

trade.SetExpertMagicNumber(MagicNumber);

```



The magic number is used to distinguish the EA's trades from:



\- manual trades

\- other EAs

\- other strategies



Position management should filter by both:



```text id="8b6rj4"

Symbol

Magic Number

```



unless the strategy intentionally manages another group of positions.



\---



\## 17. Deviation / Slippage



Where applicable, configure an execution deviation tolerance.



Example:



```mql5

trade.SetDeviationInPoints(Deviation);

```



The value must be interpreted according to the symbol's point size.



Do not describe a deviation value as "pips" without converting units correctly.



\---



\## 18. Filling Mode



Never blindly force:



```mql5

ORDER\_FILLING\_FOK

```



or:



```mql5

ORDER\_FILLING\_IOC

```



on every broker.



Prefer symbol-aware filling configuration when using `CTrade`.



Example:



```mql5

trade.SetTypeFillingBySymbol(\_Symbol);

```



MQL5 documents `SetTypeFillingBySymbol()` specifically for setting the order filling type according to the symbol's allowed settings.



This is particularly important for broker compatibility.



\---



\## 19. Trade Result Validation



An order method returning successfully does not mean the strategy should simply assume everything is complete.



After a trade request, inspect the relevant result information.



For `CTrade`, use methods such as:



```text id="x9f5hg"

ResultRetcode()

ResultRetcodeDescription()

ResultDeal()

ResultOrder()

```



where appropriate.



The EA should log meaningful failures.



\---



\## 20. Retcode-Driven Debugging



When an order fails, record:



```text id="6w2gxy"

Retcode

Retcode Description

Symbol

Volume

Price

SL

TP

Trade Mode

Execution Mode

Filling Mode

```



Do not fix execution errors by randomly changing strategy parameters.



First classify the problem:



```text id="j2l7nd"

Strategy Logic

Execution

Broker Restriction

Account Restriction

Platform Configuration

```



\---



\## 21. Order Check



Before sending complicated or high-risk trade requests, consider validating the request where appropriate.



The AI should distinguish between:



```text id="7e7f0d"

Request Validity

```



and:



```text id="b2cekr"

Actual Execution

```



A request can be valid while actual execution conditions later change.



\---



\## 22. Position Model



MQL5 supports different account position models.



In particular:



```text id="l9x1yq"

Netting

```



and:



```text id="l2js5z"

Hedging

```



behave differently.



In netting mode, only one position per symbol can exist at a time.



In hedging mode, multiple positions for the same symbol can exist.



MQL5 explicitly documents this distinction.



The EA architecture must know which account model it is targeting.



\---



\## 23. Position Enumeration



When managing multiple positions, enumerate positions carefully.



Typical tools include:



```text id="kzrfu2"

PositionsTotal()

PositionGetTicket()

PositionSelect()

PositionSelectByTicket()

PositionGetDouble()

PositionGetInteger()

PositionGetString()

```



`PositionGetTicket(index)` returns a position ticket and automatically selects that position for subsequent position-property access.



For fresh position data, explicitly select the position before reading its properties when necessary. MQL5 recommends selecting the position immediately before accessing its data.



\---



\## 24. Filtering Positions



When an EA manages only its own trades, filter positions by:



```text id="o8f4r5"

POSITION\_SYMBOL

POSITION\_MAGIC

POSITION\_TYPE

```



A common conceptual filter:



```text id="f9h3mc"

Same Symbol

AND

Same Magic

&#x20;       ↓

EA-Owned Position

```



Do not accidentally modify manual trades or positions belonging to another EA.



\---



\## 25. BUY and SELL Exclusivity



If a strategy requires:



```text id="r0fpln"

No BUY + SELL at the same time

```



then enforce this explicitly.



Do not rely on the strategy signal alone.



The position-management layer should verify existing exposure before opening a new position.



\---



\## 26. Maximum Positions



Every multi-entry EA must explicitly define:



```text id="xiz0jh"

Maximum Positions

Maximum Volume

Maximum Risk Exposure

Layer Distance

```



Never allow unlimited accumulation.



\---



\## 27. Entry Conditions



Separate the trading decision into stages:



```text id="8ygs7f"

Signal

&#x20;  ↓

Market Filters

&#x20;  ↓

Risk Check

&#x20;  ↓

Exposure Check

&#x20;  ↓

Execution Check

&#x20;  ↓

Order

```



Do not combine every condition into one unreadable `if` statement.



\---



\## 28. Market Filters



Possible filters include:



```text id="z4k9ck"

Spread

Session

Volatility

News

Trend

Time

Daily Loss

Maximum Positions

```



Each filter should have its own reason and ideally its own function.



\---



\## 29. Entry Cooldown



High-frequency strategies may need a cooldown.



Conceptually:



```text id="z1h0am"

Trade Executed

&#x20;     ↓

Cooldown Period

&#x20;     ↓

New Entries Allowed

```



The cooldown must be configurable if part of the strategy.



Do not add cooldown merely because it improved a backtest.



\---



\## 30. Stop Loss



Every production strategy should define how SL is calculated.



Possible methods:



```text id="uvn8fu"

Fixed Distance

ATR

Market Structure

Swing High / Low

Volatility

```



The EA should validate SL against broker stop/freeze restrictions before submission.



\---



\## 31. Take Profit



TP may be:



\- fixed distance

\- fixed risk/reward

\- structure-based

\- volatility-based

\- trailing

\- partial-exit based



The AI must preserve the strategy's intended exit logic.



\---



\## 32. Break-Even



Break-even must define:



```text id="c2eg9u"

Trigger

New Stop Location

Offset

Spread Consideration

Commission Consideration

One-Time vs Repeated Update

```



Example:



```text id="o3lq28"

Profit >= BETrigger

&#x20;      ↓

SL = Entry + BEOffset

```



Break-even must be tested rather than assumed beneficial.



\---



\## 33. Trailing Stop



Trailing logic must define:



```text id="8r4sl5"

Activation

Distance

Step

Direction

Update Frequency

```



Avoid unnecessary modifications on every tick.



Each modification should be validated.



\---



\## 34. Partial Close



If partial exits are used, define:



```text id="j7mgxy"

When partial close occurs

How much volume is closed

Minimum remaining volume

TP/SL behavior

Position accounting

```



Volume must still respect broker volume-step restrictions.



\---



\## 35. Time Filters



If the strategy uses trading sessions or time-based restrictions:



```text id="w1o2fh"

Identify Timezone

```



explicitly.



Do not assume:



```text id="0x1egb"

Local PC Time

=

Broker Server Time

```



Use the appropriate market/account/server time source for the intended logic.



\---



\## 36. Daily Statistics



If implementing daily loss protection, explicitly define:



```text id="l7f9tg"

Day Start Time

Reference Balance/Equity

Current Equity

Daily PnL

Daily Loss Limit

Reset Condition

```



Do not allow the protection state to reset unexpectedly after terminal restart.



Persistent state may be required depending on the design.



\---



\## 37. Equity Target Protection



If the strategy has an equity-growth target, define it mathematically.



Example:



```text id="e5xxcw"

Starting Balance = B



Target Percentage = P



Target Equity = B × (1 + P/100)

```



When:



```text id="x53p1p"

Equity >= Target Equity

```



the EA may:



```text id="a9nkmj"

Stop Opening New Trades

```



or execute another explicitly defined action.



Do not silently alter existing position-management logic.



\---



\## 38. Account Protection



Production EAs should consider:



```text id="3x3pc4"

Maximum Daily Loss

Maximum Drawdown

Maximum Exposure

Maximum Positions

Maximum Volume

Trading Cutoff

Emergency Stop

```



Protection logic should take precedence over new-entry logic.



\---



\## 39. News Filters



If a news filter is used, define:



```text id="vl2b4m"

Data Source

Currency/Event Selection

Pause Before Event

Pause After Event

Timezone

Behavior During Existing Positions

```



Do not fabricate news information in backtests.



If live news data is unavailable, explicitly state the limitation.



\---



\## 40. Error Handling



Do not write:



```mql5

trade.Buy(...);

```



and ignore the outcome.



Instead:



```text id="6v6m34"

Send Request

&#x20;   ↓

Check Result

&#x20;   ↓

Log Failure / Success

&#x20;   ↓

Update Internal State

```



Error handling is part of the EA, not an optional debugging feature.



\---



\## 41. Logging Standard



Useful log entries include:



```text id="p9g6k8"

Timestamp

Symbol

Bid

Ask

Spread

Signal

Direction

Volume

SL

TP

Magic Number

Retcode

Retcode Description

Position Ticket

Order Ticket

```



Logging should be detailed enough to reconstruct unexpected behavior.



\---



\## 42. Avoid Global State Abuse



Global variables can be useful, but do not use them to create hidden dependencies.



Prefer clearly defined state such as:



```text id="3l0t5z"

lastBarTime

lastTradeTime

dailyStartBalance

dailyTradingDisabled

indicatorHandle

```



Document what each persistent variable represents.



\---



\## 43. Modular Architecture



Prefer a structure similar to:



```text id="h74z4c"

EA

│

├── Initialization

│

├── Market Data

│

├── Signal Engine

│

├── Filters

│

├── Risk Manager

│

├── Position Sizing

│

├── Execution Engine

│

├── Position Manager

│

├── Protection

│

└── Diagnostics

```



The exact implementation may vary, but responsibilities should remain separated.



\---



\## 44. Strategy Layer vs Execution Layer



This distinction is critical.



\### Strategy Layer



Answers:



```text id="b9lx3p"

Should we BUY?

Should we SELL?

Should we stay out?

```



\### Execution Layer



Answers:



```text id="jap2sq"

Can we execute?

At what volume?

At what price?

With which filling mode?

Are SL/TP valid?

Was the order accepted?

```



Do not mix these responsibilities unnecessarily.



\---



\## 45. Backtest vs Live Behavior



The AI must recognize that Strategy Tester behavior may differ from live execution.



Potential differences include:



\- tick quality

\- spread

\- slippage

\- latency

\- execution

\- broker restrictions

\- liquidity

\- trading permissions



A backtest cannot automatically prove that live execution will behave identically.



\---



\## 46. Compilation Standard



Before considering MQL5 code complete:



```text id="4w5xzo"

Compile

&#x20;   ↓

Zero Errors

&#x20;   ↓

Review Warnings

&#x20;   ↓

Run Strategy Tester

&#x20;   ↓

Inspect Logs

&#x20;   ↓

Verify Trade Behavior

```



Warnings should not automatically be ignored.



\---



\## 47. Functional Testing



Test at minimum:



\### Entry



Does the EA enter when conditions are true?



\### No Entry



Does it correctly avoid entry when conditions are false?



\### SL/TP



Are they placed at the intended prices?



\### Position Limit



Does the EA respect the maximum?



\### Magic Number



Does it manage only its own trades?



\### Restart



Does behavior remain safe after terminal restart?



\### Market Closure



Does the EA handle unavailable trading conditions?



\### Spread Spike



Does the EA obey spread protection?



\### Error



Does it log rejected orders correctly?



\---



\## 48. Regression Testing



When modifying an existing EA:



```text id="8lq8a4"

Existing Behavior

&#x20;       ↓

Requested Modification

&#x20;       ↓

Compile

&#x20;       ↓

Re-Test Existing Features

&#x20;       ↓

Test New Feature

```



Do not assume that a new feature cannot break old functionality.



\---



\## 49. Code Modification Rule



When a user says:



```text id="5wpg7x"

"Add X without changing existing logic."

```



the AI must:



1\. Preserve existing signal logic.

2\. Preserve existing inputs unless explicitly requested.

3\. Add the minimum required code.

4\. Identify exactly what changed.

5\. Avoid opportunistic refactoring.

6\. Verify compilation and interactions.



\---



\## 50. Debugging Workflow



When an EA behaves incorrectly:



```text id="d3k8s7"

Observe

&#x20;  ↓

Collect Logs

&#x20;  ↓

Identify Layer

&#x20;  ↓

Reproduce

&#x20;  ↓

Fix

&#x20;  ↓

Compile

&#x20;  ↓

Re-Test

```



Classify the failure first:



```text id="vw0k1b"

Signal

Indicator

Data

Risk

Position Management

Execution

Broker

Account

Platform

```



\---



\## 51. Common MT5 Execution Problems



Potential problems include:



```text id="q3n7j4"

Unsupported Filling Mode

Invalid Stops

Invalid Volume

Invalid Price

Market Closed

Trade Disabled

AutoTrading Disabled

No Trading Permission

Insufficient Margin

Invalid Request

```



Do not assume an execution error is a strategy error.



Inspect the actual trade result and broker/account state.



\---



\## 52. Broker Snapshot



Before testing a production candidate, collect:



```text id="xplm5v"

Broker

Account Type

Account Currency

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

Stops Level

Freeze Level

Trade Mode

Trading Permission

```



Store the snapshot with the experiment.



\---



\## 53. XAUUSD-Specific Rule



For Gold systems, do not assume:



```text id="pk8q3h"

XAUUSD

```



always has the same specification.



The actual broker symbol may be:



```text id="x3c6w4"

XAUUSD

XAUUSDc

XAUUSDm

XAUUSD.s

```



The EA should operate on the actual chart symbol unless a deliberate symbol-mapping design is being used.



\---



\## 54. Indicator Development



Custom indicators should clearly define:



```text id="2o1pvc"

Inputs

Calculation

Buffers

Plots

Signal Conditions

Alerts

Repainting

Candle Confirmation

Timeframe

```



Avoid mixing trading execution into an indicator unless explicitly required.



\---



\## 55. Indicator Repainting



Whenever relevant, determine whether the indicator:



```text id="5w6ihk"

Recalculates Historical Signals

```



or:



```text id="k7s6fv"

Produces Stable Confirmed Signals

```



A visually perfect historical indicator may not produce identical real-time behavior.



The AI must disclose repainting behavior.



\---



\## 56. Data Validation



Before using price or indicator values, consider:



```text id="o5jgls"

Bars Available?

Data Loaded?

Handle Valid?

CopyBuffer Successful?

Values Finite?

```



Do not trade from invalid values.



\---



\## 57. Signal Confirmation



When a strategy requires candle-close confirmation:



```text id="pkb3pv"

Wait for Bar Close

&#x20;       ↓

Read Confirmed Data

&#x20;       ↓

Generate Signal

```



Do not accidentally read the still-forming candle.



\---



\## 58. Multi-Timeframe Systems



When an EA uses multiple timeframes, explicitly track:



```text id="f5d3uj"

Primary Timeframe

Higher Timeframe

Lower Timeframe

Synchronization

Bar Confirmation

Data Availability

```



Do not assume that a higher-timeframe candle is complete simply because a lower-timeframe tick arrived.



\---



\## 59. Multi-Symbol Systems



For multi-symbol EAs, explicitly manage:



```text id="ynhrmg"

Symbol List

Data Availability

Indicator Handles

Trade Permissions

Per-Symbol Risk

Per-Symbol Positions

Magic Numbers

```



Never assume `\_Symbol` is sufficient for every operation.



\---



\## 60. Performance



Avoid unnecessary:



\- indicator handle creation

\- repeated historical-data loading

\- expensive loops over all bars on every tick

\- excessive object creation

\- excessive trade modifications

\- excessive logging



Optimize only after correctness is established.



\---



\## 61. Security and Safety



Never embed sensitive credentials in MQL5 source code.



Do not store:



```text id="8hxqhz"

API Keys

Passwords

Private Tokens

Account Credentials

```



inside source files committed to GitHub.



Use secure configuration outside the repository where applicable.



\---



\## 62. Version Control



Every meaningful EA version should be associated with a Git commit.



Recommended workflow:



```text id="l1n6v6"

Modify

&#x20; ↓

Compile

&#x20; ↓

Test

&#x20; ↓

Commit

&#x20; ↓

Document Results

```



Never rely on filenames such as:



```text id="73gc2a"

EA\_final

EA\_final2

EA\_final\_FINAL

EA\_final2\_really\_final

```



Use Git history instead.



\---



\## 63. Strategy Versioning



Record a strategy version such as:



```text id="dmk5wl"

XAU\_EMA\_RSI\_v1.0

XAU\_EMA\_RSI\_v1.1

XAU\_EMA\_RSI\_v2.0

```



A significant change in:



\- signal logic

\- risk

\- execution

\- exit logic



should produce a new version.



\---



\## 64. EA Development Checklist



Before delivering an EA:



```text id="zv3xkf"

☐ Inputs validated

☐ OnInit validated

☐ Indicator handles validated

☐ Symbol specification checked

☐ Volume normalized

☐ Price normalized

☐ SL/TP validated

☐ Filling mode validated

☐ Magic number configured

☐ Position filtering implemented

☐ Trade results checked

☐ Errors logged

☐ Risk limits implemented

☐ Maximum positions implemented

☐ Compile successful

☐ Strategy Tester run

☐ Existing behavior regression-tested

```



\---



\## 65. AI Rules



When asked to create an MQL5 EA, the AI should first determine:



```text id="pz54si"

Market

Symbol

Timeframe

Strategy

Entry

Exit

Risk

Position Model

Execution Requirements

Broker Constraints

```



Then implement.



Do not immediately generate code when the strategy itself is ambiguous.



When important details are unknown, state assumptions explicitly.



\---



\## 66. AI Debugging Rules



When given an MQL5 error:



1\. Identify the actual error or retcode.

2\. Identify which subsystem produced it.

3\. Explain the likely cause.

4\. Inspect symbol/account constraints.

5\. Propose the smallest appropriate fix.

6\. Preserve unrelated strategy logic.

7\. Re-test after modification.



Do not guess based only on the visible symptom.



\---



\## 67. AI Code Quality Rules



Generated MQL5 should:



\- use meaningful names

\- use functions for repeated logic

\- minimize duplicate code

\- validate external inputs

\- handle errors

\- avoid magic numbers inside logic

\- avoid unnecessary globals

\- document non-obvious calculations

\- preserve strategy behavior

\- compile cleanly



Complexity should be justified.



\---



\## 68. Final Engineering Principle



A professional MQL5 EA consists of two major systems:



```text id="n4ws3j"

Trading Logic

```



and:



```text id="edh3mn"

Execution Infrastructure

```



Both must be correct.



Therefore:



```text id="h4em1o"

Valid Signal

\+

Correct Risk

\+

Correct Position Management

\+

Broker-Compatible Execution

\+

Verified Trade Result

\+

Reliable Protection

=

Production-Quality EA Candidate

```



A compiling EA is only the beginning.



A profitable backtest is also only evidence for further validation.



No MQL5 system should be considered production-ready without appropriate testing and risk controls.

