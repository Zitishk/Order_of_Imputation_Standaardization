# Order_of_Imputation_Standardization
To explore what order to follow when data processing - Impute missing values then standardize or standardize then impute, both mathematically and visually.

# Experiments in Imputation and Standardization

This project explores different approaches to handling missing data and standardization in statistical analysis, comparing the effects of different processing orders and their impact on statistical properties and regression results.

## 1. Mathematical Foundation

### 1.1 Basic Standardization
For any random variable Y following distribution D(μ, σ), standardization transforms it to a distribution with mean 0 and standard deviation 1:

```
Y ~ D(μ, σ)
Y' = (Y - μ)/σ
=> μ' = 0, σ' = 1
```

### 1.2 Sample Standardization
When working with a sample y₁ₓₙ, we use sample statistics:
```
y ~ D(ȳ, s)
y' = (y - ȳ)/s
=> ȳ' = 0, s' = 1
```

### 1.3 Missing Data Handling Options

#### Option 1: Standardize-then-Fill (StF)
When we standardize first and then fill missing values with zeros:
```
y'' = y' + [0,0,0...]₁ₓₘ
=> ȳ'' = 0
s'' = s' √(n/(n+m)) = √(n/(n+m))
```

Note: A Secondary standardization gives:
```
y''' = y''/s'' = y'' √((n+m)/n)
=> (y-ȳ)/s √((n+m)/n)
```

#### Option 2: Fill-then-Standardize (FtS)
When we fill missing values first and then standardize:
```
y'' = y + [0,0,0...]₁ₓₘ
ȳ'' = ȳ
s'' = s √(n/(n+m))
```

This leads to the final formula:
```
y''' = (y''-ȳ)/s'' = (y-ȳ)/s √((n+m)/n)
```

The key mathematical insight is that both approaches only differ by a inflation factor, k= √((n+m)/n) on the original standardized values, in option 1, the orignal values are unaffected but the overall standard deviation drops by k whereas in option 2,the values are k times the orignal.

## 2. Visualization Analysis

The code implements several visualization techniques to compare the different approaches:

### 2.1 Distribution Comparisons
- The violin plots show that while both StF and FtS preserve basic distributional shape, FtS exhibits longer tails
- KDE plots reveal that StF is more acute around zero but drops off faster in the tails
- The filling approach affects the density estimation, with StF showing more concentration around zero

### 2.2 Residual Analysis
- Scatter plots of residuals show different patterns:
  - StF residuals are exactly zero for original points
  - FtS residuals show a consistent inflation pattern proportional to V*(1-√0.7)
- This visualization helps understand how each method affects the original data structure

## 3. Regression Analysis Results

### 3.1 Method Comparison

We test this for a range of R-Square values and find that - 
The regression analysis reveals that:
- StF and FtS maintain same R² values as predicted
- Coefficient ratios between StF and FtS match theoretical predictions
- **Interestingly Simply dropping missing values can sometimes outperform imputation methods**
- However note that the Rsq for dropping the missing values are sometimes higher than the full datasets
- This shows that dropping missing values can cause overfitting; inline with theoretical expectations

### 3.2 Impact of Missing Data Percentage
To explore when simply dropping the missing data performs better we do another round of analysis.
The code tests different missing data percentages (20%, 40%, 60%, 80%, 95%) with different Rsq and their impact on:
- Relative performance of StF method
- Relative performance of Dropping method

Key findings:
   - Results are mixed
   - StF performs better with moderate missing data (around 60%) and low Rsquares
   - Performance degradation is non-linear with missing data percentage

## 4. Conclusions and Future Work

### Key Findings
1. The mathematical equivalence of StF and FtS in terms of final formula but different practical implications
2. The impact of missing data percentage is non-linear and depends on the underlying R² of the relationship
3. The choice between StF and dropping may depend on specific use case requirements

### Future Research Directions
1. Extend analysis to non-normal distributions
3. Study the effect of correlation structures in multivariate settings
4. Explore more sophisticated imputation methods (MICE, KNN, etc.)
5. Analyze impact on non-linear relationships
6. Investigate theoretical bounds on estimation error

### Recommendations
1. For high R² relationships, traditional methods like dropping missing values may be sufficient
2. For low R² relationships with moderate missing data, StF might be preferred
3. Consider the specific requirements of your analysis (e.g., interpretation needs, computational efficiency)
4. Always validate results with sensitivity analysis across different missing data percentages

This project provides a foundation for understanding the nuances of missing data handling and standardization order, while highlighting important considerations for practitioners in data analysis and statistical modeling.
