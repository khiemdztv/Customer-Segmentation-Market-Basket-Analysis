<div align="center">

# 🛒 Customer Segmentation & Market Basket Analysis
### E-Commerce Retail Behavior Analysis via Data Mining

[![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter&logoColor=white)](https://jupyter.org)
[![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-KMeans-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)](https://scikit-learn.org)
[![License](https://img.shields.io/badge/License-MIT-22C55E?style=for-the-badge)](LICENSE)

> **Applying end-to-end data mining to segment retail customers using RFM + K-Means and discover hidden product relationships using the Apriori algorithm — turning raw transactions into actionable business strategies.**

</div>

---

## 📌 Overview

This project analyzes **541,909 e-commerce transactions** from the [Online Retail II dataset (UCI ML Repository)](https://archive.ics.uci.edu/dataset/502/online+retail+ii) to address two core business problems:

| Problem | Approach | Outcome |
|---|---|---|
| Who are my most valuable customers? | RFM Analysis + K-Means Clustering (k=4) | 4 distinct customer segments identified |
| Which products are bought together? | Apriori Algorithm (Market Basket Analysis) | 87 association rules, 13 with Lift > 10 |

**Key Business Insight:** The top 25% of customers generate **60% of total revenue**, and cross-selling opportunities identified could unlock approximately **£1.585 million** in additional revenue.

---

## 🎯 Objectives

- Clean and preprocess messy real-world retail transaction data (missing values, returns, outliers)
- Engineer meaningful features (RFM scores, basket size, time-based features)
- Segment customers into strategic groups using unsupervised machine learning
- Mine frequent itemsets and association rules to support cross-selling strategies
- Visualize insights through Python charts and an interactive **Power BI dashboard**

---

## 📁 Repository Structure

```
Customer-Segmentation-Market-Basket-Analysis/
│
├── 📓 retail_project.ipynb          # Main analysis notebook (EDA → RFM → KMeans → Apriori)
├── 📊 data_mining.pbix              # Interactive Power BI dashboard
├── 📄 online_retail_II.csv          # Raw dataset (Online Retail II, UCI)
└── 📋 README.md
```

---

## 📦 Dataset

**Source:** [Online Retail II — UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/502/online+retail+ii)

A UK-based non-store online retailer's transactional data from **01/12/2009 – 09/12/2011**.

| Column | Type | Description |
|---|---|---|
| `Invoice` | int | Unique invoice number per transaction |
| `StockCode` | str | Product code |
| `Description` | str | Product name |
| `Quantity` | int | Number of units per transaction |
| `InvoiceDate` | datetime | Date and time of the transaction |
| `Price` | float | Unit price (GBP £) |
| `Customer ID` | float | Unique customer identifier |
| `Country` | str | Country of the customer |

| Metric | Value |
|---|---|
| Raw transactions | 541,909 |
| After cleaning | 398,410 |
| Unique customers analyzed | ~4,381 |

---

## 🔧 Data Preprocessing (5-Step Pipeline)

```
Raw Data (541,909 rows)
     │
     ├─ Step 1: Remove duplicate transactions  (Invoice + StockCode + Quantity + Date)
     ├─ Step 2: Parse & validate InvoiceDate → drop invalid datetime rows
     ├─ Step 3: Filter Price ≤ 0 and Quantity < 0  (cancelled / returned orders)
     ├─ Step 4: Drop rows with null Customer ID
     └─ Step 5: IQR-based outlier removal on Quantity  (Q3 + 1.5 × IQR threshold)
     │
Clean Data (398,410 rows)
```

**Feature Engineering:**
- `Revenue = Quantity × Price`
- `basket_size` — number of distinct products per invoice
- Time features: `hour`, `weekday`, `weekday_num`, `month`, `year`, `is_weekend`

---

## 🤖 Methodology

### Part 1 — Customer Segmentation (RFM + K-Means)

**Step 1: Build RFM Table**

| Metric | Definition |
|---|---|
| **Recency (R)** | Days since the customer's last purchase |
| **Frequency (F)** | Total number of unique invoices |
| **Monetary (M)** | Total revenue generated (£) |

**Step 2: Normalize** using Log Transform + `StandardScaler` to handle skewed distributions before clustering.

**Step 3: Determine Optimal k** via the Elbow Method (WCSS) and Silhouette Score.

> ✅ **Optimal k = 4** — Silhouette Score: **0.42** | Davies-Bouldin Index: **0.85**

**Step 4: K-Means Clustering → 4 Customer Segments**

| Cluster | Label | Profile | Est. Size | Recommended Strategy |
|---|---|---|---|---|
| 🏆 Cluster 2 | **VIP** | Low Recency, High Frequency, High Monetary | ~1,087 | Loyalty rewards, exclusive early access |
| 🌱 Cluster 1 | **Potential** | Moderate R & F, Growing spend | ~1,527 | Upsell campaigns, nurture programs |
| ⚠️ Cluster 0 | **At-Risk** | High Recency (inactive), Low F | ~1,305 | Win-back emails, re-engagement offers |
| 🆕 Cluster 3 | **New** | Very recent, Low F & M | ~462 | Onboarding flow, welcome incentives |

> 📌 **Top 25% of customers (VIP segment) drive 60% of total revenue.**

---

### Part 2 — Market Basket Analysis (Apriori Algorithm)

**Parameters used:**
```python
min_support    = 0.02   # Item pair appears in ≥ 2% of all transactions
metric         = 'lift'
min_threshold  = 1.0    # Retain only positive correlations (Lift > 1)
```

**Results:**

| Category | Count |
|---|---|
| Total association rules discovered | **87** |
| 🔴 Very Strong rules (Lift > 10) | **13** |
| 🟠 Strong rules (5 < Lift ≤ 10) | 24 |
| 🟡 Medium rules (2 < Lift ≤ 5) | 32 |
| 🟢 Weak rules (1 < Lift ≤ 2) | 18 |

**Metric Reference:**

| Metric | Meaning |
|---|---|
| **Support** | Popularity of the itemset — % of transactions containing it |
| **Confidence** | P(buying B \| bought A) — predictive strength |
| **Lift** | How much more likely A→B compared to random chance (Lift > 1 = positive) |

> 📌 **13 rules with Lift > 10** provide a strong scientific basis for product bundling and cross-selling recommendations.

---

## 📊 Visualizations

| Chart | Insight |
|---|---|
| RFM Distribution Histograms | Spread of Recency, Frequency, Monetary values |
| Elbow Method Plot (WCSS) | Selection of optimal k=4 |
| 2D Scatter Plots — RFM Clusters | Cluster separation across R/F/M dimensions |
| Box Plot — Monetary by Cluster | Revenue distribution per segment |
| Silhouette Plot | Internal cluster quality validation |
| Revenue by Hour (Line Chart) | Peak shopping windows during the day |
| Heatmap — Weekday × Time Block | High-revenue day/time combinations |
| Top 10 Rules Bar Chart (by Lift) | Strongest product associations |
| Confidence vs Lift Scatter (bubble = support) | Rule quality vs frequency tradeoff |
| Pie Chart — Lift Category Distribution | Quality breakdown of all 87 rules |
| **Power BI Dashboard** | Interactive filters for segment, country, time, products |

---

## 🚀 Getting Started

### 1. Install Dependencies

```bash
pip install pandas numpy scikit-learn mlxtend matplotlib seaborn openpyxl
```

### 2. Run the Notebook

```bash
git clone https://github.com/khiemdztv/Customer-Segmentation-Market-Basket-Analysis.git
cd Customer-Segmentation-Market-Basket-Analysis
jupyter notebook retail_project.ipynb
```

The notebook is fully self-contained and runs top-to-bottom: data loading → cleaning → RFM → clustering → Apriori → visualizations.

### 3. Explore the Dashboard

Open `data_mining.pbix` in **Microsoft Power BI Desktop** to interact with the full dashboard (segment filters, time slicers, product association explorer).

---

## 💡 Business Recommendations

### Customer Segment Strategies

| Segment | Action |
|---|---|
| 🏆 **VIP** | Personalized loyalty programs, priority customer service, early product access |
| 🌱 **Potential** | Frequency-triggered email campaigns, spend-milestone rewards |
| ⚠️ **At-Risk** | "We miss you" discounts, limited-time reactivation offers |
| 🆕 **New** | Welcome vouchers, onboarding emails, recommended bestsellers |

### Cross-Selling & Bundling (from Apriori Rules)
- Package top rule product pairs at promotional bundle pricing
- Deploy *"Customers who bought X also bought Y"* recommendations
- Restructure homepage / store layout based on product affinity clusters
- Run A/B tests to measure uplift from Apriori-driven recommendations

---

## 🛠️ Tech Stack

| Tool | Usage |
|---|---|
| **Python 3.9+** | Core analysis language |
| **pandas** | Data manipulation and feature engineering |
| **scikit-learn** | StandardScaler, KMeans, Silhouette/DB evaluation |
| **mlxtend** | Apriori algorithm, association rule mining |
| **matplotlib / seaborn** | Statistical and cluster visualizations |
| **Power BI** | Interactive business intelligence dashboard |
| **Jupyter Notebook** | Development and presentation environment |

---

## 👥 Team

**Group 10 — Data Mining Course (ITS324_251_1_D04)**
Banking University of Ho Chi Minh City | Instructor: Ngô Thanh Hùng

| # | Member | Student ID | Role |
|---|---|---|---|
| 1 | Lâm Tuấn Vũ | 030239230292 | Project lead, results synthesis, conclusion |
| 2 | Nguyễn Vũ Thắng | 030239230212 | Data cleaning pipeline, EDA, documentation |
| 3 | **Đỗ Gia Khiêm** | **030239230090** | **RFM computation, K-Means clustering, Elbow method** |
| 4 | Nguyễn Song Khang | — | Apriori algorithm, data visualizations |
| 5 | Phạm Thanh Tuyền | — | Business recommendations, cross-sell strategy |

---

## 📚 References

1. Agrawal, R. & Srikant, R. (1994). *Fast algorithms for mining association rules.* VLDB Conference.
2. Hughes, A. M. (1994). *Strategic database marketing.* Probus Publishing.
3. MacQueen, J. (1967). *Some methods for classification and analysis of multivariate observations.* Berkeley Symposium.
4. Statista (2024). *Global e-commerce revenue — $6.3 trillion USD.*
5. UCI Machine Learning Repository — [Online Retail II Dataset](https://archive.ics.uci.edu/dataset/502/online+retail+ii)

---

## 📄 License

This project is licensed under the [MIT License](LICENSE) — free to use for academic and educational purposes.

---

<div align="center">

*Crafted with ❤️ — Banking University of Ho Chi Minh City, 2025*

**⭐ If this project helped you, a star would be appreciated!**

</div>
