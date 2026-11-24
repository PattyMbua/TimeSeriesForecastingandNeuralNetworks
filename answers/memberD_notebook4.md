# Member D: Notebook 4 answers

## 1. Manager verdict

Ridge had the lowest test RMSE ($2.9595 versus $2.9620 for the naive forecast) and a slightly positive out-of-sample R-squared (0.0007), but its Diebold-Mariano p-value of 0.7476 gives no evidence that it truly beats the naive forecast.

## 2. Truncation test with `center=False`

With `center=False`, the five-day rolling mean uses only the current close and the four preceding closes. Removing later rows therefore does not change the feature value at the cutoff, so the truncation check passes. I ran `check_no_lookahead` on this version and it passed. The notebook's demonstration was restored to `center=True`; its output shows that the check catches the centered feature because its window includes future closes.

## 3. New feature and leakage check

I added `volume_surge_10_60`, the 10-day average volume divided by the 60-day average volume, to the volume feature group in `tslab/features.py`. The feature uses trailing windows only. The notebook's Step 3 check passed with this feature included, before it was used for model evaluation.

## 4. Ridge versus Gradient Boosting ablation

The models do not give a reliable, consistent ranking of feature groups. Ridge's full-feature out-of-sample R-squared was 0.00070; removing returns reduced it to 0.00052, while removing other markets increased it to 0.00094. Gradient Boosting's full-feature score was -0.02467; removing volume reduced it to -0.02615, while removing volatility improved it to -0.02248. The differences are small, the Gradient Boosting ablation used 126-day test blocks while Ridge used the configured 63-day blocks, and neither full-feature model shows meaningful predictive skill. Treat the rankings as exploratory rather than firm findings.

## 5. Tuning on the first 1,000 days

The first 1,000 days may represent a different market regime from later years. For example, parameters selected before the 2020 COVID shock may not suit the sharp volatility and changing correlations during and after the shock. This keeps tuning safely before the final test period, but the chosen settings can become stale when market behavior changes.

## Note for Member E

The 10th-to-90th percentile interval covered 76.6% of the scored days, below its nominal 80% target.
