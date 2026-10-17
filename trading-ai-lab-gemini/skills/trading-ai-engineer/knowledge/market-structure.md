\# Market Structure \& Market Context Framework



\## Purpose



This document defines how the AI should reason about market structure before designing or evaluating a trading strategy.



The AI should not treat indicators as the market itself.



Indicators are transformations of price, volume, volatility, or other market data.



Market structure should be evaluated first.



\---



\## 1. Core Principle



The strategy development process should generally follow:



```text

Market Context

&#x20;     ↓

Market Structure

&#x20;     ↓

Market Regime

&#x20;     ↓

Trading Hypothesis

&#x20;     ↓

Setup

&#x20;     ↓

Trigger

&#x20;     ↓

Risk

&#x20;     ↓

Execution

```



Do not start with:



```text

"Which indicator gives the best signal?"

```



Start with:



```text

"What behavior are we attempting to exploit?"

```



\---



\## 2. Market Structure



Market structure describes how price evolves through:



\- swing highs

\- swing lows

\- higher highs

\- higher lows

\- lower highs

\- lower lows

\- breakouts

\- failed breakouts

\- ranges

\- expansion

\- contraction



The AI should distinguish between local structure and higher-level structure.



A move that appears bullish on a low timeframe may exist inside a bearish higher-timeframe structure.



\---



\## 3. Trend



A directional trend can be described through sequential structural behavior.



\### Bullish Structure



Conceptually:



```text

Higher High

Higher Low

Higher High

Higher Low

```



\### Bearish Structure



Conceptually:



```text

Lower Low

Lower High

Lower Low

Lower High

```



The AI should avoid defining a trend using a single candle or isolated indicator value.



Trend determination should consider the relevant timeframe and structure.



\---



\## 4. Range



A ranging market generally contains repeated movement between identifiable areas of support and resistance without sustained directional expansion.



Conceptually:



```text

Resistance

────────────────

&#x20;    ↑

&#x20;    │

&#x20;    ↓

Support

────────────────

```



Range strategies and trend strategies should not be evaluated under identical assumptions.



A trend-following strategy may naturally perform poorly during prolonged ranges.



\---



\## 5. Expansion and Contraction



Markets alternate between periods of:



```text

Volatility Contraction

&#x20;       ↓

Volatility Expansion

&#x20;       ↓

New Equilibrium

&#x20;       ↓

Contraction

```



Possible measurements include:



\- ATR

\- candle range

\- standard deviation

\- realized volatility

\- Bollinger Band width

\- historical volatility percentile



The AI should treat volatility as contextual information rather than automatically as an entry signal.



\---



\## 6. Breakout



A breakout occurs when price moves beyond a previously relevant structural boundary.



Potential breakout references:



\- range high

\- range low

\- swing high

\- swing low

\- consolidation boundary

\- previous session high/low



The AI must distinguish:



```text

Breakout

```



from:



```text

False Breakout

```



and:



```text

Breakout + Acceptance

```



when the strategy hypothesis depends on sustained movement.



\---



\## 7. Breakout Confirmation



Confirmation may be based on:



\- candle close

\- multiple closes

\- volume

\- volatility expansion

\- retest

\- higher-timeframe alignment

\- continuation structure



No confirmation method should be treated as universally superior.



It must be tested against the intended market behavior.



\---



\## 8. Breakout Retest



A common structural hypothesis is:



```text

Range

&#x20;  ↓

Breakout

&#x20;  ↓

Retest

&#x20;  ↓

Continuation

```



A retest strategy must define:



\- what qualifies as the broken level

\- acceptable retest depth

\- maximum retest duration

\- confirmation trigger

\- invalidation

\- stop placement



Avoid discretionary phrases such as:



```text

"Price retests the zone nicely."

```



Convert them into measurable rules.



\---



\## 9. Pullback



A pullback is a temporary counter-move within a larger directional context.



Conceptually:



```text

Trend

&#x20; ↑

&#x20; │

&#x20; └── Pullback

&#x20;      ↓

&#x20;      Continuation

```



A pullback strategy should identify:



\- trend condition

\- pullback condition

\- invalidation

\- re-entry trigger



Do not automatically treat every retracement as a valid pullback.



\---



\## 10. Momentum



Momentum describes persistent directional movement over a defined measurement period.



Possible proxies include:



\- rate of change

\- moving-average slope

\- price acceleration

\- range expansion

\- relative strength

\- volume expansion



Momentum should be distinguished from:



```text

Temporary Price Spike

```



A sudden price movement does not automatically imply persistent momentum.



\---



\## 11. Mean Reversion



Mean-reversion strategies rely on the hypothesis that price tends to move back toward a defined reference or equilibrium under certain conditions.



Possible references include:



\- moving average

\- VWAP

\- statistical mean

\- range midpoint

\- volatility-adjusted equilibrium



The AI must identify why the market should revert.



Do not assume that:



```text

"Price is far from EMA"

```



automatically means:



```text

"Price must return to EMA."

```



Strong trends can remain extended for long periods.



\---



\## 12. Support and Resistance



Support and resistance may be represented by:



\- prior swing points

\- consolidation boundaries

\- session highs/lows

\- previous day high/low

\- previous week high/low

\- significant reaction areas



The AI should avoid treating every historical turning point as equally important.



Relevance depends on:



\- timeframe

\- number of reactions

\- recency

\- volatility

\- context



\---



\## 13. Zones vs Exact Lines



Real market reactions do not necessarily occur at a mathematically exact price.



Where appropriate, support and resistance should be treated as zones rather than perfect lines.



The zone width should be determined consistently rather than adjusted visually after seeing historical outcomes.



\---



\## 14. Liquidity Concept



Liquidity refers to areas where significant trading activity or resting orders may exist.



Potentially relevant locations include:



\- recent swing highs

\- recent swing lows

\- equal highs

\- equal lows

\- previous session extremes

\- obvious range boundaries



These areas may attract price interaction.



However, the AI must not present unobservable order-book assumptions as confirmed facts when the required data is unavailable.



\---



\## 15. Liquidity Sweep



A liquidity-sweep hypothesis may describe:



```text

Obvious High / Low

&#x20;      ↓

Price Trades Beyond It

&#x20;      ↓

Rejects / Returns

&#x20;      ↓

Potential Reversal

```



The strategy must define what constitutes a sweep.



Possible measurable definitions:



\- minimum penetration distance

\- candle close back inside range

\- rejection wick

\- follow-through

\- time window



Do not rely only on visual interpretation.



\---



\## 16. Equal Highs / Equal Lows



Repeated highs or lows can form visually obvious structural levels.



Examples:



```text

Equal Highs

───────────

&#x20;  /\\   /\\

&#x20; /  \\\_/  \\

```



and:



```text

Equal Lows

───────────

\\    /\\    /

&#x20;\\\_\_/  \\\_\_/

```



These may be useful as structural references.



Do not automatically assume they represent actual resting liquidity without supporting data.



\---



\## 17. Market Regimes



Every strategy should consider which regime it is designed for.



Possible regimes:



```text

Strong Bull Trend

Weak Bull Trend

Strong Bear Trend

Weak Bear Trend

Range

Breakout

Post-Breakout

High Volatility

Low Volatility

Transition

```



A strategy should ideally identify its preferred regime.



\---



\## 18. Regime Detection



Possible regime features include:



\- moving-average slope

\- ADX

\- ATR percentile

\- trend strength

\- market breadth where available

\- volatility

\- range width

\- structural behavior



Regime detection should be kept as simple as possible.



Do not add multiple regime indicators merely because they improve a backtest.



\---



\## 19. Multi-Timeframe Analysis



A multi-timeframe system should define:



```text

Higher Timeframe

&#x20;   ↓

Context / Bias



Trading Timeframe

&#x20;   ↓

Setup



Execution Timeframe

&#x20;   ↓

Trigger

```



Example:



```text

H1 → Market Context

M15 → Setup

M5 → Entry

```



The exact combination depends on the strategy.



Do not automatically assume that more timeframes produce better signals.



\---



\## 20. Higher-Timeframe Bias



A higher timeframe may provide context such as:



\- trend direction

\- major swing structure

\- major support/resistance

\- broader volatility

\- range boundaries



The lower timeframe should not automatically be forced to match the higher timeframe.



Sometimes countertrend setups may be intentional.



The strategy must define the relationship explicitly.



\---



\## 21. Session Context



For intraday strategies, session context can matter.



Relevant references may include:



\- Asian range

\- London opening range

\- New York opening range

\- previous session high/low

\- previous day high/low

\- previous week high/low



Session-based logic must use a clearly defined timezone.



Do not assume PC local time equals broker/server time.



\---



\## 22. XAUUSD Market Context



For XAUUSD research, consider:



\- volatility changes

\- session behavior

\- USD-related macro sensitivity

\- major economic events

\- rapid spread changes

\- sudden directional expansion



Macro/news effects should not be fabricated.



When live news or external economic data is required, the data source must be identified.



\---



\## 23. Crypto Market Context



Crypto market structure has additional characteristics:



\- continuous trading

\- exchange fragmentation

\- derivatives influence

\- funding

\- liquidation cascades

\- varying liquidity across assets

\- strong correlations during market stress



A crypto strategy should explicitly identify whether it trades:



```text

Spot

```



or:



```text

Futures / Perpetuals

```



\---



\## 24. Structure vs Indicator



The AI should not equate:



```text

EMA Cross

```



with:



```text

Trend Confirmed

```



An EMA cross is an indicator event.



A trend is a market-structure hypothesis.



Indicators can be used to quantify structure, but the underlying hypothesis should remain explicit.



\---



\## 25. Indicator Selection



Use the smallest set of indicators necessary to test the hypothesis.



Example:



```text

Hypothesis:

Trade directional momentum.



Possible measurement:

Fast EMA vs Slow EMA

\+

Volatility Filter

```



Do not automatically add:



```text

RSI

MACD

Stochastic

CCI

ADX

Bollinger Bands

```



unless each adds independent information.



\---



\## 26. Confluence



Confluence means multiple pieces of information point toward the same hypothesis.



Potential categories:



```text

Structure

\+

Trend

\+

Volatility

\+

Location

\+

Trigger

```



Do not count five highly correlated indicators as five independent confirmations.



Example:



```text

EMA 8 > EMA 21

EMA 9 > EMA 22

EMA 10 > EMA 20

```



are largely related measurements, not three independent sources of evidence.



\---



\## 27. Entry Location



A signal should be evaluated together with location.



The AI should ask:



```text

Where is price entering?

```



rather than only:



```text

What is the signal?

```



Possible locations:



\- support

\- resistance

\- range boundary

\- breakout point

\- pullback area

\- volatility expansion area



\---



\## 28. Trigger



A setup and a trigger are different.



\### Setup



The market reaches a condition where a trade may be considered.



\### Trigger



A specific event confirms entry.



Example:



```text

Setup:

Bullish higher-timeframe structure

\+

Pullback to support



Trigger:

Confirmed bullish candle close

```



The strategy must define both where applicable.



\---



\## 29. Invalidation



Every setup should define when the hypothesis is no longer valid.



Examples:



```text

Structural Low Broken

```



```text

Range Re-enters After Failed Breakout

```



```text

Volatility Condition Invalid

```



```text

Time Window Expired

```



The invalidation condition should be measurable.



\---



\## 30. Trade Location vs Trade Direction



Correct directional bias does not automatically create a good trade.



For example:



```text

Bullish Market

```



does not mean:



```text

Buy At Any Price

```



Entry location and risk placement remain important.



\---



\## 31. No-Trade Conditions



A professional strategy should define when NOT to trade.



Examples:



```text

Spread Too Wide

Volatility Too Extreme

Volatility Too Low

Outside Session

Daily Loss Limit Reached

Maximum Positions Reached

Conflicting Structure

Invalid Setup

Upcoming High-Impact Event

Execution Conditions Unacceptable

```



The ability to stay out is part of the strategy.



\---



\## 32. Conflicting Signals



When different timeframes or conditions disagree, the AI must not automatically choose the direction that produced the best historical result.



Instead, define a deterministic priority.



Example:



```text

Higher-Timeframe Context

&#x20;       >

Trading-Timeframe Setup

&#x20;       >

Execution Trigger

```



or explicitly define when disagreement means:



```text

No Trade

```



\---



\## 33. Structure Break vs Noise



Not every price excursion is a structural break.



A strategy must define the minimum condition for a break.



Possible definitions:



\- candle close beyond level

\- minimum price distance

\- minimum duration

\- follow-through

\- volatility-adjusted threshold



The rule should be reproducible.



\---



\## 34. Fractal / Swing Detection



When identifying swings, define:



\- left bars

\- right bars

\- minimum distance

\- minimum significance

\- confirmation delay



Be aware that swing detection can inherently require future bars to confirm a historical swing.



The AI must prevent such confirmation logic from creating look-ahead bias in a backtest.



\---



\## 35. Market Structure and Look-Ahead Bias



This is especially important when coding:



\- swing highs

\- swing lows

\- fractals

\- pivot points

\- support/resistance

\- liquidity levels



If a level is only confirmed after future candles occur, the strategy must not pretend the level was known earlier.



Historical visualization and tradable real-time knowledge are not always identical.



\---



\## 36. Structural Stop Placement



Stop placement may be based on:



```text

Swing High / Low

Structure Invalidation

Range Boundary

ATR

Volatility

```



The stop should represent a meaningful invalidation point where possible.



Do not move structural stops solely because optimization improves.



\---



\## 37. Structural Take Profit



Targets may be based on:



\- next swing

\- range boundary

\- previous high/low

\- measured move

\- volatility target

\- fixed reward/risk



The target should be consistent with the strategy hypothesis.



\---



\## 38. Market Context Report



When reviewing a chart or strategy, the AI should be able to summarize:



```text

Market:

Timeframe:



Higher-Timeframe Structure:



Current Regime:



Key Levels:



Volatility:



Potential Setup:



Trigger:



Invalidation:



Risk:



No-Trade Conditions:

```



The report should distinguish observation from interpretation.



\---



\## 39. Strategy Classification



Every strategy should identify its primary structural hypothesis.



Examples:



```text

Trend Following

Breakout

Breakout Retest

Pullback Continuation

Mean Reversion

Momentum

Liquidity Sweep Reversal

Range Trading

Volatility Expansion

```



A hybrid strategy may have multiple components, but the primary hypothesis should remain clear.



\---



\## 40. Avoiding Narrative Bias



The AI must not create a convincing story around every historical move.



A chart can always be explained after the fact.



Research must instead ask:



```text

Could this rule have been known before the trade?

```



and:



```text

Would the rule have produced the same decision in real time?

```



\---



\## 41. Discretion vs Automation



If a concept is too discretionary to define consistently, classify it as:



```text

Manual Concept

```



before trying to automate it.



Examples of problematic rules:



```text

"Strong rejection"

"Clean breakout"

"Good support"

"Market feels overextended"

```



These should be converted into measurable definitions before implementation.



\---



\## 42. Structure-Based Strategy Development



Recommended workflow:



```text

Observe Behavior

&#x20;     ↓

Form Hypothesis

&#x20;     ↓

Define Structure

&#x20;     ↓

Define Setup

&#x20;     ↓

Define Trigger

&#x20;     ↓

Define Invalidation

&#x20;     ↓

Define Risk

&#x20;     ↓

Backtest

```



Do not start by optimizing parameters.



\---



\## 43. AI Rules



Before proposing indicators, the AI should ask:



1\. What market behavior are we exploiting?

2\. What market regime does it require?

3\. Where does the setup occur?

4\. What confirms the setup?

5\. When is the hypothesis invalid?

6\. What should cause a no-trade decision?

7\. Can the rule be expressed mathematically?

8\. Can the rule be tested without future information?

9\. Can the rule be executed consistently?



If the answer is unclear, the concept is not ready for automation.



\---



\## 44. Final Principle



The objective of market-structure analysis is not to predict every move.



It is to define repeatable conditions under which a trading hypothesis may have an advantage.



Therefore:



```text

Context

\+

Structure

\+

Regime

\+

Location

\+

Trigger

\+

Invalidation

\+

Risk

=

Testable Trading Hypothesis

```



The AI must prefer testable definitions over attractive narratives.



Never confuse a convincing chart explanation with statistical evidence.

