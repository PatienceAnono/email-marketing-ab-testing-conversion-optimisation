# 📧 Email Marketing A/B Testing & Conversion Optimisation Analysis

<div align="center">

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.0+-150458?style=flat&logo=pandas&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-Statistical%20Testing-8CAAE6?style=flat&logo=scipy&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard%20Ready-F2C811?style=flat&logo=powerbi&logoColor=black)
![Status](https://img.shields.io/badge/Status-Complete-2EC4B6?style=flat)
![Verdict](https://img.shields.io/badge/Verdict-Roll%20Out%20Variant%20B-success?style=flat)

**Consulting-grade A/B test evaluation for an e-commerce email marketing campaign.**  
*Full funnel analysis · Statistical hypothesis testing · Customer segmentation · Revenue impact quantification*

[View Notebook](#) · [Case Study PDF](#) · [PA Data Analytics](https://padataanalytics.com)

</div>

---

## 📌 Project Overview

An e-commerce growth team wanted to know: **does Email Variant B actually perform better than Variant A — and by how much?**

This project answers that question with a complete consulting-grade analysis across 25,000 customer records over a 90-day email campaign. Every stage of the funnel is evaluated. Every KPI is tested for statistical significance. The revenue impact of full rollout is quantified in hard numbers.

**The answer:** Variant B wins across every single metric. The analysis recommends immediate full rollout.

---

## 🏆 Key Results

| Metric | Variant A | Variant B | Lift | Significant? |
|--------|-----------|-----------|------|-------------|
| Open Rate | 23.99% | 36.66% | **+52.8%** | ✅ p < 0.0001 |
| Click Rate | 4.24% | 10.15% | **+139.4%** | ✅ p < 0.0001 |
| Conversion Rate | 0.59% | 1.76% | **+201.2%** | ✅ p < 0.0001 |
| Revenue Per Send | $4.56 | $18.33 | **+301.9%** | ✅ p < 0.0001 |
| Unsubscribe Rate | 1.50% | 1.50% | No change | ✅ No adverse effect |

### 💰 Revenue Impact

```
Incremental revenue (this campaign)    →    $344,277
Projected monthly uplift (full rollout) →    $688,555 / month
Projected annual revenue uplift        →    $8,262,660 / year
```

> Based on 25,000 sends per campaign × 2 campaigns per month

---

## 📁 Repository Structure

```
email-ab-testing-analysis/
│
├── 📓 Email_Marketing_AB_Testing_Analysis.ipynb   ← Main analysis notebook
│
├── 📄 AB_Testing_Case_Study_Patience_Anono.docx   ← Full stakeholder case study
│
├── 📂 data/
│   ├── raw/
│   │   └── Marketing-Experimentation-AB-Testing.csv
│   └── processed/
│       ├── email_ab_testing_clean.csv             ← Cleaned analysis dataset
│       └── email_ab_testing_powerbi.csv           ← Power BI-ready dataset
│
├── 📂 visuals/
│   ├── 3_1_variant_split.png
│   ├── 3_2_segment_balance.png
│   ├── 3_3_demographics.png
│   ├── 3_4_revenue_distribution.png
│   ├── 4_1_funnel_analysis.png
│   ├── 4_2_funnel_rates.png
│   ├── 4_3_funnel_waterfall.png
│   ├── 5_4_significance_summary.png
│   ├── 6_1_segment_conversion.png
│   ├── 6_2_device_analysis.png
│   ├── 6_3_subject_lines.png
│   ├── 6_4_send_time.png
│   ├── 6_5_heatmap.png
│   ├── 7_1_revenue_lift.png
│   ├── 7_2_annual_forecast.png
│   ├── 7_3_revenue_impact.png
│   └── 8_1_executive_dashboard.png
│
└── README.md
```

---

## 📊 Dataset

| Property | Detail |
|----------|--------|
| **Records** | 25,000 customer rows |
| **Campaign period** | January – April 2024 (90 days) |
| **Columns** | 17 raw → 19 after feature engineering |
| **Groups** | Variant A: 12,639 (50.6%) · Variant B: 12,361 (49.4%) |
| **Regions** | East Africa, West Africa, North America, Europe, Middle East, South Asia |
| **Segments** | VIP, Loyal, Returning, At-Risk, New Customer |
| **Devices** | Mobile (62%), Desktop (31%), Tablet (7%) |
| **Product Categories** | Fashion & Apparel, Electronics, Food & Beverage, Sports & Fitness, Home & Living, Health & Beauty |

### Column Reference

| Column | Type | Description |
|--------|------|-------------|
| `customer_id` | int | Unique customer identifier |
| `variant` | str | A (control) or B (treatment) |
| `customer_segment` | str | VIP / Loyal / Returning / At-Risk / New Customer |
| `age_group` | str | 18-24 / 25-34 / 35-44 / 45-54 / 55+ |
| `region` | str | Geographic region (6 values) |
| `device_type` | str | Mobile / Desktop / Tablet |
| `product_category` | str | 6 product categories |
| `subject_line_type` | str | 6 subject line variants |
| `send_hour` | int | Hour of send (7–21) |
| `opened` | int | 1 = email opened |
| `clicked` | int | 1 = link clicked |
| `purchased` | int | 1 = purchase made |
| `revenue_usd` | float | Revenue generated (0 for non-purchasers) |
| `unsubscribed` | int | 1 = unsubscribed |

---

## 🔬 Analysis Methodology

### Phase 1 — Data Quality Assessment
- Missing value audit across all 17 columns
- Duplicate detection (0 exact row duplicates; 299 duplicate customer IDs treated as valid distinct send events)
- Funnel logic validation (zero clicks-without-opens; zero purchases-without-clicks)
- Structural vs random missingness identification

**Data treatments applied:**
```python
# Categorical missingness → 'Unknown'
df[col].fillna('Unknown')

# New customers (no purchase history) → impute median + flag
df['new_customer_flag'] = df['days_since_last_purchase'].isnull().astype(int)
df['days_since_last_purchase'].fillna(df['days_since_last_purchase'].median())

# Non-openers (time_to_open structurally N/A) → sentinel
df['time_to_open_hrs'].fillna(-1)
```

### Phase 2 — Exploratory Data Analysis
- Variant split validation (50.6% / 49.4% — properly randomised)
- Segment balance confirmation across customer type, region, device, age group
- Revenue distribution analysis → confirmed highly right-skewed, zero-heavy → mandates non-parametric testing

### Phase 3 — Funnel Analysis
Full funnel construction from Sent → Opened → Clicked → Purchased for each variant, including:
- Absolute customer counts at each stage
- Stage-level conversion rates
- Absolute and relative lift calculations
- Drop-off waterfall analysis

### Phase 4 — Statistical Significance Testing

**Binary KPIs (Open / Click / Conversion rates):**  
Two-Proportion Z-Test — one-tailed at α = 0.05

```python
def two_prop_z_test(n1, x1, n2, x2, alternative='larger'):
    p_pool = (x1 + x2) / (n1 + n2)
    se = np.sqrt(p_pool * (1 - p_pool) * (1/n1 + 1/n2))
    z  = (p2 - p1) / se
    p_val = 1 - norm.cdf(z)   # one-tailed
    ci = (p2 - p1) ± 1.96 * se
```

**Revenue (continuous, non-normal):**  
Mann-Whitney U Test (primary) + Welch T-Test (validation)

```python
u_stat, p_mw = mannwhitneyu(rev_B, rev_A, alternative='greater')
t_stat, p_tt = ttest_ind(rev_B, rev_A, alternative='greater', equal_var=False)
```

> Mann-Whitney is preferred here: revenue is zero-heavy and right-skewed — parametric tests are unreliable without normality.

### Phase 5 — Customer Segmentation Analysis
Performance breakdown by:
- Customer segment (VIP / Loyal / Returning / At-Risk / New)
- Device type (Mobile / Desktop / Tablet)
- Subject line type (6 variants)
- Send time bucket (Morning / Afternoon / Evening / Night)
- Segment × Device heatmap

### Phase 6 — Revenue Impact Analysis
- Campaign-level total revenue comparison
- Revenue per send and revenue per purchaser
- Annualised projections at full rollout scale
- Monthly incremental revenue modelling

---

## 📈 Statistical Test Results

```
════════════════════════════════════════════════════════════
  SIGNIFICANCE TESTING RESULTS — α = 0.05
  Hypothesis: Variant B > Variant A (one-tailed)
════════════════════════════════════════════════════════════

── Open Rate Test ──────────────────────────────────────────
  Variant A:    23.989%  (3,032 / 12,639)
  Variant B:    36.656%  (4,531 / 12,361)
  Z-Statistic:  21.7983
  P-Value:      < 0.0001
  95% CI:       [+11.53%, +13.81%]
  Result:       ✅ STATISTICALLY SIGNIFICANT

── Click Rate Test ─────────────────────────────────────────
  Variant A:    4.241%   (536 / 12,639)
  Variant B:    10.153%  (1,255 / 12,361)
  Z-Statistic:  18.1224
  P-Value:      < 0.0001
  95% CI:       [+5.27%, +6.55%]
  Result:       ✅ STATISTICALLY SIGNIFICANT

── Conversion Rate Test ────────────────────────────────────
  Variant A:    0.585%   (74 / 12,639)
  Variant B:    1.764%   (218 / 12,361)
  Z-Statistic:  8.6683
  P-Value:      < 0.0001
  95% CI:       [+0.91%, +1.45%]
  Result:       ✅ STATISTICALLY SIGNIFICANT

── Revenue Test (Mann-Whitney U) ───────────────────────────
  Variant A Mean:  $4.5622 / send
  Variant B Mean:  $18.3333 / send
  U-Statistic:     79,036,470
  P-Value:         < 0.0001
  Result:          ✅ STATISTICALLY SIGNIFICANT
```

---

## 🧩 Segmentation Findings

### By Customer Segment

| Segment | Conv Rate A | Conv Rate B | Lift (pp) |
|---------|------------|------------|-----------|
| VIP Customer | 1.24% | 4.30% | **+3.06pp** |
| Loyal Customer | 1.19% | 2.61% | **+1.42pp** |
| Returning Customer | 0.61% | 1.81% | **+1.20pp** |
| At-Risk Customer | 0.06% | 1.25% | **+1.18pp** |
| New Customer | 0.30% | 0.97% | **+0.68pp** |

> **VIP and Loyal Customers** show the highest absolute lift — these are the highest-priority cohorts for rollout. The At-Risk segment shows notable responsiveness, suggesting Variant B can serve as a re-engagement tool.

### By Send Time Bucket

| Time Window | Hours | Performance |
|-------------|-------|-------------|
| 🔴 Evening | 17:00–20:59 | Highest open & conversion rates |
| 🟡 Morning | 05:00–11:59 | Above average |
| 🟢 Afternoon | 12:00–16:59 | Near average |
| ❌ Night | 21:00–04:59 | Lowest engagement |

---

## 🗂️ Visuals Preview

<details>
<summary>Click to see all 17 charts generated</summary>

| File | Description |
|------|-------------|
| `3_1_variant_split.png` | Experiment group distribution — bar + pie |
| `3_2_segment_balance.png` | Segment balance validation across variants |
| `3_3_demographics.png` | Age group and customer segment distributions |
| `3_4_revenue_distribution.png` | Revenue histogram, box plot, descriptive stats |
| `4_1_funnel_analysis.png` | Full funnel — absolute counts and % rates |
| `4_2_funnel_rates.png` | Absolute and relative lift at each funnel stage |
| `4_3_funnel_waterfall.png` | Customer drop-off waterfall by variant |
| `5_4_significance_summary.png` | Statistical significance results table |
| `6_1_segment_conversion.png` | Conversion rate by customer segment |
| `6_2_device_analysis.png` | Open rate, conversion rate, revenue by device |
| `6_3_subject_lines.png` | Subject line type performance |
| `6_4_send_time.png` | Performance by send time bucket |
| `6_5_heatmap.png` | Conversion rate heatmap: segment × device |
| `7_1_revenue_lift.png` | Total revenue, revenue per send, per purchaser |
| `7_2_annual_forecast.png` | Cumulative revenue forecast + monthly uplift |
| `7_3_revenue_impact.png` | Revenue impact KPI dashboard |
| `8_1_executive_dashboard.png` | Single-page C-suite executive dashboard |

</details>

---

## ⚙️ How to Run

### Requirements

```bash
pip install pandas numpy scipy matplotlib seaborn
```

Or install from requirements:

```bash
pip install -r requirements.txt
```

### Run the Notebook

```bash
# Clone the repository
git clone https://github.com/anonopatience/email-ab-testing-analysis.git
cd email-ab-testing-analysis

# Launch Jupyter
jupyter notebook Email_Marketing_AB_Testing_Analysis.ipynb
```

### Directory Setup

The notebook auto-creates output directories on first run:

```
visuals/      ← all 17 charts saved here automatically
data/processed/ ← clean CSV and Power BI-ready CSV saved here
```

---

## 📦 Requirements

```
pandas>=1.5.0
numpy>=1.23.0
scipy>=1.9.0
matplotlib>=3.6.0
seaborn>=0.12.0
pathlib (built-in)
warnings (built-in)
```

---

## 💡 Skills Demonstrated

| Skill | Applied In |
|-------|-----------|
| **Data Cleaning & Transformation** | Handled 7 types of missing data; structural vs random missingness identification; funnel logic validation |
| **Exploratory Data Analysis** | Distribution analysis; randomisation validation; outlier treatment; revenue skew identification |
| **Funnel Analysis** | End-to-end funnel construction; drop-off quantification; stage-level conversion rates |
| **A/B Testing & Statistical Inference** | Two-Proportion Z-Test; Mann-Whitney U; Welch T-Test; confidence intervals; one-tailed hypothesis testing |
| **Customer Segmentation** | Segment × device performance matrix; heatmap analysis; priority tiering |
| **Revenue Impact Analysis** | Revenue per send; per-purchaser analysis; annualised projections; scenario modelling |
| **Data Visualisation** | 17 production charts; custom colour palette; executive dashboard; waterfall charts; heatmaps |
| **Business Communication** | Consulting-grade case study; executive summary; strategic recommendation framework |

---

## 📋 Strategic Recommendations Summary

### Immediate (0–2 Weeks)
- ✅ Roll out Variant B to 100% of email list
- ✅ Update all email templates to Variant B design
- ✅ Brief CRM and marketing teams on findings
- ✅ Set up weekly KPI monitoring dashboard for first 60 days

### Short-Term (2–8 Weeks)
- A/B test subject line types — Personalised Offer and Urgency + Discount show strongest results
- Launch segment-specific campaigns for VIP and Loyal customers
- Mobile-first email redesign — 62% of audience is mobile
- Shift send times toward Evening window (17:00–21:00)

### Experimentation Roadmap (2–6 Months)

```
Month 1–2:  Subject line personalisation test
Month 2–3:  ML-predicted optimal send time per user
Month 3–4:  Email content depth test (short vs long)
Month 4–5:  Discount incentive size test
Month 5–6:  Multivariate test combining all best elements
```

---

## 👤 About

**Patience Anono** — Data Analyst & Marketing Analytics Specialist

I help e-commerce brands and marketing teams transform raw data into revenue-driving decisions through rigorous analysis, statistical testing and clear business recommendations.

📧 hello@padataanalytics.com  
🌐 [padataanalytics.com](https://padataanalytics.com)  
💼 [LinkedIn](https://www.linkedin.com/in/patience-anono-22ab06176/)  
💻 [GitHub](https://github.com/anonopatience)

---

## 📄 License

This project is for portfolio and educational demonstration purposes.  
Dataset is synthetic, generated for analytical practice.

---

<div align="center">

*Built with Python · Analysed with statistical rigour · Documented for stakeholders*  
**PA Data Analytics — Data → Decisions → Growth**

</div>
