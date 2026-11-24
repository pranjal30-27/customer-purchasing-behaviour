# Case Study: Probabilistic Modelling of Customer Purchasing Behaviour

## Summary

Customers in an online retail business differ widely in how often they buy, how long they take to return, and how much they spend. Using the Online Retail II dataset (800,000+ cleaned transactions, 5,852 customers), this project tested several standard probability models, segmented customers into 9 groups, and produced a revenue forecast with quantified uncertainty. The main finding is that single-model, average-based approaches miss important patterns in customer behaviour.

## Problem

Retail reports usually show totals and averages. These can hide a small group of high-value customers and a large group of infrequent buyers. The goal was to explain customer behaviour statistically, segment customers by their purchasing patterns, and forecast future value in a way that reflects uncertainty.

## Data and Cleaning

The raw dataset contained 1,067,371 transaction lines. After removing cancellations, invalid values, missing customer IDs, service entries, test stock codes, and accounting adjustments, 802,555 valid merchandise lines remained. These covered 36,594 invoices and 5,852 customers.

Different analyses required different levels of detail, so the data was organised into four datasets: product-level records for transaction detail, invoice-level data for spending models, customer-month counts for purchase frequency, and customer-level features for segmentation and forecasting.

Some decisions were deliberate. Large orders were kept because they are plausibly genuine bulk purchases. Customers who had not yet returned were kept in the return-timing analysis as right-censored observations, rather than being treated as customers who would never return.

## Exploratory Analysis

The data was strongly skewed. The average invoice value was £476.40, but the median was £304.64. Average customer spending was £2,979.03, against a median of £880.38. Purchase counts were also highly variable, with a variance-to-mean ratio of 26.01. This indicated that customers were not behaving alike.

Repeat purchases tended to happen relatively soon after the first one, with a median gap of about 25 days and a long tail of irregular returns.

## Modelling

**Purchase frequency.** A Poisson model with a single rate was tested on monthly purchase counts. It was rejected. The dispersion ratio was 1.98, and the chi-square test gave a value of 9,691.83 (p < 0.001). Customer activity varies too much for one common rate.

**Return timing.** A Geometric model assumes a constant daily probability of returning. Kaplan–Meier survival curves showed that customers return sooner than this model predicts in the early months, and that the return rate falls over time. A two-phase model fitted the data better (AIC 53,997.69 vs 54,360.41).

**Invoice values.** Gamma and Lognormal distributions were both fitted to the 36,594 invoices. Lognormal had the lower AIC (518,591.05 vs 524,482.28) and BIC. The Gamma model overestimated the share of invoices above £1,000 (12.18% predicted vs 7.35% observed). Neither model captured the extreme upper tail perfectly.

## Customer Segmentation

Purchase count and average invoice value were log-transformed and standardised, then clustered with a Gaussian mixture model. AIC and BIC both selected 9 components. Average assignment confidence was 95.27%, and 450 customers below 80% confidence were flagged for caution near cluster boundaries.

The segments differ sharply. One segment of 110 customers averaged 59.27 purchases and £56,366.77 in total spending, while the largest segment of 940 customers averaged 2 purchases and £685.65. The segment labels are statistical groupings, not fixed customer types.

## Forecasting and Validation

**Customer value.** A segment-based model was trained on January–August 2011 and tested on September–November 2011. Compared with a one-group baseline, it reduced mean absolute error by 28.4% (£466.47 vs £651.42) and RMSE by 12.7%. However, it still underestimated actual 3-month revenue (£1.55M predicted vs £2.40M observed), a shortfall of about 35.2%.

**Revenue simulation.** A 12-month Monte Carlo simulation with 1,000 runs combined Poisson purchase counts with Lognormal invoice values for each customer. The simulated portfolio revenue had a mean of £8.68M and a 95% range of £8.46M to £8.90M.

## Limitations

- The segment-based model underestimates total revenue. Its calibration needs improvement before it is used for budgeting.
- The Monte Carlo interval reflects the model's assumptions, not guaranteed future accuracy.
- The sample is UK-weighted. Sales in the UK are much larger than in other countries, so results may not generalise to other markets.
- The Lognormal model is a better relative fit than Gamma but still misses some of the extreme upper tail.

## Tools and Methods

Python and Jupyter notebooks, Power BI with DAX. Methods: maximum likelihood estimation, AIC and BIC model comparison, Kaplan–Meier survival analysis, bootstrap confidence intervals, Gaussian mixture models, log transformations, Monte Carlo simulation.

## Next Steps

- Test time-varying return models, since the constant-rate assumption does not hold
- Validate the forecast on out-of-sample data from a later period
- Improve calibration of the segment-based revenue model, especially for high-value segments
- Model seasonality more explicitly, since the observation window includes incomplete boundary months

## Links

- Repository: https://github.com/pranjal30-27/customer-purchasing-behaviour
- Dashboard: https://github.com/pranjal30-27/customer-purchasing-behaviour/main/retail_dashboard.pbix
- Presentation deck: https://github.com/pranjal30-27/customer-purchasing-behaviour/main/customer_purchasing_behaviour.pdf
