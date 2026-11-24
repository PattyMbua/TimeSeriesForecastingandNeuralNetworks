# Member C: Notebook 3, Classical Forecasting Baselines (JPM)

## Setup and validation
- Stock: JPM (JPMorgan Chase), 2981 trading days, 2 January 2015 to 9 November 2026, with 7 predictor series.
- Walk-forward validation: 29 folds. The first training block is 1000 days and each test block is 63 days. The training window expands forward and is never shuffled.
- Scoring period: 8 October 2019 to 6 November 2026.
- ARIMA order chosen by AIC on the first training block only: ARIMA(0,1,0).
- ARMAX uses yesterday's values of the seven predictors (`.shift(1)`), so no future information is used.

## Main results (one day ahead, against the naive forecast)

| Model | RMSE ($) | MAE ($) | Directional accuracy | Out-of-sample R2 | DM p-value | Verdict |
|---|---|---|---|---|---|---|
| Naive (no change) | 2.9620 | 2.0043 | n/a | 0.0000 | n/a | baseline |
| Drift | 2.9595 | 1.9969 | 53.6% | 0.0009 | 0.6152 | No significant difference |
| Moving average (20 days) | 7.4443 | 5.6021 | 49.7% | -5.3221 | 0.0000 | Significantly worse |
| Simple exponential smoothing | 2.9673 | 2.0094 | 49.0% | -0.0061 | 0.3188 | No significant difference |
| ARIMA(0,1,0) | 2.9620 | 2.0043 | n/a | 0.0000 | 0.8567 | No significant difference |
| ARMA(1,0), own past only | 2.9670 | 2.0094 | 51.4% | -0.0087 | 0.2917 | No significant difference |
| ARMAX(1,0), 7 predictors | 2.9734 | 2.0121 | 53.1% | -0.0070 | 0.5899 | No significant difference |

## Other observations
- Step 7: the ARIMA forecast looks almost perfect on a chart, but zoomed in it is just a copy of the previous day's price moved one day forward. ARIMA(0,1,0) is exactly the naive forecast.
- Step 8: the Ljung-Box p-values are far below 0.05 at lags 5, 10 and 21, so some pattern remains in the residuals. This is mainly volatility clustering, which ARIMA cannot capture because it assumes the error variance stays constant.
- Step 9: the 63-day forecast is a flat line at 342.08, the last close, with a 95% interval from 262.98 to 444.96.

## Exercises

**1. Lowest RMSE and the DM test.**
The drift forecast has the lowest RMSE (2.9595 against 2.9620 for naive), but its Diebold-Mariano p-value is 0.6152, so the difference is not significant.
For a manager: none of our classical models predicts JPM's next closing price better than simply assuming it will be the same as today.

**2. SES alpha close to 1.**
SES estimated alpha at 0.991 and 0.994 in the first folds. An alpha close to 1 means the model puts almost all its weight on the most recent price and ignores older prices. That makes SES almost identical to the naive forecast, which is what we expect when a price behaves like a random walk.

**3. ARMA(1,0) against ARMAX(1,0).**
The extra predictors did not help in any meaningful way. Out-of-sample R2 was -0.0087 for ARMA and -0.0070 for ARMAX. Both are below zero, which means both are worse than the naive forecast, and the gap between them is tiny. ARMAX's DM p-value of 0.5899 confirms it is not better than naive. One possible reason is that markets absorb public information quickly, so yesterday's moves in the S&P 500, oil, gold and bond yields are already reflected in JPM's price and carry almost nothing about tomorrow's return.

**4. Removing `.shift(1)`.**
Without the shift, the out-of-sample R2 jumped from -0.0070 to 0.6104 and directional accuracy rose from 53.1% to 75.4%. RMSE fell from 2.97 to 2.22. The result looks excellent but is worthless, because the model now uses the other markets' moves from the same day it is predicting. In practice those values are not known until that day closes, so the model is using future information. This is look-ahead bias. The shift was restored before the final run.

**5. SARIMA with a weekly seasonal term, seasonal_order=(1, 0, 0, 5).**
SARIMA gave an out-of-sample R2 of -0.0095, slightly worse than ARMA(1,0) at -0.0087, with directional accuracy of 51.8%. The weekly pattern did not help. This agrees with Notebook 2, Step 8, which found no useful weekly seasonality in daily stock returns.

**6. Moving average window of 5, 20 and 60 days.**

| Window | RMSE ($) | Out-of-sample R2 |
|---|---|---|
| 5 days | 4.17 | -0.97 |
| 20 days | 7.44 | -5.32 |
| 60 days | 12.30 | -16.63 |

A moving average lags the price by about half its window: about 2 days for a window of 5, 10 days for 20 and 30 days for 60. A longer window averages older prices, so it reacts more slowly and falls further behind any trend. That is why the error grows as the window gets longer. All three are worse than the naive forecast. The window was set back to 20 for the final run.

**7. Growth of the interval with the horizon.**
The 95% interval is about 22.67 wide on day 1 and 181.98 wide on day 63, a factor of about 8.0. This is close to the square root of 63 (about 7.9), because uncertainty in a random walk grows with the square root of the number of days. By day 63 the range runs from 262.98 to 444.96, so the 63-day forecast carries very little precise information.

## Verdict 
Our best classical model is the drift forecast (RMSE 2.9595 against 2.9620 for naive). Its DM p-value against the naive forecast is 0.6152, so it does not beat the naive forecast. ARIMA chose (0,1,0), which is exactly the naive forecast. ARMAX and SES were not significantly different from naive, and the 20-day moving average was significantly worse.