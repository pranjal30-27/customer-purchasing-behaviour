# Probabilistic Modelling of Customer Purchasing Behaviour

An end-to-end statistical analysis of the Online Retail II dataset (December 2009 to December 2011). The project examines how customers differ in purchase frequency, return timing, and spending, and uses these differences to segment customers and forecast revenue.

## Key Results

- Purchase counts are strongly overdispersed (variance-to-mean ratio of 26.01), so a Poisson model is rejected (dispersion ratio 1.98, χ² = 9,691.83, p < 0.001).
- Customer return timing is not well described by a constant-probability Geometric model. Early-period return hazard is roughly 0.0073, falling to about 0.0025 after 91 days.
- Lognormal fits invoice values better than Gamma (AIC 518,591.05 vs 524,482.28).
- Gaussian mixture modelling identifies 9 customer segments, with 95.27% average assignment confidence.
- Segment-based CLV reduces 3-month prediction MAE by 28.4% (£466.47 vs £651.42) compared with a one-group baseline.
- The 12-month Monte Carlo revenue forecast has a mean of £8.68M and a 95% simulation range of £8.46M to £8.90M.

**Limitations:** The segment-based model underestimates 3-month revenue by about 35.2%. The Monte Carlo interval is conditional on model assumptions and is not a guaranteed prediction interval.

## Dataset

Online Retail II, a public transactional dataset from a UK-based online retailer covering December 2009 to December 2011. The raw file has 1,067,371 line items.

The data is not included in this repository. Download it from the UCI Machine Learning Repository and place it in the `data/` folder before running the notebooks.

## Data Cleaning

Cleaning reduced the raw data to 802,555 valid merchandise line items, covering 36,594 invoices and 5,852 customers. The main steps were:

1. Removing cancellations, invalid or non-positive quantities and prices, and missing customer IDs
2. Removing service, shipping, and administrative entries
3. Removing manually identified test stock codes
4. Removing bad-debt, bank, and related charges
5. Excluding next-day carriage entries

Plausible large orders and possible legitimate duplicates were retained.

## Methods

- **Exploratory analysis:** Descriptive statistics, skewness, overdispersion, and time trends
- **Distribution fitting:** Poisson (monthly purchase counts), Geometric with Kaplan–Meier survival (return timing), and Gamma and Lognormal (invoice values), compared using AIC and BIC
- **Customer segmentation:** Log-transformed purchase count and average invoice value, clustered with a 9-component Gaussian mixture model
- **Customer lifetime value:** Segment-based 3-month revenue forecast, trained on January–August 2011 and tested on September–November 2011
- **Simulation:** 1,000 Monte Carlo runs of 12-month revenue, using Poisson counts and Lognormal invoice values

## Repository Structure

```
data/            Raw dataset (not included, see Dataset section)
notebooks/       Jupyter notebooks for EDA, fitting, segmentation, CLV, and simulation
dashboard/       Power BI file (.pbix) and dashboard screenshots
reports/         Presentation deck (PDF) and case study
```

## Tools

- Python and Jupyter Notebooks
- Power BI with DAX
- Statistical methods: maximum likelihood estimation, AIC/BIC, Kaplan–Meier survival analysis, bootstrap confidence intervals, Gaussian mixture models, Monte Carlo simulation

## How to Run

1. Clone the repository.
2. Download the Online Retail II dataset and place it in `data/`.
3. Run the notebooks in order, starting with the cleaning notebook.
4. Open `dashboard/` in Power BI Desktop to explore the interactive dashboard.

## Author

Pranjal
