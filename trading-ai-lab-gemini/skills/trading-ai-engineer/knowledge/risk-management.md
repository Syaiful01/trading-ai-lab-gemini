\# Risk Management Framework



\## Purpose



This document defines the risk-management principles used throughout Trading AI Lab.



Risk management is more important than maximizing historical profit.



A strategy with lower returns and controlled drawdown is generally preferable to a strategy with higher returns but unstable risk.



\---



\## 1. Risk Per Trade



Every strategy must explicitly define the maximum amount of capital that may be lost on one trade.



Recommended default research range:



\- Conservative: 0.25%–0.50%

\- Moderate: 0.50%–1.00%

\- Aggressive: 1.00%–2.00%



The selected risk must be justified by the strategy, account size, volatility, and drawdown tolerance.



Do not increase position size merely because recent trades were profitable.



Do not increase risk to recover previous losses.



\---



\## 2. Position Sizing



Position size should be derived from predefined risk rather than arbitrary lot size whenever the instrument and broker specification allow it.



Conceptually:



```text

Position Size =

Maximum Monetary Risk

/

Monetary Loss Per Unit At Stop Loss

```



For MT5, calculation may need to account for:



\- tick size

\- tick value

\- contract size

\- symbol digits

\- point size

\- stop-loss distance

\- lot step

\- minimum lot

\- maximum lot



Never assume that 1 point or 1 pip has the same monetary value across symbols or brokers.



\---



\## 3. Stop Loss



A stop loss must have a logical relationship to the strategy.



Possible approaches include:



\- structural stop

\- volatility-based stop

\- ATR-based stop

\- fixed-distance stop

\- time-based invalidation



The stop-loss method should match the strategy's market hypothesis.



Avoid selecting a stop distance solely because it produces a better historical backtest.



\---



\## 4. Take Profit



Take-profit rules must be tested together with the entry and stop-loss logic.



Possible approaches:



\- fixed risk/reward

\- structural target

\- volatility target

\- trailing exit

\- partial profit-taking

\- dynamic exit



A nominal 1:2 risk/reward ratio does not guarantee positive expectancy.



The actual distribution of winning and losing trades must be measured.



\---



\## 5. Expectancy



A strategy should be evaluated using expected value rather than win rate alone.



Conceptually:



```text

Expectancy =

(Win Rate × Average Win)

\-

(Loss Rate × Average Loss)

```



A strategy can have a relatively low win rate and still be profitable when average wins significantly exceed average losses.



A high win rate does not automatically indicate a good strategy.



\---



\## 6. Maximum Drawdown



Every strategy must define an acceptable maximum drawdown before testing.



Track at minimum:



\- maximum equity drawdown

\- maximum balance drawdown

\- daily drawdown

\- consecutive losing trades



A strategy that produces acceptable profit but unacceptable drawdown should be rejected or redesigned.



\---



\## 7. Daily Loss Protection



Automated systems should support a daily loss limit where appropriate.



Example:



```text

Daily Loss Limit = predefined percentage of starting daily equity

```



When the limit is reached:



```text

Stop opening new trades

```



Existing positions should only be managed according to previously defined exit rules.



Do not immediately restart trading simply because market conditions appear to improve.



\---



\## 8. Maximum Exposure



Define limits for:



\- simultaneous trades

\- total position exposure

\- directional exposure

\- correlated positions

\- symbol exposure



Multiple trades in correlated markets can represent one large hidden risk.



For example:



```text

BTCUSD

ETHUSD

SOLUSD

```



may create substantially correlated crypto exposure during certain market regimes.



\---



\## 9. No Uncontrolled Recovery Systems



The default framework rejects uncontrolled:



\- martingale

\- loss doubling

\- revenge trading

\- unlimited averaging

\- unlimited grid expansion



Any recovery mechanism requires independent justification and stress testing.



A strategy must not assume that the next trade will recover the previous loss.



\---



\## 10. Risk of Ruin



Backtesting should consider the possibility of prolonged losing streaks.



At minimum, test:



\- consecutive losses

\- randomized trade order

\- adverse slippage

\- increased spread

\- reduced win rate

\- lower average reward

\- higher transaction costs



A strategy should be evaluated under unfavorable but plausible conditions.



\---



\## 11. Execution Risk



Backtest assumptions must not ignore execution.



Consider:



\- spread

\- commission

\- slippage

\- latency

\- rejected orders

\- requotes where applicable

\- partial fills

\- stop-level restrictions

\- freeze-level restrictions

\- market closure

\- abnormal volatility



For MT5, symbol and trading specifications must be read from the broker rather than hard-coded from assumptions.



\---



\## 12. XAUUSD Risk Considerations



XAUUSD can experience rapid price movement and changing spreads.



An EA trading XAUUSD should verify:



```text

Symbol

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

Execution Mode

Filling Mode

Trade Mode

Spread

```



These values may differ between brokers and account types.



Never hard-code assumptions that can be read from the symbol specification.



\---



\## 13. Crypto Risk Considerations



Crypto systems should consider:



\- exchange fees

\- funding rates

\- slippage

\- liquidity

\- minimum order quantity

\- quantity precision

\- price precision

\- leverage

\- liquidation risk

\- API failures

\- rate limits

\- partial fills



Leverage increases the speed at which both profits and losses occur.



The system must calculate risk from the actual position and stop-loss relationship rather than from leverage alone.



\---



\## 14. Position Limits



Every automated strategy should define explicit limits such as:



```text

Max Positions

Max Risk Per Trade

Max Daily Loss

Max Portfolio Risk

Max Symbol Exposure

Max Consecutive Loss Response

```



These must be configurable rather than hidden inside the code.



\---



\## 15. Equity Protection



For production EAs, consider hard protection mechanisms such as:



```text

Equity < Protection Level

&#x20;       ↓

Stop Opening New Trades

```



Optionally:



```text

Severe Equity Breach

&#x20;       ↓

Close Positions

&#x20;       ↓

Disable Trading

```



The exact behavior must be explicitly documented before implementation.



\---



\## 16. Risk Scaling



Do not automatically increase risk because a strategy is currently profitable.



Risk scaling should require evidence from:



\- out-of-sample results

\- forward testing

\- robustness testing

\- stable drawdown

\- stable expectancy

\- sufficient sample size



Risk should generally be reduced when the strategy enters a statistically uncertain or unstable regime.



\---



\## 17. Backtest Stress Testing



Every serious strategy should eventually be tested with unfavorable assumptions.



Examples:



```text

Normal Spread

&#x20;       vs

Wider Spread



Normal Slippage

&#x20;       vs

Adverse Slippage



Original Win Rate

&#x20;       vs

Reduced Win Rate



Original Profit Target

&#x20;       vs

Reduced Profit Target

```



A strategy that completely collapses under small realistic changes may be over-optimized.



\---



\## 18. Risk Management Priority



The development priority is:



```text

Capital Preservation

&#x20;       ↓

Controlled Risk

&#x20;       ↓

Robustness

&#x20;       ↓

Consistency

&#x20;       ↓

Profitability

```



Profitability must never override predefined risk limits.



\---



\## 19. AI Rules



When reviewing or building a trading system, the AI must always ask:



1\. How much can be lost on one trade?

2\. How much can be lost in one day?

3\. What is the maximum expected drawdown?

4\. What happens during a losing streak?

5\. What happens when spread increases?

6\. What happens when slippage increases?

7\. What happens when the win rate decreases?

8\. What happens when market conditions change?

9\. Is position sizing based on actual instrument specifications?

10\. Is the strategy still viable after realistic trading costs?



The AI must never prioritize a higher backtest profit at the expense of uncontrolled risk.



\---



\## 20. Default Research Principle



Unless a strategy explicitly requires otherwise:



```text

Small Risk

\+

Defined Stop

\+

Defined Position Size

\+

Defined Daily Loss Limit

\+

Defined Maximum Drawdown

\+

Realistic Trading Costs

=

Acceptable Starting Framework

```



This framework does not guarantee profitability.



It exists to prevent strategy research from becoming uncontrolled risk-taking.

