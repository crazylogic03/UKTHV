# NST DVA Capstone 2 — Olist E-Commerce Analytics

> **Newton School of Technology | Data Visualization & Analytics**
> A 2-week industry simulation capstone using Python, GitHub, and Tableau to convert raw data into actionable business intelligence.

---

## Before You Start

1. Repository named using the format `SectionName_TeamID_ProjectName` → **UKTHV**
2. Project details and team table filled below.
3. Raw dataset added to `data/raw/`.
4. Notebooks completed in order from `01` to `05`.
5. Final dashboard published — public link in `tableau/dashboard_links.md`.
6. Final report and presentation PDFs exported to `reports/`.

### Quick Start

If you are working locally:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook
```

If you are working in Google Colab:

- Upload or sync the notebooks from `notebooks/`
- Keep the final `.ipynb` files committed to GitHub
- Export any cleaned datasets into `data/processed/`

---

## Project Overview

| Field | Details |
|---|---|
| **Project Title** | Olist E-Commerce Analytics — Customer Churn, Seller Performance & Delivery Intelligence |
| **Sector** | Retail / E-Commerce |
| **Team ID** | UKTHV |
| **Section** | DVA Capstone 2 |
| **Faculty Mentor** | To be filled |
| **Institute** | Newton School of Technology |
| **Submission Date** | April 28, 2026 |

### Team Members

| Role | Name | GitHub Username |
|---|---|---|
| Project Lead | Sai Thrishul | [@crazylogic03](https://github.com/crazylogic03) |
| Data Lead | Burra Karthikeya | [@Karthikeya1500](https://github.com/Karthikeya1500) |
| ETL Lead | Hariksh Suryawanshi | [@Hariksh](https://github.com/Hariksh) |
| Analysis Lead | Uday Kumar Choudhary | [@Uday-Choudhary](https://github.com/Uday-Choudhary) |
| Visualization Lead | Vamsi | [@vamsi2246](https://github.com/vamsi2246) |

---

## Business Problem

Olist is Brazil's largest department store marketplace, connecting thousands of small and medium sellers to customers across all 27 Brazilian states. Despite strong order volume growth from 2016 to 2018, the platform suffers from a critical customer retention problem — the vast majority of customers purchase only once and never return. Simultaneously, inconsistent seller performance and regional delivery failures in under-served states are eroding customer trust and suppressing revenue growth. This project analyses Olist's full transactional dataset to diagnose the root causes of churn, identify delivery bottlenecks, and rank sellers by performance — delivering evidence-backed recommendations for sustainable marketplace growth.

**Core Business Question**

> How can Olist reduce customer churn and improve seller performance to grow revenue across its e-commerce marketplace?

**Decision Supported**

> This analysis enables Olist's Chief Revenue Officer and Head of Marketplace Operations to prioritise re-engagement investment, enforce seller SLA tiers, and target logistics improvement in the regions with the highest delivery failure rates.

---

## Dataset

| Attribute | Details |
|---|---|
| **Source Name** | Brazilian E-Commerce Public Dataset by Olist (Kaggle) |
| **Direct Access Link** | [https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) |
| **Row Count** | ~99,441 orders (core table); ~112,650 order items |
| **Column Count** | 40+ analytical columns across all 9 tables |
| **Time Period Covered** | September 2016 – October 2018 |
| **Format** | 9 relational CSV files joined via order_id / customer_id / seller_id |

**Key Columns Used**

| Column Name | Description | Role in Analysis |
|---|---|---|
| `customer_unique_id` | True per-person customer identifier (persists across orders) | Churn analysis — identifies one-time vs. repeat buyers |
| `order_delivered_customer_date` | Actual date the order reached the customer | On-time delivery KPI computation |
| `order_estimated_delivery_date` | Promised delivery date shown to customer at purchase | Baseline for delivery delay calculation |
| `review_score` | 1–5 star rating submitted by the customer post-delivery | Primary customer satisfaction metric |
| `payment_type` | Payment method: credit_card, boleto, voucher, debit_card | Payment behaviour segmentation |
| `price` | Item price in BRL (excluding freight) | AOV and revenue analysis |
| `freight_value` | Shipping cost in BRL | Freight burden analysis |
| `seller_id` | Unique identifier for each seller | Seller performance scoring and segmentation |
| `customer_state` | 2-letter Brazilian state code | Regional delivery performance analysis |
| `product_category_name_english` | English-translated product category | Category-level performance breakdown |

For full column definitions, see [`docs/data_dictionary.md`](docs/data_dictionary.md).

---

## KPI Framework

| KPI | Definition | Formula / Computation |
|---|---|---|
| **Customer Churn Rate** | Percentage of unique customers who placed only one order in the full dataset window | `(count of customer_unique_id appearing once / total unique customers) × 100` — computed in `05_final_load_prep.ipynb` |
| **Average Order Value (AOV)** | Mean revenue per order including freight | `sum(price + freight_value) / count(order_id)` — computed in `05_final_load_prep.ipynb` |
| **On-Time Delivery Rate** | Percentage of delivered orders that arrived on or before the estimated date | `count(is_late == 0) / count(delivered orders) × 100` — `is_late` derived in `02_cleaning.ipynb` |
| **Average Review Score** | Mean customer star rating across all reviewed, delivered orders | `mean(review_score)` grouped by segment — computed in `03_eda.ipynb` |
| **Seller Performance Score** | Composite per-seller quality index combining satisfaction and delivery reliability | `0.5 × normalised(avg_review_score) + 0.5 × (1 − seller_late_rate)` — computed in `05_final_load_prep.ipynb` |

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

1. **97% of customers never place a second order** — Olist has a retention crisis, not an acquisition crisis. The platform continuously pays to acquire customers who buy once and disappear, creating an unsustainable unit economics model.
2. **Late delivery is the single largest driver of 1-star reviews** — Pearson r = −0.32 (p < 0.001). Every additional day of delay measurably increases the probability of a 1-star review. Delivery speed is the most controllable lever for satisfaction improvement.
3. **North-East Brazil has a delivery infrastructure crisis** — States like Maranhão, Piauí, and Alagoas show on-time delivery rates 15–20 percentage points below São Paulo, with average delays of 7–12 days. Every failed delivery in these states is a permanent customer loss.
4. **The bottom 13% of sellers generate 44% of 1-star reviews** — A small group of chronic underperformers is disproportionately damaging the brand. Identifying, remediating, or removing these sellers is a targeted, solvable problem.
5. **Credit card customers are the highest-value segment** — ~74% of orders use credit cards. These buyers have higher AOV, use installments more frequently, and are the most likely candidates for loyalty programme conversion.
6. **Freight costs average 19% of order value** — This is high by global e-commerce standards and likely acts as a psychological barrier to repeat purchases, especially for lower-income customers and those in freight-heavy regions.
7. **Sellers who ship within 24 hours receive significantly higher review scores** — Shipping speed is a stronger predictor of review score than product category or price point. This metric should be a formal SLA requirement.
8. **The top 20% of sellers drive 60% of GMV** — Key concentration risk. If even a few elite sellers leave the platform, GMV would be severely impacted. A dedicated elite seller retention programme is a strategic priority.
9. **Installment plans of 1–3 months dominate** — Customers are price-sensitive and use installments as a credit mechanism. Promotional 0% financing offers on high-AOV categories could lift conversion significantly.
10. **Black Friday is the only significant seasonal spike** — Other seasonal events (Mother's Day, Valentine's Day, back-to-school) are entirely untapped GMV growth opportunities.
11. **Product weight is the primary freight cost driver** — Categories like furniture and home appliances carry disproportionately high freight ratios, reducing their margin attractiveness. Weight-based freight optimisation could unlock growth in these categories.
12. **Anomalous delivery delays (>30 days) are traceable to specific seller IDs** — ~1.2% of delivered orders experienced extreme delays, concentrated in North-East states and tied to identifiable sellers — this is a seller-level problem, not purely a logistics infrastructure issue.

---

## Recommendations

| # | Insight | Recommendation | Expected Impact |
|---|---|---|---|
| 1 | 97% of customers are one-time buyers | Launch an automated 30-day post-purchase CRM re-engagement sequence with a 10–15% personalised discount trigger for first-time buyers | 5% conversion of one-time buyers ≈ 4,600 new repeat customers; estimated BRL 14–18M incremental annual GMV |
| 2 | Bottom 13% of sellers drive 44% of 1-star reviews | Implement a three-tier Seller SLA system (Elite / Standard / At-Risk); At-Risk sellers receive a 60-day remediation window; persistent underperformers face listing restrictions | −30–40% in 1-star reviews; +5–8 NPS points; +3–5% platform GMV |
| 3 | North-East Brazil on-time rate is 15–20 points below average | Negotiate dedicated last-mile logistics contracts with regional carriers in MA, PI, AL, SE, RN; explore micro-fulfilment hubs in Recife and Fortaleza | −15–20% logistics claims and refund costs; unlock access to 50M+ underserved consumers |

---

## Repository Structure

```text
UKTHV/
|
|-- README.md
|
|-- data/
|   |-- raw/                          # Original dataset — never edited
|   `-- processed/                    # Cleaned output from ETL pipeline
|
|-- notebooks/
|   |-- 01_overview.ipynb             # Dataset profiling and quality assessment
|   |-- 02_cleaning.ipynb             # ETL pipeline and data standardisation
|   |-- 03_eda.ipynb                  # Exploratory data analysis
|   |-- 04_statistical_analysis.ipynb # Correlation, regression, hypothesis tests
|   `-- 05_final_load_prep.ipynb      # KPI computation and Tableau export
|
|-- scripts/
|   `-- etl_pipeline.py               # Standalone ETL pipeline script
|
|-- tableau/
|   |-- screenshots/                  # Dashboard screenshots
|   `-- dashboard_links.md            # Tableau Public URL
|
|-- docs/
|   `-- data_dictionary.md            # Full column-level data dictionary
```

---

## Analytical Pipeline

The project follows a structured 7-step workflow:

1. **Define** — Sector selected, problem statement scoped, mentor approval obtained.
2. **Extract** — Raw dataset sourced and committed to `data/raw/`; data dictionary drafted.
3. **Clean and Transform** — Cleaning pipeline built in `notebooks/02_cleaning.ipynb` and `scripts/etl_pipeline.py`.
4. **Analyze** — EDA and statistical analysis performed in notebooks `03` and `04`.
5. **Visualize** — Interactive Tableau dashboard built and published on Tableau Public.
6. **Recommend** — 3 data-backed business recommendations delivered with impact estimates.
7. **Report** — Final project report and presentation deck completed.

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

## Evaluation Rubric

| Area | Marks | Focus |
|---|---|---|
| Problem Framing | 10 | Is the business question clear and well-scoped? |
| Data Quality and ETL | 15 | Is the cleaning pipeline thorough and documented? |
| Analysis Depth | 25 | Are statistical methods applied correctly with insight? |
| Dashboard and Visualization | 20 | Is the Tableau dashboard interactive and decision-relevant? |
| Business Recommendations | 20 | Are insights actionable and well-reasoned? |
| Storytelling and Clarity | 10 | Is the presentation professional and coherent? |
| **Total** | **100** | |

> Marks are awarded for analytical thinking and decision relevance, not chart quantity, visual decoration, or code length.

---

## Submission Checklist

**GitHub Repository**

- [x] Public repository created with correct naming convention
- [x] All notebooks committed in `.ipynb` format
- [x] `data/raw/` contains the original, unedited dataset
- [x] `data/processed/` contains the cleaned pipeline output
- [x] `tableau/screenshots/` contains dashboard screenshots
- [x] `tableau/dashboard_links.md` contains the Tableau Public URL
- [x] `docs/data_dictionary.md` is complete
- [x] `README.md` explains the project, dataset, and team
- [x] All members have visible commits

**Tableau Dashboard**

- [x] Published on Tableau Public and accessible via public URL
- [x] Interactive filters included (date range, state, category, seller tier)
- [x] Dashboard directly addresses the business problem

**Project Report**

- [ ] Final report exported as PDF into `reports/`
- [x] Cover page, executive summary, sector context, problem statement
- [x] Data description, cleaning methodology, KPI framework
- [x] EDA with written insights, statistical analysis results
- [x] Dashboard screenshots and explanation
- [x] 12 key insights in decision language
- [x] 3 actionable recommendations with impact estimates
- [x] Contribution matrix matches GitHub history

---

## Contribution Matrix

This table matches evidence in GitHub Insights, PR history, and committed files.

| Team Member | Dataset & Sourcing | ETL & Cleaning | EDA & Analysis | Statistical Analysis | Tableau Dashboard | Report Writing | PPT & Viva |
|---|---|---|---|---|---|---|---|
| Sai Thrishul | Owner | Owner | — | Owner | Support | — | — |
| Burra Karthikeya | Owner | Owner | — | — | Owner | Owner | — |
| Hariksh Suryawanshi | Owner | — | Owner | Owner | Owner | — | — |
| Uday Kumar Choudhary | Owner | — | Owner | — | Owner | — | Owner |
| Vamsi | Owner | — | — | — | Owner | Owner | Owner |

_Declaration: We confirm that the above contribution details are accurate and verifiable through GitHub Insights, PR history, and submitted artifacts._

**Team Lead:** Sai Thrishul

**Date:** April 28, 2026

---

## Academic Integrity

All analysis, code, and recommendations in this repository are the original work of the UKTHV team. No pre-cleaned datasets, copied dashboards, or reused templates were used. Free-riding is tracked via GitHub Insights and pull request history. Any mismatch between the contribution matrix and actual commit history may result in individual grade adjustments.

---

*Newton School of Technology — Data Visualization & Analytics | Capstone 2*
