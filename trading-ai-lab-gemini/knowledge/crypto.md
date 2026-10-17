\# Cryptocurrency Trading \& Engineering Framework



\## Purpose



This document defines the research and engineering standards for cryptocurrency algorithmic trading.



Crypto must be treated as a distinct market from XAUUSD and traditional centralized-market instruments.



The AI must not assume that crypto exchanges, spot markets, futures markets, and MetaTrader behave the same way.



\---



\## 1. Supported Crypto Markets



Possible market types include:



\- Spot

\- Margin

\- Perpetual Futures

\- Delivery Futures

\- Options where supported



The strategy must explicitly identify the market type.



Do not mix assumptions between:



```text

Spot

```



and:



```text

Perpetual / Futures

```



because leverage, funding, liquidation, and position mechanics can differ.



\---



\## 2. Exchange Abstraction



A crypto strategy should separate:



```text

Strategy Logic

&#x20;     ↓

Risk Management

&#x20;     ↓

Execution Layer

&#x20;     ↓

Exchange API

```



The strategy itself should not contain unnecessary exchange-specific assumptions.



Whenever possible, exchange-specific behavior should be isolated in the execution layer.



\---



\## 3. Exchange Specification



Before live trading, determine the actual exchange specifications.



Important parameters may include:



```text

Symbol

Base Asset

Quote Asset

Market Type

Minimum Order Size

Maximum Order Size

Quantity Precision

Price Precision

Tick Size

Step Size

Maker Fee

Taker Fee

Funding Rules

Leverage Limits

Margin Rules

Order Types

Position Modes

```



Never assume that every exchange uses identical rules.



\---



\## 4. Spot vs Perpetual Futures



\### Spot



Spot trading normally represents direct ownership of the underlying asset.



Risk considerations include:



\- trading fees

\- spread

\- slippage

\- liquidity

\- withdrawal/deposit constraints where relevant



\### Perpetual Futures



Perpetual contracts introduce additional considerations:



\- leverage

\- initial margin

\- maintenance margin

\- liquidation

\- funding payments

\- position mode

\- isolated/cross margin

\- mark price

\- index price



A strategy must explicitly account for these variables when relevant.



\---



\## 5. Leverage



Leverage must not be interpreted as additional risk budget.



For example:



```text

10x leverage

```



does not mean the strategy should automatically risk ten times more capital.



Risk should be determined from:



```text

Position Size

\+

Entry Price

\+

Stop Loss

\+

Contract Specifications

```



Leverage primarily affects the capital required to support a position and can accelerate liquidation risk.



\---



\## 6. Position Sizing



Position sizing should be based on predefined monetary or percentage risk whenever practical.



Conceptually:



```text

Maximum Risk

&#x20;       ÷

Expected Loss Per Unit At Stop

&#x20;       =

Position Size

```



Then enforce exchange-specific:



```text

Minimum Quantity

Maximum Quantity

Step Size

Precision

Notional Limits

```



Never submit quantities with invalid precision.



\---



\## 7. Fees



Crypto strategies must account for transaction costs.



Consider:



\- maker fees

\- taker fees

\- spread

\- slippage

\- funding

\- borrowing costs where applicable



For high-frequency or scalping strategies, transaction costs can become one of the dominant factors determining expectancy.



A strategy must be evaluated after realistic costs.



\---



\## 8. Funding



Perpetual futures can involve periodic funding payments.



Funding may affect:



\- holding cost

\- realized PnL

\- strategy expectancy

\- long-term position carry



For strategies that hold positions across funding periods, funding should be included in the research model.



Do not assume funding is always negligible.



\---



\## 9. Liquidation



Leveraged positions must account for liquidation risk.



The system should distinguish:



```text

Strategy Stop Loss

```



from:



```text

Exchange Liquidation

```



A strategy must never intentionally rely on liquidation as a risk-management mechanism.



The liquidation price should remain sufficiently distant from the normal strategy invalidation level.



\---



\## 10. Margin Mode



Where supported, explicitly identify:



```text

Isolated Margin

Cross Margin

```



These have different portfolio-risk implications.



A strategy designed for isolated margin should not silently be deployed under cross margin without evaluating the consequences.



\---



\## 11. Position Mode



Some exchanges support different position modes, such as:



```text

One-Way Mode

Hedge Mode

```



The execution layer must know which mode is active.



Do not assume that opening a BUY and opening a SELL always produces independent positions.



\---



\## 12. Order Types



Possible order types include:



```text

Market

Limit

Stop

Stop-Limit

Take-Profit

Take-Profit-Limit

Conditional Orders

```



The strategy must explicitly define which order type is required.



Do not substitute order types merely because an exchange implementation makes one easier.



\---



\## 13. Market Orders



Market orders prioritize execution over exact price.



They are exposed to:



\- spread

\- slippage

\- liquidity

\- market impact



Market-order strategies should measure realistic execution cost.



\---



\## 14. Limit Orders



Limit orders provide price control but introduce execution uncertainty.



A backtest should not automatically assume that every limit order is filled.



Where realistic simulation is possible, consider:



\- whether price actually traded through the order

\- queue position where relevant

\- partial fill

\- cancellation

\- latency

\- adverse selection



Avoid unrealistic assumptions such as:



```text

If candle low touches limit price,

order is always perfectly filled.

```



\---



\## 15. Slippage



Crypto execution can differ significantly from the intended entry or exit price.



Track:



```text

Expected Price

Actual Fill Price

Slippage

```



Stress test the strategy under:



```text

Normal Slippage

Moderate Adverse Slippage

Severe Adverse Slippage

```



A short-term strategy that depends on near-zero slippage should receive extra scrutiny.



\---



\## 16. Liquidity



Liquidity matters greatly in crypto.



Consider:



\- order-book depth

\- daily volume

\- spread

\- market impact

\- liquidity during volatility spikes



A strategy should not trade large position sizes in thin markets merely because historical candles appear tradable.



Backtests based only on OHLC data may fail to represent true execution constraints.



\---



\## 17. Volatility



Crypto volatility changes across:



\- market regimes

\- assets

\- sessions

\- news events

\- liquidation cascades



Possible volatility measurements:



\- ATR

\- realized volatility

\- standard deviation

\- range expansion

\- volatility percentile



A volatility filter should exist because of a defined hypothesis, not simply because it improves historical results.



\---



\## 18. Market Regimes



Crypto strategies should be tested across:



```text

Bull Trend

Bear Trend

Sideways Range

High Volatility

Low Volatility

Breakout

Post-Breakout

Crash

Recovery

```



A strategy that only works in one regime must be classified as regime-dependent.



\---



\## 19. Correlation Risk



Crypto assets can become highly correlated during market stress.



Examples:



```text

BTC

ETH

SOL

Other Large-Cap Altcoins

```



Multiple independent-looking positions may represent one concentrated market bet.



Portfolio risk should therefore consider correlation.



\---



\## 20. Stablecoin and Quote-Currency Risk



The strategy must distinguish the quote asset.



Examples:



```text

USDT

USDC

BTC

ETH

USD

```



Do not assume that all quote assets have identical liquidity, fees, or accounting behavior.



\---



\## 21. API Reliability



Automated crypto systems depend on APIs.



The execution system should handle:



\- connection failure

\- timeout

\- rate limits

\- authentication errors

\- invalid requests

\- temporary exchange errors

\- order rejection

\- partial fill

\- duplicated requests

\- delayed responses



The strategy must not assume that every API request succeeds immediately.



\---



\## 22. Idempotency



Where supported, order requests should use unique client order identifiers.



This helps prevent accidental duplicate orders when:



```text

Request Sent

&#x20;   ↓

Response Delayed

&#x20;   ↓

Client Unsure Whether Order Was Accepted

```



The system should verify order status before retrying blindly.



\---



\## 23. Retry Policy



Retries must be categorized.



Safe-to-retry examples may include:



\- temporary network failure

\- timeout where order status can be checked first



Potentially unsafe retries include:



\- uncertain market order submission

\- duplicated conditional order

\- state-changing request whose previous result is unknown



Never blindly repeat an order request without determining whether the original request was executed.



\---



\## 24. Rate Limits



Exchange APIs may impose request limits.



The system should respect:



\- request frequency

\- websocket limits

\- REST limits

\- endpoint-specific restrictions



Design data collection and order management to minimize unnecessary API calls.



\---



\## 25. Data Sources



Possible crypto data sources include:



\- exchange REST APIs

\- exchange WebSocket APIs

\- historical datasets

\- third-party market-data providers



Record the exact data source used for every meaningful backtest.



Do not mix data sources without documenting the change.



\---



\## 26. Candle Data



For candle-based strategies, verify:



```text

Timestamp

Open

High

Low

Close

Volume

```



Also verify:



\- timezone

\- candle completeness

\- duplicate timestamps

\- missing candles

\- exchange-specific candle boundaries



Do not assume all exchanges define candles identically.



\---



\## 27. Intrabar Assumptions



OHLC candles do not reveal the exact path price took inside the candle.



If a backtest assumes a specific order of:



```text

High

Low

Entry

Stop

Target

```



the assumption must be documented.



This matters especially for strategies with tight stops and targets.



\---



\## 28. Look-Ahead Bias



Crypto backtests must not use future information.



Check for:



\- future candle data

\- future volume

\- future highs/lows

\- improperly shifted indicators

\- future funding information

\- future regime labels

\- leaked features in machine learning



All features used to generate a trade must have been available at the decision time.



\---



\## 29. Repainting



Determine whether an indicator or signal changes after the candle closes.



Clearly distinguish:



```text

Real-Time Signal

```



from:



```text

Confirmed Candle-Close Signal

```



A historical chart signal that changes retroactively is not equivalent to a live tradable signal.



\---



\## 30. Backtesting Metrics



Record at minimum:



```text

Net Profit

Total Trades

Win Rate

Profit Factor

Maximum Drawdown

Average Win

Average Loss

Expectancy

Average Trade

Maximum Consecutive Losses

Trade Frequency

```



Where relevant, also evaluate:



```text

Fees

Funding

Slippage

Liquidity Assumptions

Turnover

Exposure

```



\---



\## 31. Strategy Robustness



Test whether the strategy survives changes in:



```text

Fees

Spread

Slippage

Parameters

Market

Timeframe

Trading Period

Volatility

Execution Delay

```



A strategy that only works under one exact configuration should be treated as fragile.



\---



\## 32. Cross-Asset Validation



Where appropriate, evaluate whether the core hypothesis survives across related assets.



For example:



```text

BTCUSDT

ETHUSDT

SOLUSDT

```



However, do not automatically assume that success across multiple assets proves robustness.



Assets can share market regimes and correlations.



Cross-asset testing should support the hypothesis, not replace independent validation.



\---



\## 33. Cross-Timeframe Validation



Possible testing:



```text

1m

5m

15m

1h

4h

```



Do not optimize the strategy across every available timeframe and then select the best result without accounting for multiple testing.



The timeframe should be chosen based on the trading hypothesis.



\---



\## 34. Backtest → Forward Test



After historical testing:



```text

Backtest

&#x20;   ↓

Out-of-Sample

&#x20;   ↓

Forward / Paper Trading

```



Forward testing should use market data that was not used to tune the strategy.



Track:



\- real execution

\- actual fees

\- actual slippage

\- signal timing

\- API behavior

\- rejected orders

\- latency



\---



\## 35. Crypto Strategy Categories



Potential strategy families include:



```text

Trend Following

Momentum

Breakout

Mean Reversion

Market Making

Arbitrage

Funding Strategies

Statistical Arbitrage

Volatility Strategies

Multi-Timeframe

Order-Flow Strategies

```



The AI should first understand the hypothesis before selecting technical indicators.



\---



\## 36. Machine Learning / AI Strategies



Machine-learning strategies require additional controls against:



\- data leakage

\- target leakage

\- overfitting

\- feature selection bias

\- survivorship bias

\- non-stationarity

\- regime changes



A model with high historical classification accuracy does not automatically represent a profitable trading system.



Evaluate the model through actual trading outcomes and costs.



\---



\## 37. Live Safety



A crypto trading bot should support a safe lifecycle:



```text

Backtest

&#x20;  ↓

Dry Run

&#x20;  ↓

Paper / Demo

&#x20;  ↓

Small Capital

&#x20;  ↓

Monitored Live

```



Do not move directly from an optimized backtest to full capital deployment.



\---



\## 38. Emergency Controls



Production systems should have explicit controls such as:



```text

Maximum Daily Loss

Maximum Drawdown

Maximum Position Size

Maximum Number Of Orders

Maximum Exposure

API Error Shutdown

Abnormal Spread Protection

Abnormal Volatility Protection

Manual Emergency Stop

```



When a safety threshold is triggered:



```text

Stop New Entries

```



and preserve sufficient state information for diagnosis.



\---



\## 39. Logging



The execution layer should record:



```text

Timestamp

Exchange

Symbol

Side

Order Type

Requested Price

Filled Price

Quantity

Fees

Slippage

Order ID

Client Order ID

Status

Error Code

Error Message

Latency

```



Logs should be sufficient to reconstruct what happened.



\---



\## 40. Research Journal



Every crypto experiment should record:



```text

Experiment ID:

Date:



Market:

Exchange:

Instrument Type:



Strategy:



Hypothesis:



Entry Rules:



Exit Rules:



Risk Rules:



Position Sizing:



Fees:



Funding Assumption:



Slippage Assumption:



Backtest Period:



Results:



Profit Factor:



Win Rate:



Maximum Drawdown:



Expectancy:



Weaknesses:



Potential Overfitting:



Conclusion:



Next Action:

```



\---



\## 41. Common Failure Modes



When a crypto strategy fails, classify the problem.



Possible categories:



```text

Strategy Failure

Risk Failure

Execution Failure

Data Failure

API Failure

Liquidity Failure

Cost Failure

Overfitting

Regime Change

Implementation Bug

```



Do not automatically modify strategy logic when the real problem is execution or infrastructure.



\---



\## 42. AI Rules for Crypto



When designing or reviewing a crypto system, verify:



1\. What market type is being traded?

2\. What exchange is being used?

3\. What are the symbol specifications?

4\. What are the fees?

5\. Is funding relevant?

6\. What is the minimum order size?

7\. What precision is required?

8\. What is the expected slippage?

9\. What is the liquidity?

10\. What happens if the API fails?

11\. What happens if an order is partially filled?

12\. What happens if an order submission times out?

13\. Can a retry create a duplicate order?

14\. What is the liquidation risk?

15\. What is the maximum account-level risk?



Unknown values must be identified explicitly rather than assumed.



\---



\## 43. Final Principle



Crypto algorithmic trading requires two separate validations:



```text

Statistical Edge

\+

Execution Reliability

```



A strategy with a theoretical edge can fail because of:



\- fees

\- funding

\- slippage

\- liquidity

\- API failure

\- execution delay

\- liquidation

\- exchange rules



Therefore:



```text

Trading Hypothesis

\+

Robust Strategy

\+

Realistic Cost Model

\+

Reliable Execution

\+

Strict Risk Control

=

Production Candidate

```



This equation does not guarantee profitability.



It defines the minimum engineering and research mindset required before live deployment.

