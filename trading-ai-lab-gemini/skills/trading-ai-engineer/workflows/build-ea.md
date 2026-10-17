\# EA Development Workflow



\## Purpose



Use this workflow whenever the user asks to create, modify, debug, optimize, or prepare an Expert Advisor for MetaTrader 5.



The goal is to produce an EA whose strategy logic and execution infrastructure are both correct.



\---



\# 1. Identify Requirements



Extract:



\- Market

\- Symbol

\- Timeframe

\- Strategy

\- Entry

\- Exit

\- Stop Loss

\- Take Profit

\- Risk

\- Maximum Positions

\- Filters

\- Session

\- No-Trade Conditions

\- Execution Requirements



Do not invent unspecified strategy rules.



\---



\# 2. Read Relevant Knowledge



For MT5 EA development, use:



```text

knowledge/mql5.md

knowledge/risk-management.md

knowledge/backtesting.md

```



For XAUUSD also use:



```text

knowledge/xauusd.md

knowledge/market-structure.md

knowledge/strategy-development.md

```



For crypto through MT5 where applicable, use the crypto resource as well.



\---



\# 3. Preserve Existing Logic



When modifying an existing EA:



```text

Read Existing Code

&#x20;       ↓

Identify Current Behavior

&#x20;       ↓

Identify Requested Change

&#x20;       ↓

Modify Minimum Required Components

&#x20;       ↓

Regression Review

```



Do not rewrite unrelated logic unless necessary.



\---



\# 4. Define EA Architecture



Prefer:



```text

Market Data

&#x20;   ↓

Signal Engine

&#x20;   ↓

Filters

&#x20;   ↓

Risk Manager

&#x20;   ↓

Position Sizing

&#x20;   ↓

Execution Engine

&#x20;   ↓

Position Manager

&#x20;   ↓

Protection

&#x20;   ↓

Logging

```



Avoid putting the entire system into one huge `OnTick()` function.



\---



\# 5. Initialization



During `OnInit()` validate:



```text

Inputs

Symbol

Indicator Handles

Trade Object

Magic Number

Symbol Properties

Risk Configuration

Required Data

```



If required initialization fails:



```text

Log Error

↓

Return Initialization Failure

```



Do not start normal trading with invalid resources.



\---



\# 6. Indicator Handling



Where applicable:



```text

Create Indicator Handles During Initialization

↓

Validate Handles

↓

Read Data Using CopyBuffer

↓

Validate Returned Data

↓

Generate Signal

```



Do not recreate indicator handles on every tick without a specific reason.



\---



\# 7. Candle Logic



Explicitly identify whether the strategy uses:



```text

Current Candle

```



or:



```text

Last Closed Candle

```



If one signal per candle is intended, implement new-bar detection.



Do not accidentally generate duplicate entries on every tick.



\---



\# 8. Signal Engine



Keep signal logic separate.



Example:



```mql5

bool BuySignal()

bool SellSignal()

```



The signal engine should answer only:



```text

BUY

SELL

NO TRADE

```



Do not place risk-management logic inside signal conditions unnecessarily.



\---



\# 9. Filters



Separate filters such as:



```text

Spread

Session

Volatility

Trend

News

Daily Loss

Position Limit

Cooldown

```



Each filter should have a documented purpose.



\---



\# 10. Risk Manager



Before opening a trade, validate:



```text

Risk Per Trade

Daily Loss

Maximum Drawdown

Maximum Exposure

Maximum Positions

Position Direction

Volume

Margin

```



Risk checks should happen before execution.



\---



\# 11. Position Sizing



If using fixed lot:



```text

Validate Minimum

Validate Maximum

Validate Volume Step

```



If using risk-based sizing:



```text

Account Risk

↓

Stop Distance

↓

Tick Size

↓

Tick Value

↓

Raw Volume

↓

Volume Normalization

↓

Broker Validation

```



Never assume one lot has the same monetary risk across instruments.



\---



\# 12. Symbol Specification



Before execution, inspect where relevant:



```text

Digits

Point

Tick Size

Tick Value

Contract Size

Volume Min

Volume Max

Volume Step

Stops Level

Freeze Level

Trade Mode

Execution Mode

Filling Mode

Spread

```



Use the actual symbol properties.



\---



\# 13. Price Validation



Before placing an order:



```text

Entry Price

Stop Loss

Take Profit

```



must be checked against:



```text

Digits

Tick Size

Stops Level

Freeze Level

Current Market Price

```



\---



\# 14. Execution Configuration



Configure:



```text

Magic Number

Allowed Deviation

Filling Type

```



Where using `CTrade`, prefer symbol-aware filling configuration when appropriate.



Do not force a filling policy blindly.



\---



\# 15. Trade Execution



Conceptually:



```text

Signal

↓

Risk Check

↓

Exposure Check

↓

Execution Validation

↓

Send Order

↓

Inspect Result

↓

Log Result

```



A signal is not the same thing as a successful trade.



\---



\# 16. Trade Result



After execution inspect relevant result information.



At minimum record:



```text

Retcode

Retcode Description

Order Ticket

Deal Ticket

```



when available.



Do not silently ignore execution failure.



\---



\# 17. Position Management



Separate management of existing trades from new-entry logic.



Possible components:



```text

Break-Even

Trailing Stop

Partial Close

Time Exit

Emergency Exit

```



Each should be independently understandable.



\---



\# 18. Position Filtering



When managing EA positions, filter appropriately using:



```text

Symbol

Magic Number

Position Type

```



Do not accidentally modify positions belonging to:



```text

Manual Trading

Another EA

Another Strategy

```



\---



\# 19. Position Model



Determine whether the target account uses:



```text

Netting

```



or:



```text

Hedging

```



The EA must be designed consistently with the target account model.



\---



\# 20. Multiple Positions



If multiple entries are allowed, define:



```text

Maximum Positions

Maximum Total Volume

Maximum Total Risk

Layer Distance

Same Direction Limit

Opposite Direction Policy

```



Never allow unlimited position accumulation.



\---



\# 21. BUY / SELL Exclusivity



If simultaneous BUY and SELL positions are forbidden:



```text

Check Existing Exposure

↓

Reject Opposite Entry

```



Do not rely only on signal logic.



\---



\# 22. Break-Even



Define:



```text

Trigger

New Stop Price

Offset

Update Frequency

```



Ensure the new SL respects broker restrictions.



Do not assume break-even improves performance before testing.



\---



\# 23. Trailing Stop



Define:



```text

Activation

Distance

Step

Direction

```



Avoid unnecessary modifications on every tick.



\---



\# 24. Daily Protection



When implementing daily limits, explicitly track:



```text

Daily Start

Reference Balance / Equity

Current Equity

Daily PnL

Daily Loss Limit

Trading Disabled State

```



The reset behavior should remain safe after terminal restart.



\---



\# 25. Equity Target



If the user specifies an equity-growth target:



```text

Target Equity =

Starting Balance × (1 + Target Percentage)

```



When the target is reached:



```text

Stop New Entries

```



unless the user explicitly requests another behavior.



Do not silently change unrelated position management.



\---



\# 26. Emergency Protection



Production candidates may include:



```text

Maximum Drawdown

Daily Loss

Maximum Exposure

Maximum Positions

Abnormal Spread

Execution Failure

Manual Emergency Stop

```



Protection must take precedence over new-entry logic.



\---



\# 27. Logging



Log meaningful state such as:



```text

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

Ticket

```



Avoid both:



```text

No useful logging

```



and:



```text

Excessive meaningless logging

```



\---



\# 28. Error Diagnosis



Classify problems before fixing them:



```text

Strategy

Indicator

Data

Risk

Position

Execution

Broker

Account

Platform

```



Do not change strategy logic to solve broker-specific execution problems.



\---



\# 29. Compilation



Before delivery:



```text

Compile

↓

Zero Errors

↓

Review Warnings

↓

Strategy Tester

↓

Inspect Logs

```



\---



\# 30. Functional Testing



Test:



```text

Entry

No Entry

SL

TP

Maximum Positions

Magic Number

BUY / SELL Rules

Break-Even

Trailing

Daily Protection

Restart

Market Closure

Spread Protection

Execution Errors

```



\---



\# 31. Regression Testing



For an existing EA:



```text

Old Feature

↓

New Feature

↓

Compile

↓

Old Feature Retest

↓

New Feature Retest

```



Never assume a new feature cannot break existing behavior.



\---



\# 32. Backtesting



The EA should be tested with:



```text

Defined Period

Defined Symbol

Defined Timeframe

Defined Costs

Defined Risk

Defined Parameters

```



Record the exact configuration.



\---



\# 33. Delivery



When providing the EA, include:



```text

File Name

Inputs

Main Logic

Important Assumptions

Testing Procedure

Known Limitations

```



If existing code was modified, list:



```text

What Changed

What Was Preserved

```



\---



\# 34. Final Rule



A successful EA development process is:



```text

Correct Strategy

\+

Correct Risk

\+

Correct Execution

\+

Correct Position Management

\+

Correct Protection

\+

Verified Testing

```



A compiling EA is not automatically a production-ready trading system.

