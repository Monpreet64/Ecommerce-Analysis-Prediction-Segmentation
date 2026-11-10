# E-Commerce Sales Analysis, Prediction, and Customer Segmentation

An end-to-end Python data-science project using the UCI Online Retail
transactions. The notebook combines transaction cleaning, exploratory sales
analysis, a chronological daily-sales prediction experiment, and RFM-based
customer segmentation with K-Means.

## Project goals

- Prepare transaction data for analysis and document the cleaning decisions.
- Explore sales over time and across countries.
- Predict daily sales on a historical holdout period and compare the model
  with a 7-day seasonal-naive baseline.
- Build descriptive customer segments from recency, frequency, and monetary
  value (RFM) features.
- Export report-ready summaries, model diagnostics, and holdout predictions.

## Repository contents

| Path | Description |
|---|---|
| `Ecommerce_Analysis_Prediction_Segmentation.ipynb` | Complete analysis notebook, including code, visualizations, and executed outputs |
| `Online Retail.xlsx` | Source transaction workbook from the UCI Online Retail dataset |
| `outputs/customer_rfm_segments.csv` | Per-customer RFM features, cluster, and descriptive segment |
| `outputs/customer_segment_summary.csv` | Aggregate customer-segment summary |
| `outputs/kmeans_cluster_selection_metrics.csv` | K-Means inertia and silhouette diagnostics for K=2 through K=8 |
| `outputs/daily_sales_test_predictions.csv` | Actual, Random Forest-predicted, and lag-7 benchmark sales for the test period |
| `PROJECT_DOCUMENTATION.docx` | Detailed goals, architecture, methodology, results, limitations, and reproduction guide |
| `Ecommerce_Project_Report.docx` / `.pdf` | Project report |
| `Ecommerce_Project_Presentation_Complete.pptx` | Project presentation |
| `6.44468.pdf` | Supporting reference article |

## Quick start

The notebook expects `Online Retail.xlsx` in the project root (as included
here). From the repository directory:

```bash
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
python -m pip install -r requirements.txt
jupyter notebook Ecommerce_Analysis_Prediction_Segmentation.ipynb
```

Run the notebook cells from top to bottom. It reads the workbook, performs the
analysis, and writes CSV results under `outputs/`. Existing notebook outputs
are included to make the reported results easy to inspect without first
rerunning every cell.

## Method and results at a glance

After removing rows without a customer ID, cancelled invoices, and transactions
with non-positive quantity or unit price, the analysis retains **397,884
transaction lines**, **18,532 invoices**, and **4,338 customers**. The cleaned
transaction sales total is **£8,911,407.90**.

For the sales experiment, daily sales are modeled using lagged sales, trailing
rolling means, and calendar features. A chronological 288-day training / 72-day
test split is used; the holdout runs from **2011-09-29 to 2011-12-09**.
The Random Forest achieves **£11,260.04 MAE**, **£21,908.12 RMSE**, and
**0.3138 R²**. The lag-7 benchmark achieves **£13,155.37 MAE** and
**£22,451.47 RMSE**. This is a one-step-ahead historical evaluation with
previous actual sales available, not a recursive long-range forecast.

For segmentation, log-transformed and standardized RFM features are clustered.
K=3 is retained for business interpretability, although K=2 has the highest
silhouette score among the tested values. The three descriptive segments are:

| Segment | Customers | Share | Median recency | Median invoices | Median spend |
|---|---:|---:|---:|---:|---:|
| Toppers / High-value | 757 | 17.45% | 10 days | 10 | £3,722.82 |
| Moderated / Mid-value | 1,695 | 39.07% | 30 days | 3 | £982.38 |
| Churn-risk / Low-engagement | 1,886 | 43.48% | 158 days | 1 | £299.81 |

The labels describe observed purchasing behavior; they are not validated
predictions of future churn. Full metric details and caveats are in the
[project documentation](PROJECT_DOCUMENTATION.docx) and [project report](Ecommerce_Project_Report.pdf).

## Data and privacy note

The workbook is the publicly available UCI Online Retail dataset. The
customer-level CSV contains pseudonymous `CustomerID` values and derived
purchase-behavior features. Do not attempt to identify individuals from these
records; handle derived customer profiles as potentially sensitive analytical
data.

## Sources

- Chen, D. (2015). [Online Retail dataset](https://doi.org/10.24432/C5BW33).
  UCI Machine Learning Repository.
- Mehta, D., & Sehgal, S. (2024). [A study on micro-segmentation of retail
  customers using K-means clustering](https://doi.org/10.18311/jbt/2024/44468).
- Rousseeuw, P. J. (1987). [Silhouettes: A graphical aid to the interpretation
  and validation of cluster analysis](https://doi.org/10.1016/0377-0427(87)90125-7).

No separate license is provided for the project materials. The dataset remains
subject to its source terms.
