\# Strategy Specification



\## Metadata



Strategy Name:



Strategy Version:



Experiment ID:



Date:



Author:



Status:



\---



\## 1. Market



Market:



Symbol:



Market Type:



Exchange / Broker:



Account Currency:



\---



\## 2. Timeframe



Primary Timeframe:



Higher Timeframe:



Lower / Execution Timeframe:



\---



\## 3. Hypothesis



\### Market Behavior



What market behavior is being exploited?



\### Hypothesis



State the hypothesis in a falsifiable form.



\### Why Should This Edge Exist?



Explain the market mechanism or behavioral reason.



\### Expected Market Regime



Which conditions should favor this strategy?



\---



\## 4. Setup



Define the conditions required before a trade can be considered.



\### Context



\### Location



\### Market Structure



\### Volatility



\### Trend / Bias



\---



\## 5. Entry Trigger



Define the exact event that activates the trade.



Entry Direction:



BUY / SELL / BOTH



Trigger:



Entry Timing:



Entry Price Model:



Order Type:



\---



\## 6. Stop Loss



Method:



Distance:



Structural Reference:



Maximum SL:



Why is this stop logically meaningful?



\---



\## 7. Take Profit



Method:



Target:



Risk/Reward:



Partial Exit:



Trailing:



Break-Even:



\---



\## 8. Position Sizing



Risk Per Trade:



Position Size Method:



Minimum Position:



Maximum Position:



Maximum Total Exposure:



\---



\## 9. Filters



\### Spread Filter



\### Volatility Filter



\### Session Filter



\### Trend Filter



\### News Filter



\### Other Filters



For every filter explain:



```text

Purpose:

Expected Benefit:

Potential Drawback:

```



\---



\## 10. No-Trade Conditions



Do not trade when:



1\.



2\.



3\.



4\.



\---



\## 11. Invalidation



The setup becomes invalid when:



1\.



2\.



3\.



\---



\## 12. Position Management



Maximum Positions:



Layering:



Cooldown:



Opposite Position Policy:



Position Modification Rules:



\---



\## 13. Risk Protection



Daily Loss Limit:



Maximum Drawdown:



Maximum Consecutive Losses:



Equity Protection:



Emergency Stop:



\---



\## 14. Execution Requirements



Broker / Exchange:



Execution Type:



Filling Type:



Maximum Spread:



Maximum Slippage:



Minimum Volume:



Maximum Volume:



Volume Step:



Price Precision:



Other Restrictions:



\---



\## 15. Backtest Plan



Data Source:



Testing Period:



Initial Capital:



Commission:



Spread Assumption:



Slippage Assumption:



Execution Model:



In-Sample Period:



Out-of-Sample Period:



\---



\## 16. Metrics



Track at minimum:



\- Net Profit

\- Profit Factor

\- Win Rate

\- Maximum Drawdown

\- Expectancy

\- Total Trades

\- Average Trade

\- Average Win

\- Average Loss

\- Maximum Consecutive Losses



\---



\## 17. Robustness Plan



Test:



\- parameter sensitivity

\- spread sensitivity

\- slippage sensitivity

\- transaction costs

\- market regimes

\- different periods

\- out-of-sample data

\- trade-order variation



\---



\## 18. Known Weaknesses



Document known risks and weaknesses before testing.



\---



\## 19. Validation Criteria



Define the conditions required before the strategy can progress to the next stage.



\---



\## 20. Final Classification



Choose one:



```text

Experimental

Backtest Positive

Robust Candidate

Forward Testing

Validated

Rejected

```



\---



\## 21. Conclusion



Summary:



Next Action:

