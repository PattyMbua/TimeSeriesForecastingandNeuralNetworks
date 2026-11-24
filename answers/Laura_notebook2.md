# Laura — Notebook 2: Stationarity and Returns

## JPM stationarity results

| Series | ADF p-value | KPSS p-value | Conclusion |
|---|---:|---:|---|
| Closing price | 0.9976 | ≤ 0.01 | Non-stationary |
| Daily log return | < 0.0001 | ≥ 0.10 | Stationary |

Both tests suggest that JPM’s closing price did not stay around a stable average during the period studied. Its daily log returns had a more stable average, which supports the group’s decision to forecast returns and then convert them back into prices.

## Exercise 1 — What do the ADF results mean?

For the closing price, the ADF p-value was 0.9976. This gives us little evidence that the price returns to a fixed average. For the daily log return, the p-value was below 0.0001. This supports using daily returns as a more stable starting point for forecasting, although tomorrow’s return can still be difficult to predict.

## Exercise 2 — What did taking the log achieve?

Taking the log put price changes on a relative scale. For example, a 10% increase produces the same change in log price whether the share starts at $50 or $200. The log price itself still had an ADF p-value of 0.9067 and remained non-stationary. Taking the difference between consecutive log prices gave us the stationary daily log return.

## Exercise 3 — What happened when the number of days changed?

| Days per simulation | Pairs with a significant result using levels | Pairs with a significant result using daily changes |
|---|---:|---:|
| 100 | 79.4% | 5.8% |
| 5,000 | 95.8% | 6.2% |

The pairs were generated independently, so any apparent relationship was a false alarm. Using raw levels, the share of false alarms rose from about 79% to 96% as the sample grew. Using daily changes, it stayed close to 6%. Adding more days made the misleading relationship in the levels look even stronger.

## Exercise 4 — Comparing JPM with another predictor

I used the S&P 500 as the second predictor and compared it with the notebook’s gold example.

| Predictor | R² using log prices | R² using daily log returns |
|---|---:|---:|
| Gold | 0.860 | 0.0033 |
| S&P 500 | 0.963 | 0.5092 |

The S&P 500 showed an even stronger relationship with JPM in price levels than gold did. The biggest difference appeared in daily returns. The S&P 500 explained about 51% of the variation in JPM’s same-day returns, while gold explained less than 1%. This shows that JPM often moved with the wider market on the same day. Predicting the next day’s return is a separate question.

## Exercise 5 — Which plot best describes JPM’s daily log returns?

I would choose the plot that fluctuates around a fairly steady centre, with occasional bursts of larger movements. In Step 3, JPM’s 63-day rolling average return stayed close to zero for most of the period. The rolling standard deviation rose sharply in 2020, showing that daily movements became much larger for a while even though the average return remained close to zero.

## Exercise 6 — Comparing the two GARCH models

| Estimate | Student’s t model | Normal model |
|---|---:|---:|
| Average daily return, mu (%) | 0.109 | 0.105 |
| Baseline variance term, omega | 0.144 | 0.203 |
| Reaction to recent shocks, alpha | 0.131 | 0.124 |
| Effect of past variance, beta | 0.818 | 0.793 |
| Log-likelihood | −5,206.87 | −5,363.67 |

With the normal distribution, the baseline variance term increased from 0.144 to 0.203. The reaction to recent shocks and the effect of past variance both fell slightly. The Student’s t model fit JPM’s returns better because its log-likelihood was higher: −5,206.87 compared with −5,363.67.