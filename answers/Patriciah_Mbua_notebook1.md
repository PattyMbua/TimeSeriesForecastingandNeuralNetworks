# Notebook 1 answers: Patriciah Mbua (Member A)

Stock: JPM (JPMorgan Chase). Data: 2,981 trading days, 2015-01-02 to 2026-11-09.

## Exercise 1: Which line of the data quality report would worry me most if it were not zero, and why?

`non_positive_prices`. Later in the lab we take the log of prices to get log returns, and you cannot take the log of zero or a negative number. One bad price would break the return series, and every model after that depends on it. For JPM it is 0, and so are the duplicate dates, missing values, zero volume days and `high_below_low`, so the data looks clean.

## Exercise 2: What is missing from "The S&P 500 correlates 0.7 with our stock, so it will be an excellent predictor"?

The timing. The stock and the S&P 500 close at the same moment, so today's S&P 500 move is only known when the day is over, and it cannot be used to forecast today's stock move. What we can actually use is the market's move up to and including today to forecast tomorrow's stock return. A same-day correlation of 0.7 says the two move together, but it says nothing about whether today's market predicts tomorrow's stock, and that relationship is usually much weaker.

## Exercise 3: What does volume do when the price falls sharply (Feb to Apr 2020), and why?

Volume goes up when the price falls sharply. In my chart JPM drops from about 112 in early February to a low of about 68, and over that fall the daily volume rises from roughly 10 to 15 million shares to peaks above 40 million, then it eases off in April as the price steadies. This is probably because many investors react to the same bad news at once, so they all try to sell at the same time and far more shares change hands. A big price fall on very low volume would be less convincing, because only a few traders would have caused it.

## Exercise 4: A stock rises 50% in year one and falls 50% in year two.

- Total simple return: 1.5 x 0.5 - 1 = **-0.25, which is -25%**.
- Total log return: ln(1.5) + ln(0.5) = 0.4055 - 0.6931 = **-0.2877, about -28.8%** (the same as ln(0.75)).

The stock ends 25% lower even though the two moves are both "50%". Simple returns cannot just be added across periods, but log returns can.

## Exercise 5: A risk management decision that would be wrong if returns followed the red curve

The red curve is the normal curve with the same mean and spread as the returns. If I assumed it, I would set a risk limit, such as a stop-loss or a value-at-risk limit or the amount of capital to hold in reserve, as if extreme days almost never happen. JPM's daily return has a standard deviation of 0.0169, but its biggest one-day loss is -0.1621 and its biggest gain is +0.1656, which are about 9.6 and 9.8 standard deviations away from the mean. A normal curve says those days should essentially never happen, yet there are 7 days with moves over 10%. So a limit built on the red curve would underestimate the risk of a very bad day.

## Why we chose JPM

JPM is a large bank, so it suits a client at a financial institution. It has complete Yahoo Finance data from January 2015, 2,981 trading days with no missing values, no duplicate dates and no bad prices, which is plenty for the walk-forward validation in the later notebooks. It is a heavily traded stock, so the prices are reliable. It also avoids NVDA, which the lab caps at 60, and GOOGL, which the notebooks already use. The period covers real stress too, such as the Feb to Apr 2020 crash, which makes it a fair test of how well a model copes with uncertainty.