# Olist Brazilian E-Commerce Analysis

End-to-end analysis of the public [Olist Brazilian E-Commerce dataset](https://www.kaggle.com/code/hatemomara/brazilian-e-commerce): about 100,000 marketplace orders placed between 2016 and 2018. The project covers data validation, a clean order-level master table, exploratory analysis, cohort retention, RFM segmentation, and a statistical test, and ends with business recommendations.

**Tools:** Python, pandas, NumPy, SciPy, Matplotlib

## Business questions

1. How reliable is delivery, and which cities and months are hit hardest by late orders?
2. How loyal are customers, and where do repeat purchases concentrate?
3. Where does revenue come from by city, payment method, and product category?
4. Which sellers and categories drive sales volume and value?
5. What drives customer satisfaction, and how much does late delivery hurt review scores?
6. How well do customers come back after a first purchase, and which groups should retention target?

## Key findings

| Area | Finding |
|---|---|
| Delivery | 91.9% of 96,470 delivered orders arrived on or before the estimated date. Average delivery time is 12.6 days. |
| Late spikes | The late rate reached 14.3% in November 2017, then 16.0% in February 2018 and 21.4% in March 2018, against roughly 1% to 8% in most other months. The 2018 spikes do not coincide with a volume jump. |
| Reviews | Late orders average a review score of 2.57, against 4.29 for on-time orders. Orders delivered in 20 or more days average 3.23 stars, against 4.45 within 5 days. |
| Statistical test | Mann–Whitney U on 88,163 on-time and 7,661 late orders: p < 0.001, rank-biserial effect size about 0.55 (large), median score 5 vs 2. |
| Price | Review scores are flat across price ranges (4.12 under R$ 50, 4.00 above R$ 1,000). |
| Geography | Six of the ten cities with the highest late rate are also among the ten with the highest negative-review rate. Maceio is the worst case: 29.7% late deliveries and 27.8% negative reviews. |
| Estimates | Delivery estimates are conservative: all 135 ranked cities average 5 to 18 days earlier than estimated. Manaus is the slowest city (26.5 days) but only 3.6% of its orders are late. |
| Retention | Only 3.1% of customers (2,997 of 96,096) ordered more than once. In the cohorts from September 2017 to July 2018, only 0.2% to 0.7% ordered again in the following month. |
| Revenue | R$ 16.0M recorded payment value across all order statuses (R$ 15.4M for delivered orders). Sao Paulo accounts for 13.8% of it. Credit cards are 78% of payment value. |
| Sellers | 60% of sellers (1,849 of 3,095) are based in Sao Paulo state. |

**Takeaway.** Delivery quality is the strongest lever on satisfaction found in this analysis, and retention is the clearest commercial gap. These are associations in observational data, not proof of causation.

## Approach

- **Data audit first.** All nine source tables were checked for duplicates, missing values, key uniqueness, and referential integrity before any transformation.
- **One row per order.** The master table aggregates payments, items, and reviews before every join, with assertions that the row count does not change.
- **Minimum sample sizes.** Cities and months are ranked only above a minimum sample (configurable at the top of the notebook), so small groups do not distort rankings.
- **Customer identity.** Customer-level metrics use `customer_unique_id`, because `customer_id` is generated per order.
- **Cohort retention.** Cohorts are defined by the month of a customer's first delivered purchase.
- **RFM with a data-aware design.** About 97% of customers ordered once, so frequency cannot be scored in quintiles. Segments combine fixed frequency buckets (1, 2, 3+) with recency and monetary quintiles, and the code asserts that segment names match real order counts.
- **Effect size, not only p-values.** With about 96,000 orders almost any difference is significant, so the test also reports the rank-biserial correlation.
- **Consistent money definitions.** All-status payment value and delivered-order payment value are reported separately, and neither is called revenue or profit in the accounting sense.

## Notebook structure

1. Setup, 2. Data Loading, 3. Data Quality Audit, 4. Orders Preparation, 5. Master Dataset
6. Exploratory Analysis: order status, delivery, customers, revenue and payments, products, sellers, reviews
7. KPI Summary and Business Insights, 8. Export
9. Advanced Customer Analytics: cohort retention, RFM segmentation, statistical test, metric definitions
10. Limitations, 11. Recommendations, 12. Advanced Exports
- Appendix A. Geolocation Cleaning

## Recommendations

- Plan capacity ahead of peak periods, and review carriers in the worst-performing regions (Rio de Janeiro state and Maceio first).
- Build retention journeys using cohort retention and RFM segments: onboarding, repeat-purchase, and win-back campaigns.
- Monitor review scores alongside delivery status, and test other drivers such as category and seller.
- Report all-status and delivered-order payment value separately.

## Limitations

- Observational data: relationships are associations, not causal effects.
- Recent cohorts have a shorter follow-up window, so compare cohorts over equal windows.
- The data is a 2016–2018 snapshot of one marketplace and does not describe current performance.
- Payment value is not profit: refunds, commissions, taxes, and costs are not in the data.
- RFM recency and monetary scores are relative to this dataset and should be recalibrated before operational use.

## How to run

**On Kaggle:** add the Olist dataset to a notebook and run all cells. The data directory is detected automatically.

**Locally:**

```bash
pip install pandas numpy matplotlib scipy jupyter
# download the dataset from Kaggle and extract the CSV files into this folder
jupyter notebook brazilian-e-commerce.ipynb
```

The notebook detects the dataset directory automatically (Kaggle input folders first, then the current directory). It writes results to `/kaggle/working` when running on Kaggle, or `output/` locally, and bundles the exported summary tables into `olist_analysis_datasets.zip`. The raw data is not included in this repository; download it from Kaggle (see the dataset page for its license).

## Author

**Hatem Omara**
