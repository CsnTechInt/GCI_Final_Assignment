# Telecom Churn Prediction - GCI Final Project | UTokyo Matsuo Lab

> **Business Impact: $183.8M Net Annual Saving | $184M Revenue Saved**

**Author:** Brastus George Mabeva  
**Account:** Csn-Tech2026  
**ID:** 163576  
**Course:** GCI World 2026 September - CS 151: Final Assignment

### 🔗 Live Demo / Report
- PDF Report: `/reports/Csn-Tech2026.pdf`
- Notebook: `Csn-Tech2026.ipynb`

### 📌 Problem Statement
A telecom company is losing customers (churn). With 100,000 customers in the dataset, we need to predict who will churn and propose data-driven actions to retain them.

**Dataset:**
- `Client.csv` - Demographics (age, income, numbcars, dwllsize, ethnic)
- `Record.csv` - Behavioral (eqpdays, change_mou, change_rev, drop_vce_Mean, custcare_Mean, etc.)
- Target: `churn` (0/1) - 49.6% churn rate (balanced)

### 🔍 EDA & Key Insights

**Hypotheses Validated:**
1. **H1 Equipment age:** High `eqpdays` -> churn
2. **H2 Customer service:** High `custcare_Mean` -> frustration -> churn
3. **H3 Network quality:** High `drop_vce_Mean` & `drop_dat_Mean` -> churn
4. **H4 Disengagement:** Falling `change_mou` (< -20%) signals customer testing competitor

**Top Features (HistGradientBoosting):**
1. `eqpdays` ~0.059 - #1 driver
2. `change_mou` ~0.045
3. `mou_Mean` ~0.040
4. `avgrev`, `change_rev`, `adjrev`

Insight: Behavior > Demographics. Root cause is product (old device) and service failure, not price.

### 🛠️ Data Preparation
- **Missing Values:** 49% missing in demographics (skipped survey), ~1% in behavioral (new customers)
- **Strategy:** Median imputation (robust to outliers)
- **Split:** 80/20 stratified, random_state=42
- **No data leakage verified**

### 🤖 Modeling

| Model | AUC | Improvement |
|-------|-----|-------------|
| RandomForest (n_estimators=100) | 0.660 | baseline |
| **HistGradientBoosting (max_iter=200, max_depth=6)** | **0.674** | **+2.1% relative** |

- Metric: ROC-AUC (suitable for balanced classification)
- No scaling needed (tree-based models)
- Better than random (0.5)

### 💰 Business Proposal & ROI

**The math from notebook (`roi_calculation_output` cell):**
- Total at risk: 49,562 customers × $1,031.92 MOU = $51.1M/month at risk
- If we save 30%: 14,869 customers saved
- Saved Revenue: $184.1M / Year (14,869 × $1,031.92 × 12)
- Campaign Cost: $297k (14,869 × $20 SMS/support)
- **NET ROI: $183.8M / year**
- ROI: 61,700% | Payback < 1 month
- Assumption: 30% save rate (benchmark 25-40%)

#### Proposal 1: Trade-in Program (eqpdays > 300)
- **Target:** ~19,800 customers (~40% of churners)
- **Offer:** $200 discount + free upgrade + 12-month extension
- **Expected:** -40% cut in eqpdays-driven churn
- **Channel:** Automated SMS & app push when eqpdays crosses 300
- **Cost:** $3.96M one-time
- **Timeline:** Launch in 2 weeks via CRM

#### Proposal 2: Fix Network & Service Issues
- **2a Network fix:** Free signal booster when `drop_vce_Mean > 2%` or high `drop_dat_Mean`
- **2b VIP support:** Dedicated agent & proactive call when `custcare_Mean > 5 calls/month`
- **2c Loyalty discount:** 10% off next 3 months when `change_mou < -20%`
- **Early Warning:** Trigger intervention when change_mou drops >20% in one month

### 📁 Project Structure
