# Exploratory Data Analysis of Bank Customer Churn

Which customers of a bank close their accounts, and what do they have in common? This notebook explores 28,382 customers: their demographics, account details, balances and transactions. It tests eleven hypotheses about who churns, reporting how large each difference is and not only whether it is significant.

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/harsh-github007/Analysis_Churn/blob/main/Churn_analysis.ipynb)

## Key findings

- **18.5% of customers churned.**
- **No single attribute explains much of it.** Every categorical association has a Cramér's V of 0.06 or less. With 28,000 customers, a gap of one or two percentage points is "significant", so the notebook reports churn rates alongside p-values.
- **Recent activity doesn't protect against churn.** Customers inactive for more than 6 months churn *less* (13.6%) than those who transacted recently (20.6%). Churners look like active customers moving their money out, not dormant accounts.
- **Several expected patterns don't hold:**
  - Customers under 18 churn *less* (12.9%) than adults (19.4%).
  - Customers with dependents churn *more* (22.5%, against 17.4% with none).
  - Net worth category barely matters (17.9%–19.2%).
  - Branch size makes no difference.
- **Missing values carry information.** Customers with no recorded dependents churn at 21.4%, so that gap should be kept as its own category rather than filled with 0.
- **Balances and transactions are heavily skewed,** with outliers that repeat from month to month. Balance variables are strongly correlated with each other, credit and debit variables moderately so, and the two groups are nearly unrelated.

| Hypothesis | Churn rates | Cramér's V | Verdict |
| --- | --- | ---: | --- |
| Females churn less than males | 17.6% vs 19.2% | 0.02 | True, but small |
| Young customers churn more | Under 18: 12.9%; 18–59: 19.4% | 0.04 | Reversed |
| Lower net worth churns more | 19.1% / 17.9% / 19.2% | 0.02 | No meaningful effect |
| Customers with dependents churn less | None: 17.4%; 1–3: 22.5% | 0.05 | Reversed |
| Inactive for 6+ months churn more | 13.6% vs 20.6% | 0.06 | Reversed |
| Small cities churn more | 19.3% vs 17.8% | 0.02 | True, but small |
| Small branches churn more | 18.5% vs 19.9% | 0.00 | No difference |

## What's in the notebook

1. **Variable identification and typecasting.** IDs, branch, city and net worth category become categories; the last transaction date is split into day, week, month, weekday and days since the last transaction.
2. **Univariate analysis.** Distributions and summary statistics for numerical variables, value counts for categorical ones, missing values, and outliers using Tukey's fences (1.5 × IQR beyond the quartiles).
3. **Bivariate analysis, numerical.** Pearson, Kendall and Spearman correlation heatmaps, plus log-scale scatter plots of the balance and transaction variables.
4. **Bivariate analysis, categorical.** A chi-square test for each hypothesis above, with churn rate per group and Cramér's V for the strength of the association.
5. **Missing values against churn,** for gender, dependents and occupation.
6. **Conclusion.**

## Data

`churn_analysis.csv` has 28,382 rows, one per customer, with 21 columns:
- **Customer details:** age, gender, dependents, occupation, city, net worth category.
- **Account details:** vintage in days, branch.
- **Balances:** current, previous month end, and average for the previous two quarters.
- **Transactions:** current and previous month credits and debits.
- **Last transaction date:** during 2019.
- **`churn` flag.**

Some fields have missing values: last transaction date (3,223), dependents (2,463), city (803), gender (525) and occupation (80).

## Running it

Click the Colab badge above; the notebook loads the data from this repository by itself. To run it locally:

```bash
pip install -r requirements.txt
jupyter notebook Churn_analysis.ipynb
```

It runs top to bottom on current versions of pandas (3.x), seaborn (0.13) and SciPy.
