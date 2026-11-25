
# Member E: Notebook 4 Answers

## Exercise 6: Absolute Error and Fat Tails

Absolute error is more robust for returns with fat tails because it penalises prediction errors linearly rather than squaring them. This reduces the influence of extreme stock returns on model training and prevents large errors from dominating the learning process.

After changing the loss in make_boosting from squared_error to absolute_error, I obtained the following results:

- Tuning time: 49 seconds
- Best maximum depth: 5
- Best learning rate: 0.01
- Tuning MSE: approximately 0.000171

Absolute error may improve robustness, but these tuning results alone do not prove better out-of-sample forecasting performance.

## Exercise 7: Trading at the Closing Price

This assumption is optimistic because the final closing price is only known once the market closes. By the time the model generates a prediction, the trader may no longer be able to trade at that price and may have to wait until the next trading session. Price changes, slippage and additional trading costs could reduce actual profits, making the backtest results more favourable than what would be achieved in real trading.
