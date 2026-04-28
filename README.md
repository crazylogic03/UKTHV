# UKTHV — Olist E-Commerce Analytics

**Capstone 2 | Data Visualization & Analytics**
**Sector:** Retail / E-Commerce
**Dataset:** Brazilian E-Commerce Public Dataset by Olist (Kaggle)

---

## Business Problem

> **"How can Olist reduce customer churn and improve seller performance to grow revenue across its e-commerce marketplace?"**

Olist is Brazil's largest department store marketplace. This project analyses transactional data from 2016–2018 to uncover patterns in customer retention, seller quality, delivery performance, and payment behaviour — delivering actionable recommendations for business growth.

---

## Team

| Name | GitHub |
|---|---|
| Sai Thrishul | [@crazylogic03](https://github.com/crazylogic03) |
| Burra Karthikeya | [@Karthikeya1500](https://github.com/Karthikeya1500) |
| Hariksh Suryawanshi | [@Hariksh](https://github.com/Hariksh) |
| Uday Kumar Choudhary | [@Uday-Choudhary](https://github.com/Uday-Choudhary) |
| Vamsi | [@vamsi2246](https://github.com/vamsi2246) |

---

## Dataset

| Property | Detail |
|---|---|
| **Source** | [Kaggle — Brazilian E-Commerce (Olist)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) |
| **Total Files** | 9 raw CSV files |
| **Total Rows** | ~100,000+ orders |
| **Columns** | 40+ analytical columns across all tables |
| **Date Range** | September 2016 – October 2018 |
| **Format** | Row-level transactional records |

---

## Repository Structure

```
UKTHV/
├── README.md
├── data/
│   ├── raw/                          # Original, unedited source datasets
│   └── processed/                    # Cleaned & analysis-ready outputs
├── notebooks/
│   ├── 01_overview.ipynb             # Dataset profiling & quality assessment
│   ├── 02_cleaning.ipynb             # ETL pipeline & data standardisation
│   ├── 03_eda.ipynb                  # Exploratory data analysis
│   ├── 04_statistical_analysis.ipynb # Correlation, regression, hypothesis tests
│   └── 05_final_load_prep.ipynb      # KPI computation & Tableau export
├── scripts/
│   └── etl_pipeline.py               # Standalone ETL pipeline script
├── tableau/
│   ├── screenshots/                  # Dashboard screenshots
│   └── dashboard_links.md            # Tableau Public URL
└── docs/
    └── data_dictionary.md            # Column-level data dictionary
```

---

## How to Run

```bash
# 1. Clone the repository
git clone https://github.com/crazylogic03/UKTHV.git
cd UKTHV

# 2. Create and activate virtual environment
python -m venv .venv
source .venv/bin/activate   # macOS/Linux
.venv\Scripts\activate      # Windows

# 3. Install dependencies
pip install pandas numpy matplotlib seaborn scipy jupyter

# 4. Launch Jupyter
jupyter notebook

# 5. Run notebooks in order: 01 → 02 → 03 → 04 → 05
```

---

## Key KPIs Analysed

- **Customer Churn Rate** — % of customers who never reordered
- **Average Order Value (AOV)** — Mean revenue per order
- **On-Time Delivery Rate** — % orders delivered before estimated date
- **Average Review Score** — Customer satisfaction metric
- **Seller Performance Score** — Composite metric per seller

---

## Tableau Dashboard

See [`tableau/dashboard_links.md`](tableau/dashboard_links.md) for the live interactive dashboard.

---

## Findings Summary

- ~97% of customers are one-time buyers — a critical churn problem
- Late deliveries strongly correlate with low review scores (r = -0.32)
- Credit card is the dominant payment method (~74% of orders)
- Seller response time is a strong predictor of customer satisfaction
- North-East Brazil has the highest delivery delay rates

---

## Academic Integrity

All analysis, code, and recommendations are the original work of the UKTHV team.
No pre-cleaned datasets, copied dashboards, or reused templates were used.

---

## KPI Framework

| KPI | Definition | Formula / Computation |
|---|---|---|
| **Customer Churn Rate** | Percentage of unique customers who placed only one order | `(count of customer_unique_id appearing once / total unique customers) × 100` (computed in `05_final_load_prep.ipynb`) |
| **Average Order Value (AOV)** | Mean revenue per order including freight | `sum(price + freight_value) / count(order_id)` (computed in `05_final_load_prep.ipynb`) |
| **On-Time Delivery Rate** | Percentage of delivered orders that arrived on or before the estimated date | `count(is_late == 0) / count(delivered orders) × 100` (computed in `02_cleaning.ipynb`) |
| **Average Review Score** | Mean customer star rating across all reviewed, delivered orders | `mean(review_score)` (computed in `03_eda.ipynb`) |
| **Seller Performance Score** | Composite per-seller quality index combining satisfaction and delivery reliability | `0.5 × normalised(avg_review_score) + 0.5 × (1 − seller_late_rate)` (computed in `05_final_load_prep.ipynb`) |

Document KPI logic clearly in `notebooks/04_statistical_analysis.ipynb` and `notebooks/05_final_load_prep.ipynb`.

---

## Tableau Dashboard

| Item | Details |
|---|---|
| **Dashboard URL** | [Customer Segmentation & Revenue Analysis](https://public.tableau.com/views/E-Commerce_Dashboard_17773880667830/CustomerSegmentationRevenueAnalysis?:language=en-GB&:sid=&:redirect=auth&publish=yes&showOnboarding=true&:display_count=n&:origin=viz_share_link) |
| **Executive View** | KPI scorecards (Total Orders, AOV, Churn Rate, On-Time Delivery Rate, Avg Review Score), order volume trend, payment method breakdown, review score distribution |
| **Operational View** | Brazil choropleth map by on-time delivery rate per state; seller tier distribution; top/bottom 10 sellers by performance score; scatter plot of review score vs. late rate |
| **Main Filters** | Date range slider, state multi-select, product category, seller tier, payment type toggle |

Store dashboard screenshots in [`tableau/screenshots/`](tableau/screenshots/) and document the public links in [`tableau/dashboard_links.md`](tableau/dashboard_links.md).

---

## Key Insights

1. **97% of customers never place a second order** — Olist has a retention crisis, not an acquisition crisis. The platform continuously pays to acquire customers who buy once and disappear.
2. **Late delivery is the single largest driver of 1-star reviews** — Pearson r = −0.32 (p < 0.001). Every additional day of delay measurably increases the probability of a 1-star review.
3. **North-East Brazil has a delivery infrastructure crisis** — States like Maranhão, Piauí, and Alagoas show on-time delivery rates 15–20 percentage points below São Paulo.
4. **The bottom 13% of sellers generate 44% of 1-star reviews** — A small group of chronic underperformers is disproportionately damaging the brand.
5. **Credit card customers are the highest-value segment** — ~74% of orders use credit cards. These buyers have higher AOV and use installments more frequently.
6. **Freight costs average 19% of order value** — This is high by global e-commerce standards and likely acts as a psychological barrier to repeat purchases.
7. **Sellers who ship within 24 hours receive significantly higher review scores** — Shipping speed is a stronger predictor of review score than product category or price point.
8. **The top 20% of sellers drive 60% of GMV** — Key concentration risk. If elite sellers leave the platform, GMV would be severely impacted.
9. **Installment plans of 1–3 months dominate** — Customers are price-sensitive and use installments as a credit mechanism.
10. **Black Friday is the only significant seasonal spike** — Other seasonal events (Mother's Day, Valentine's Day) are entirely untapped GMV growth opportunities.
11. **Product weight is the primary freight cost driver** — Categories like furniture and home appliances carry disproportionately high freight ratios.
12. **Anomalous delivery delays (>30 days) are traceable to specific seller IDs** — This indicates a seller-level problem, not purely a logistics infrastructure issue.

---

## Recommendations

| # | Insight | Recommendation | Expected Impact |
|---|---|---|---|
| 1 | 97% of customers are one-time buyers | Launch an automated 30-day post-purchase CRM re-engagement sequence with a 10–15% personalised discount trigger for first-time buyers | 5% conversion of one-time buyers ≈ 4,600 new repeat customers; estimated BRL 14–18M incremental annual GMV |
| 2 | Bottom 13% of sellers drive 44% of 1-star reviews | Implement a three-tier Seller SLA system (Elite / Standard / At-Risk); At-Risk sellers receive a 60-day remediation window; persistent underperformers face listing restrictions | −30–40% in 1-star reviews; +5–8 NPS points; +3–5% platform GMV |
| 3 | North-East Brazil on-time rate is 15–20 points below average | Negotiate dedicated last-mile logistics contracts with regional carriers in MA, PI, AL, SE, RN; explore micro-fulfilment hubs in Recife and Fortaleza | −15–20% logistics claims and refund costs; unlock access to 50M+ underserved consumers |

---

## Tech Stack

| Tool | Status | Purpose |
|---|---|---|
| Python + Jupyter Notebooks | Mandatory | ETL, cleaning, analysis, and KPI computation |
| Google Colab | Supported | Cloud notebook execution environment |
| Tableau Public | Mandatory | Dashboard design, publishing, and sharing |
| GitHub | Mandatory | Version control, collaboration, contribution audit |

**Python libraries used:** `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`

---

## Contribution Matrix

This table must match evidence in GitHub Insights, PR history, and committed files.

| Team Member | Dataset & Sourcing | ETL & Cleaning | EDA & Analysis | Statistical Analysis | Tableau Dashboard | Report Writing | PPT & Viva |
|---|---|---|---|---|---|---|---|
| Sai Thrishul | Owner | Owner | — | Owner | Support | — | — |
| Burra Karthikeya | Owner | Owner | — | — | Owner | Owner | — |
| Hariksh Suryawanshi | Owner | — | Owner | Owner | Owner | — | — |
| Uday Kumar Choudhary | Owner | — | Owner | — | Owner | — | Owner |
| Vamsi | Owner | — | — | — | Owner | Owner | Owner |

_Declaration: We confirm that the above contribution details are accurate and verifiable through GitHub Insights, PR history, and submitted artifacts._

**Team Lead Name:** Sai Thrishul

**Date:** April 28, 2026

---

## Academic Integrity

All analysis, code, and recommendations in this repository must be the original work of the team listed above. Free-riding is tracked via GitHub Insights and pull request history. Any mismatch between the contribution matrix and actual commit history may result in individual grade adjustments.

---

*Newton School of Technology - Data Visualization & Analytics | Capstone 2*
