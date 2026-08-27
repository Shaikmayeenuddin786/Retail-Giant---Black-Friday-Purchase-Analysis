# **Retail Giant - Black Friday Purchase Analysis**

### Confidence Intervals & Central Limit Theorem (CLT) | Customer Segmentation | Top Business Insights & Recommendations

  <img width="678" height="452" alt="image" src="https://github.com/user-attachments/assets/ed38ab1f-f00a-471d-bc2e-165a5366cb9b" />

---


## Quick Overview

| Section | Details |
|---------|---------|
| **Business Problem** | Walmart needs to analyze customer purchase patterns during **Black Friday** to determine if spending habits differ by gender, age, and marital status. Without clear insights, marketing budgets are wasted and product placement doesn't match customer behavior. |
| **Objectives** | 1. Analyze purchase patterns by gender, age, and marital status<br>2. Apply **Confidence Intervals** to quantify spending differences<br>3. Demonstrate **Central Limit Theorem (CLT)** with sampling distributions<br>4. Provide data-driven marketing and product placement recommendations<br>5. Identify highest-value customer segments |
| **Technical Stack** | Python (Pandas, NumPy, Matplotlib, Seaborn, SciPy)<br>Jupyter Notebook<br>Markdown for documentation |
| **Project Features** | • Analyzed **550,068** real Black Friday transactions<br>• Built Confidence Intervals (90%, 95%, 99%) for key segments<br>• Demonstrated CLT with sample sizes 30, 100, 500<br>• Identified **men spend $703 more** than women per transaction<br>• Found **age 51-55** group spends the most ($9,535)<br>• Proved **marital status makes no difference** in spending |
| **Start-to-End Pipeline** | Data Loading → EDA → Gender Analysis → CLT Demonstration → Confidence Intervals → Age/Marital Analysis → Business Recommendations |

---

## The Big Picture

Walmart needed to understand customer purchase patterns during Black Friday to optimize marketing and product placement. Instead of guessing which segments spend more, we analyzed over **550,000 real transactions** using statistical methods like Confidence Intervals and the Central Limit Theorem.

This project proves that **men spend significantly more than women**, **older customers (46+) are the highest spenders**, and **marital status doesn't affect spending at all**. The repo shares the complete Python analysis so Walmart's marketing, merchandising, and leadership teams can make data-driven decisions.

---

## Business Problem

Walmart's Black Friday operation needed clear answers on:

- **Do men or women spend more** during Black Friday?
- **Does age affect** purchase amounts?
- **Does marital status** change spending behavior?
- How can Walmart use these insights for **better marketing and product placement**?
- Can we use **Confidence Intervals** and **CLT** to make reliable conclusions?

Without this analysis, Walmart might waste ad budget on the wrong segments, misplace products in stores, and miss revenue opportunities from high-value customers.

---

## Objectives

- **Analyze purchase patterns** – Compare spending by gender, age, and marital status
- **Apply Confidence Intervals** – Quantify differences with 90%, 95%, and 99% confidence
- **Demonstrate CLT** – Show how sampling distribution becomes normal with larger samples
- **Identify high-value segments** – Find customers who spend the most
- **Provide recommendations** – Give clear marketing and product placement advice
- **Make statistics practical** – Show how CLT and CI help real business decisions

---

## Technical Stack

**Python Libraries**:
- **Pandas & NumPy** – Data manipulation and analysis
- **Matplotlib & Seaborn** – Visualization and plotting
- **SciPy** – Statistical functions (standard error, t-distribution)

**Environment**:
- Jupyter Notebook
- Markdown for documentation

---

## Repository Structure

```
walmart-black-friday-analysis/
│
├── README.md                           # Project overview and documentation
├── WALMART_DATA.csv                    # Original dataset (550,068 records)
├── Walmart_BlackFriday_Analysis.ipynb  # Complete Jupyter Notebook
├── Walmart_BlackFriday_Report.pdf      # Final project report
├── visuals/                            # All plots and graphs
│   ├── purchase_distribution.png
│   ├── purchase_boxplot.png
│   ├── gender_boxplot.png
│   ├── gender_avg_comparison.png
│   ├── clt_female_n30.png
│   ├── clt_female_n100.png
│   ├── clt_female_n500.png
│   ├── clt_male_n30.png
│   ├── clt_male_n100.png
│   ├── clt_male_n500.png
│   ├── age_group_spending.png
│   └── confidence_intervals_comparison.png
└── output/                             # Analysis results
    ├── gender_stats.csv
    ├── age_stats.csv
    └── marital_stats.csv
```

---

## Column Profiling

| Column | Type | Description |
|--------|------|-------------|
| **User_ID** | Integer | Unique customer identifier |
| **Product_ID** | String | Unique product identifier |
| **Gender** | Categorical | F (Female) or M (Male) |
| **Age** | Categorical | 0-17, 18-25, 26-35, 36-45, 46-50, 51-55, 55+ |
| **Occupation** | Integer | Occupation code (0-20) |
| **City_Category** | Categorical | A, B, C (city tier) |
| **Stay_In_Current_City_Years** | Integer | 0, 1, 2, 3, 4+ |
| **Marital_Status** | Categorical | 0 (Unmarried) or 1 (Married) |
| **Product_Category** | Integer | Product category code (1-20) |
| **Purchase** | Integer | Transaction amount ($) |

---

## Analysis Steps

### 1. Data Loading & Initial Exploration

First, we loaded the dataset and performed initial checks to understand its structure and quality.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from scipy import stats

df = pd.read_csv("WALMART_DATA.csv")
print("Shape:", df.shape)  # (550068, 10)
df.head()
```

**Findings:**
- **Total records**: 550,068 transactions
- **Total features**: 10 columns
- **No missing values** – completely clean dataset
- **Data types**: Mix of integer, categorical, and object
- **Categories**: Gender (2), Age (7), City_Category (3), Marital_Status (2)

---

### 2. Data Types & Basic Information

```python
df.info()
```

**Findings:**
- All 550,068 rows have complete data – **no missing values to fix**
- Categorical columns converted to 'category' dtype:
  - Gender, Age, City_Category, Marital_Status
- Memory usage: 27.3+ MB

---

### 3. Statistical Summary

```python
print(df.describe())
```

**Findings:**

| Metric | Purchase Amount |
|--------|-----------------|
| **Mean** | $9,263.97 |
| **Median** | $8,047.00 |
| **Min** | $12.00 |
| **Max** | $23,961.00 |
| **25th Percentile** | $5,823.00 |
| **75th Percentile** | $12,054.00 |

**Key Observations:**
- Purchase amounts range from **$12 to $23,961**
- Half of all customers spend below $8,047, and half spend above
- Average purchase is **$9,264**
- Large range indicates diverse customer spending behavior

---

### 4. Categorical Variables Analysis

```python
print(df.describe(include=['category']))
```

**Findings:**

| Variable | Most Common | Frequency |
|----------|-------------|-----------|
| **Gender** | M (Male) | 414,259 (75%) |
| **Age** | 26-35 | 219,587 (40%) |
| **City_Category** | B | 231,173 (42%) |
| **Marital_Status** | 0 (Unmarried) | 324,731 (59%) |

**Key Insight:** Men make up about **75% of customers** (414k vs 136k women). The most common age group is **26-35**, and most customers live in **City Category B**. These are the segments Walmart should focus on first.

---

### 5. Outlier Detection

![Purchase Amount Boxplot](visuals/purchase_boxplot.png) *(Placeholder - Insert your purchase boxplot here)*

```python
plt.figure(figsize=(8,4))
sns.boxplot(x=df['Purchase'])
plt.title("Purchase Amount - Outlier Check")
plt.show()
```

**Findings:**
- **High-value outliers exist** above $20,000
- Small group of customers spends far more than the average
- This is **normal for Black Friday retail data** – some customers buy big-ticket items

**Mean vs Median:**
- **Mean Purchase**: $9,263.97
- **Median Purchase**: $8,047.00
- **Difference**: $1,216.97

The mean is higher than the median, confirming **right-skewed distribution** (a few very high purchases pull the average up).

---

### 6. Univariate Analysis - Purchase Distribution

![Purchase Distribution](visuals/purchase_distribution.png) *(Placeholder - Insert your histogram here)*

```python
plt.figure(figsize=(10,5))
sns.histplot(df['Purchase'], bins=50, kde=True)
plt.title("Distribution of Purchase Amount")
plt.xlabel("Purchase Amount")
plt.show()
```

**Observations:**
- Most transactions fall between **$5,000 and $15,000**
- Highest frequency around **$8,000-$9,000**
- **Long tail on the right** indicates fewer but very high-value purchases
- Normal pattern for Black Friday retail data

---

### 7. Gender Analysis

#### Gender-wise Purchase (Boxplot)

![Gender Boxplot](visuals/gender_boxplot.png) *(Placeholder - Insert your gender boxplot here)*

```python
plt.figure(figsize=(8,5))
sns.boxplot(x='Gender', y='Purchase', data=df)
plt.title("Purchase Amount by Gender")
plt.show()
```

**Findings:**
- **Men's median purchase is higher** than women's
- Men have **more extreme high-value transactions** (above $20,000)
- Women's spending stays within a **tighter range**

#### Average Spend per Transaction by Gender

```python
gender_avg = df.groupby('Gender')['Purchase'].mean()
print("Average Purchase by Gender:")
print(gender_avg)
```

**Results:**

| Gender | Average Purchase |
|--------|------------------|
| **Female** | $8,734.57 |
| **Male** | $9,437.53 |
| **Difference** | **$702.96** |

**Answer: Do women spend more than men?**
- **No, men spend more** per transaction than women
- Men spend **$703 more** on average
- This is a **significant difference**

---

### 8. Central Limit Theorem (CLT) Demonstration

We created a function to plot sampling distributions for different sample sizes to prove CLT in action.

```python
def plot_sampling_distribution(data, sample_size, num_samples=500, title=""):
    means = []
    for _ in range(num_samples):
        sample = np.random.choice(data, size=sample_size, replace=True)
        means.append(sample.mean())
    
    plt.figure(figsize=(10,4))
    plt.hist(means, bins=30, edgecolor='black')
    plt.title(f"Sampling Distribution of Mean - {title} (n={sample_size})")
    plt.xlabel("Sample Mean")
    plt.ylabel("Frequency")
    plt.show()
    print(f"Mean of sample means: {np.mean(means):.2f}")
    print(f"Population mean: {data.mean():.2f}")
```

#### CLT for Female Customers

![CLT Female n=30](visuals/clt_female_n30.png) *(Placeholder)*
![CLT Female n=100](visuals/clt_female_n100.png) *(Placeholder)*
![CLT Female n=500](visuals/clt_female_n500.png) *(Placeholder)*

| Sample Size | Mean of Sample Means | Population Mean |
|-------------|---------------------|-----------------|
| **30** | $8,732.45 | $8,734.57 |
| **100** | $8,729.28 | $8,734.57 |
| **500** | $8,732.41 | $8,734.57 |

**Observation:** As sample size increases from 30 to 500, the distribution of sample means becomes **tighter** and more centered around the true population mean ($8,734.57). This confirms the **Central Limit Theorem in action** – larger samples give more reliable estimates.

#### CLT for Male Customers

![CLT Male n=30](visuals/clt_male_n30.png) *(Placeholder)*
![CLT Male n=100](visuals/clt_male_n100.png) *(Placeholder)*
![CLT Male n=500](visuals/clt_male_n500.png) *(Placeholder)*

| Sample Size | Mean of Sample Means | Population Mean |
|-------------|---------------------|-----------------|
| **30** | $9,311.68 | $9,437.53 |
| **100** | $9,443.51 | $9,437.53 |
| **500** | $9,453.46 | $9,437.53 |

**Observation:** Same pattern holds for men. At n=30, the spread is wider. At n=500, the distribution is much narrower and closer to the population mean ($9,437.53). **Bigger samples = less guesswork.**

---

### 9. Confidence Intervals

#### Confidence Interval Function

```python
def confidence_interval(data, confidence=0.95):
    n = len(data)
    mean = np.mean(data)
    se = stats.sem(data)  # standard error
    margin = se * stats.t.ppf((1 + confidence) / 2, n - 1)
    return (mean - margin, mean + margin)
```

#### 95% Confidence Intervals for Gender

```python
ci_female = confidence_interval(female_purchase, confidence=0.95)
ci_male = confidence_interval(male_purchase, confidence=0.95)

print("95% CI - Female:", ci_female)
print("95% CI - Male:", ci_male)
```

**Results:**

| Segment | 95% Confidence Interval |
|---------|------------------------|
| **Female** | ($8,709.21, $8,759.92) |
| **Male** | ($9,422.02, $9,453.03) |

**Checking Overlap:**
- Intervals **DO NOT overlap**
- Male CI is completely above female CI
- **Significant difference confirmed** – not random chance

#### Different Confidence Levels

| Confidence Level | Female CI | Width | Male CI | Width |
|------------------|-----------|-------|---------|-------|
| **90%** | ($8,719.29, $8,755.84) | $42.56 | ($9,424.51, $9,450.54) | $26.03 |
| **95%** | ($8,709.21, $8,759.92) | $50.71 | ($9,422.02, $9,453.03) | $31.01 |
| **99%** | ($8,701.24, $8,767.89) | $66.64 | ($9,417.15, $9,457.91) | $40.76 |

**Observation:** Higher confidence levels create **wider intervals**:
- **90% CI**: Narrowest (less certainty, more precision)
- **99% CI**: Widest (more certainty, less precision)
- **95% CI**: Good balance for business decisions

---

### 10. Marital Status Analysis

```python
married_purchase = df[df['Marital_Status'] == 1]['Purchase'].values
unmarried_purchase = df[df['Marital_Status'] == 0]['Purchase'].values

print("Married avg spend:", np.mean(married_purchase))
print("Unmarried avg spend:", np.mean(unmarried_purchase))

ci_married = confidence_interval(married_purchase)
ci_unmarried = confidence_interval(unmarried_purchase)

print("95% CI - Married:", ci_married)
print("95% CI - Unmarried:", ci_unmarried)
```

**Results:**

| Marital Status | Average Spend | 95% Confidence Interval |
|----------------|---------------|------------------------|
| **Married** | $9,261.17 | ($9,240.47, $9,281.89) |
| **Unmarried** | $9,265.91 | ($9,248.62, $9,283.20) |

**Key Finding:** Intervals **OVERLAP heavily** – no significant difference between married and unmarried customers. **Marital status does NOT affect spending.**

---

### 11. Age Group Analysis

```python
age_order = ['0-17', '18-25', '26-35', '36-45', '46-50', '51-55', '55+']
age_avg = df.groupby('Age')['Purchase'].mean().reindex(age_order)
print("Average Purchase by Age Group:")
print(age_avg)
```

![Age Group Spending](visuals/age_group_spending.png) *(Placeholder - Insert your age group bar chart here)*

**Results:**

| Age Group | Average Purchase |
|-----------|------------------|
| **0-17** | $8,933.46 |
| **18-25** | $9,169.66 |
| **26-35** | $9,252.69 |
| **36-45** | $9,331.35 |
| **46-50** | $9,208.63 |
| **51-55** | **$9,534.81** (Highest) |
| **55+** | $9,336.29 |

**Key Findings:**
- **Age 51-55** spends the most at **$9,535** per transaction
- **55+** follows at **$9,336** and **36-45** at **$9,331**
- **Youngest group (0-17)** spends the least at **$8,933**
- Difference between highest and lowest: ~$600

---

## Top Key Insights (Quick Summary)

1. **Men spend more than women.**
   - Average purchase: Men = **$9,438**, Women = **$8,735**
   - Difference: **$703** more per transaction

2. **Confidence intervals for gender do not overlap.**
   - Men's CI ($9,422-$9,453) is completely above women's CI ($8,709-$8,760)
   - The difference is **significant**, not random chance

3. **Marital status does not affect spending.**
   - Married: $9,261 | Unmarried: $9,266
   - Almost identical – intervals overlap heavily
   - **No real difference** between married and unmarried customers

4. **Age matters – older customers spend more.**
   - **51-55** age group spends the most ($9,535)
   - Followed by **55+** ($9,336) and **36-45** ($9,331)
   - **0-17** spends the least ($8,933)

5. **Youngest group spends the least.**
   - Age 0-17 spend only $8,933, about **$600 less** than the highest spending group

6. **Purchase amount distribution is right-skewed.**
   - Most customers make smaller purchases
   - A few make very large purchases (visible from boxplot and histogram)

7. **No missing values** – data quality is good.

8. **Outliers exist** in purchase amounts, which is normal for retail data.

9. **Sampling distribution follows normal distribution.**
   - CLT holds true, which is why confidence intervals are reliable
   - Even when original data is not normally distributed

10. **Higher confidence levels create wider intervals.**
    - 90% CI: Narrower (less certainty)
    - 99% CI: Wider (more certainty)
    - **95% CI is best balance** for business decisions

---

## Top Recommendations

### Gender

- **Men spend $703 more** than women per transaction. Increase ad money on men's products during Black Friday.
- **Show men**: Electronics, tools, automotive products
- **Show women**: Fashion, home goods, beauty products
- **Keep messaging separate** – target men with high-value items
- **Place expensive men's items** at eye level and store entrances
- **For women**, focus on discounts and deals

### Marital Status

- **Stop separate "married vs single" campaigns.**
- Data shows **no spending difference** – it's a waste of marketing budget

### Age

- **Start a premium loyalty program** for customers aged **46+**
- They spend the most and will appreciate early access and extra rewards
- **Place expensive products** where older customers shop – home electronics, premium appliances
- **For customers aged 0-17**, introduce entry-level products – don't try to sell them high prices
- **Test one store** with premium sections for older age groups. Monitor sales. Roll out if it works.

---

## Final Conclusion

**Men and customers aged 46+ are Walmart's highest spenders on Black Friday.** Married vs unmarried makes no difference – so ignore it. Shift ad budget to men's products, start a loyalty program for older customers, and keep women's offers discount-focused. Simple changes, real impact.

---

## How to Use This Repository

1. **Clone the repo:**
   ```bash
   git clone https://github.com/your-username/walmart-black-friday-analysis.git
   ```

2. **Install required libraries:**
   ```bash
   pip install pandas numpy matplotlib seaborn scipy
   ```

3. **Open the Jupyter Notebook:**
   ```bash
   jupyter notebook Walmart_BlackFriday_Analysis.ipynb
   ```

4. **Run the complete analysis** – All code is in the notebook

5. **Reproduce the results** – Follow the code in sequence

---

## How to Run

1. Clone the repository
2. Ensure `WALMART_DATA.csv` is in the root folder
3. Open `Walmart_BlackFriday_Analysis.ipynb` in Jupyter Notebook
4. Run all cells in sequence
5. Review the output and visualizations

---

## Project Summary Table

| Section | Key Finding | Recommendation |
|---------|-------------|----------------|
| **Gender** | Men spend $703 more than women | Target men with electronics, women with discounts |
| **Marital Status** | No spending difference | Stop separate campaigns |
| **Age** | 51-55 spend most ($9,535) | Loyalty program for 46+ customers |
| **CLT** | Normal with larger samples | Reliable confidence intervals |
| **Confidence Intervals** | Gender intervals don't overlap | Confirms significant difference |
| **Outliers** | High-value purchases exist | Normal for Black Friday |
| **Data Quality** | 550,068 records, no missing | Ready for analysis |

---

**Final Message for Walmart:** Men spend $703 more than women. Customers aged 46+ are your highest spenders. Marital status doesn't matter – ignore it. Use these insights to target marketing, place products strategically, and boost Black Friday revenue! 

---

# 👤 **Author**

### **Shaik Mayeenuddin**

#### Business Analyst | Data Analytics & AI/ML | Optimizing Processes to Drive Revenue & Retention

🔗https://www.linkedin.com/in/shaikmayeenuddin


