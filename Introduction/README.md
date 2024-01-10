# Introduction 

> Data mining is the process of discovering patterns, correlations, and insights from large datasets using statistical and computational techniques. It involves applying various algorithms to analyze and extract useful information from structured and unstructured data sources, such as databases, data warehouses, and the internet. The goal of data mining is to extract knowledge from data and use it to make informed business decisions, identify new opportunities, and improve organizational efficiency. Some common techniques used in data mining include clustering, classification, regression analysis, association rule mining, and anomaly detection.

### Why Mine Data

1. Big datasets are increasingly collected routinely
2. The data is being warehoused
3. Computing power is cheap
4. Competition between companies is strong
5. Integrated software is now available

### Two basic styles of DM

1. Hypothesis testing (also known as the top-down approach) involves confirming or refuting preconceived ideas. 
    1. Formal 
    2. ‘Statistician’
2. Knowledge discovery (aka bottom-up approach) is essentially letting a smart computer program loose on a data set and letting it get back information that we can then apply to make money and win customers. [informal - trying to find any sort of information] 
    1. Undirected:  asks the computer to identify patterns in the data that may be significant. It provides supplementary knowledge. —  recognize relationships in the data — No response variable 
    2. Directed: explain the value of some particular variable in terms of the other variables. It uses classification, regression and/or prediction —explain those relationships once they have been found — have a response variable 
3. Mixture 
    1. false discourage rate — trying to find the knowledge by hypothesis testing 

### Standard Process for Data Mining

1. Business Understanding:  Objectives must be stated clearly, e.g., what improvements are desired and how will they be measured? → **specific, clearly stated and can be measured** 
2. Data Understanding: Variables with units, limitations, NAs, etc. → **missing value** 
3. Data Preparation Selecting subsets, constructing new variables, transforming variables, etc. → **cleaning and preparing data for analysis** 
4. Modelling: Iteratively perform the classification or regression, usually for prediction. Train & test the model, e.g., using CV.
5. Evaluation: The model must be evaluated in terms of its results, and interpret their significance. Do residual plots etc. → check we don’t underfit or overfit 
6. Deployment: The model (a good model that’s not over or under fitted) can be used to
    1. recommend actions based on simply viewing the model and its results, e.g., look at the clusters identified, the rules that define the model,
    2. apply the model to different data sets.

### Some practical tips

- A benchmark(simple statistical method) method is invaluable.
    - It is useful to fit a simple model before attempting a more complex model. It is useful to fit a simple model before attempting a more complex model, for example, fitting a logistic regression or LM before fitting a tree or neural network. Why? This way, the results of the more complex analysis can be compared with the simpler model. One can see the added value. If the complex model has little benefit, then the simpler model may be preferable. The simpler model may be preferable. Customers also like to get an early promise of results, and data miners are working on an improved model.
    - Compare the improvement
- Know your tools, especially their strengths and weaknesses.
    - Also, every statistical method has its set of assumptions which should be checked if used.
    - e.g. residuals after fitting model
- Communication is very important & being able to work in a team.
- It is essential to understand the client’s problem. Often the client has unrealistic expectations and expects some magic. They often have no idea of exactly what questions their data is capable of answering.
- If it can be done in R then do it in R. It may be a little slower, but the added functions such as plotting make up for it.
- Some statistical methods are estimated iteratively, e.g., logistic regression. To speed up convergence, try fitting the model to a small random sample and then use it for initial values when fitted to all the data.

### Over-fitting

Fitting a over complicated statistical model to the dataset that cannot be generalised into population 

### **Two types of data mining**

1. Text mining: the process of mining unstructured text for pattern recognition and context.
2. Survival mining: a use of predictive analytics to identify when something is likely to occur in a defined time span.

## Generic topic

### Data types

1. Structured data: [with 2 basic dimensions] 
    1. the number of rows (records/subjects/cases) — observation 
    2. the number of columns (variables) — variables 
2. Unstructured data 
    1. e.g. quality, customer service and warranty data

### 3 V of big data

- Volume → big
- Velocity → Sometimes the data is a continuous stream
- Variety → often due to various sources and is unstructured
- Veracity: trustworthiness, e.g., representative?, of imprecise and uncertain data.

## Order notation — big-oh [measuring the computational cost]

The computational cost of an algorithm is an important consideration. Often one is restricted to solutions that are (computationally) cheap, or equivalently, not expensive.

1. **Exercise:** 
    
    The cost of inverting an n × n matrix is, theoretically, O($n^3$). If it takes 20 minutes to compute the inverse of an order-1000 matrix, how long would it take (theoretically) to invert an order-4000 matrix?
    
    ```r
    > n=10
    > system.time(solve(matrix(runif(n*n),n,n))) 
      
    user  system elapsed   
    0.000   0.003   0.040
    
    > n=500
    > system.time(solve(matrix(runif(n*n),n,n)))   
    user  system elapsed   
    0.123   0.003   0.128
    ```
    
    - `solve` is the inversion of a matrix, which in general = O($n^3$)
    
    → Time = Constant * $n^3$
    
    → 20 minutes = Constant * $1000^3$
    
    → Constant = $20 / 1000^3$
    
    → Time (for inverting an order-4000 matrix) =  $20 / 1000^3$ * $4000^3$
    
    → 1280 minutes 
    
    → 21.3 Hrs (approximately)

## Dirty data

### Missing data

- **Structural 0**: values that logically could not gave a value [not random]
    - e.g. for a poisonous plants, the number of insect on it it’s always 0
    - e.g. number of pregnancy of males
- **CCA - complete case analysis:**
    - → use only the cases (observations) that have complete records in the analysis
    - by using `complete.cases()` and `na.omit()`
    - But this can be wasteful — as for the object has only one of the variables is empty, the whole object (row) will be deleted ← **disadvantages of complete case analysis**
    - In DM, the chief disadvantage of complete-case analysis is practical: even a smattering of missing values in high dimensional data can cause a huge reduction in data
    - Will be more problematic if the dimension of dataset is high —  a large amount of data will be lost
    - Complete-case analysis can be okay if
        - the proportion of deleted observations is small relative to the entire data set  → **proportion of missingness**
        - if the mechanism that leads to the missing data is independent of the variables in question—this is known as missing at random (MAR) or missing completely at random (MCAR). → **caution of missingness**
    - Exercise:
        
        > Suppose there are n cases and d variables, and that data values are missing at random with probability p. Using the values
        n = 1000, d = 10 and for p = 0.1 and 0.3, calculate the expected number of complete cases.
        > 
        
        Answer: 1000 × (1 − 0.1)^10 ≈ 349, 1000 × (1 − 0.3)^10 ≈ 28. 
        
        ```r
        > x = matrix(runif(n*d),n,d)
        > x = ifelse(runif(n*d)<p,NA,x) # MCAR
        > nrow(na.omit(matrix(x,n,d)))
        ```
        
- Available-Case analysis
    - The full data set is retained but subsequent analyses use those observations that have no missing values for the subset of variables required for that analysis. For example, lm(y ∼ x2 + x3, data = ldata) will only delete rows of ldata if any of the 3 variables is missing. The data frame might have 100 other variables containing lots of missing values
    - But only applicable for MCAR otherwise the regression will be very biased
    - `na.fail`
    - `na.exclude`
    - `nz.pass`
- Imputation:
    - Fill the missing values with some reasonable value, and then run the analysis on the full (filled-in) data.
    - The simplest type of imputation is to replace missing values by the mean (mode for categorical variables) of the complete cases. This is called mean imputation.
    - The method can be refined by using the mean within homogeneous groups of the data. Hot-deck imputation is where a missing value is imputed by substituting a value from a similar but complete record in the same
    data set.
    - Regression imputation is where a missing value is imputed by the predicted value of a regression on the completely recorded
    data.
    - Multiple imputation fills in the missing values m > 1 times, where the imputed values are generated each time from a distribution that may be different for each missing value; this creates m different data sets, which are analyzed separately, and then the m results are combined to estimate model parameters, standard errors and confidence intervals. A recent article on MI is Murray (2018).
    - The missing values of categorical variables could be treated as a separate category. For example, type of residence might be: own home, rents home, live with parents, mobile home, and unknown. This method would be preferable if the missingness is itself a predictor of the target.
    - Mean imputation — replacing the missing value with mean
        
        ```r
        > x = 1:99
        > x [ 55 ] = NA
        > x[77] = NA
        > x[is.na(x)]
        [1] NA NA
        > x[is.na(x)]<-mean(x,na.rm = TRUE)
        > x
         [1]  1.0000  2.0000  3.0000  4.0000  5.0000  6.0000  7.0000  8.0000  9.0000 10.0000 11.0000 12.0000
        [13] 13.0000 14.0000 15.0000 16.0000 17.0000 18.0000 19.0000 20.0000 21.0000 22.0000 23.0000 24.0000
        [25] 25.0000 26.0000 27.0000 28.0000 29.0000 30.0000 31.0000 32.0000 33.0000 34.0000 35.0000 36.0000
        [37] 37.0000 38.0000 39.0000 40.0000 41.0000 42.0000 43.0000 44.0000 45.0000 46.0000 47.0000 48.0000
        [49] 49.0000 50.0000 51.0000 52.0000 53.0000 54.0000 49.6701 56.0000 57.0000 58.0000 59.0000 60.0000
        [61] 61.0000 62.0000 63.0000 64.0000 65.0000 66.0000 67.0000 68.0000 69.0000 70.0000 71.0000 72.0000
        [73] 73.0000 74.0000 75.0000 76.0000 49.6701 78.0000 79.0000 80.0000 81.0000 82.0000 83.0000 84.0000
        [85] 85.0000 86.0000 87.0000 88.0000 89.0000 90.0000 91.0000 92.0000 93.0000 94.0000 95.0000 96.0000
        [97] 97.0000 98.0000 99.0000
        ```
        
- EM algorithm:
    - This has expectation and maximization steps, and usually runs slowly. The key concept is that of unobserved latent variables. It was proposed by Dempster et al. (1977).

### Outliers

**[ Can be difficult to detect when the dimension (number of variables) goes up]**

### Data cleaning

- time consuming
- Range of data: e.g. range of age must be plausible → **range checking**
- number of pregnancy = 5  v.s. number of children = 5 → contracdiction → **logical check**
- **domain expert**
- **data collect actively or passively**

### Missing value — Technical definition
1. Missing Completely At Random
   - Not so realistic and easy to handle
    - The probability of an observation is missing is independent of the variable and observation
    - The probability of being missing given all the x variables as a function of phi, does not depend on x variables, equal to the probability of being missing
    - **There is no relationship between whether a data point is missing and any values in the data set, missing or observed.**
2. Missing At Random
   - More realistic and harder to handle
    - The probability of being missing depend only on the observed data
    - **the propensity for a data point to be missing is not related to the missing data, but it is related to some of the observed data.**
    - inference based on likelihood is valid: missingness is independent
    - The key assumption of MAR is that **the missingness of data can be predicted by the observed variables in the dataset**, but not by the missing values themselves. — completely untestable, that cannot be proved by data. This is the external assumption
3. Missing Not At Random
   - Most realistic and hardest to handle
    - if the unavailability of the data depends on both observed and unobserved data such as its value itself.
    - 
### **Non-ignorable missingness**

- missing means that **the missing data mechanism is related to the missing values**. For example, it commonly occurs when
people do not want to reveal something very personal or unpopular, such as those on higher incomes being less likely to reveal them on a survey than are those on a lower income. Then the missing data mechanism for income is non-ignorable; whether income is missing or observed is related to its value.
- Complete case analysis can give highly biased results for NI missing data
- will create bias - as missingness relate to the missing data itself

### Sampling

e.g. what causes rare diseases?? You might have an n*p dataset. Given a very large dataset, this is not a efficient dataset as not many people will have the rare disease. Solution: 

**case-control sampling** 

> selecting study participants based on whether they have a certain condition or outcome of interest (the "cases") or whether they do not have the condition or outcome (the "controls"). In a case-control study, researchers typically start by identifying a group of individuals who have the condition or outcome being studied (cases) and a group of individuals who do not have the condition or outcome (controls). The cases and controls are then compared in terms of their exposure to potential risk factors or other variables of interest. — For e.g. a 25 male has the rare disease(case), and then we search in the dataset for all the 25-year-old males (control)
> 

### Confidentiality and Encryption

Data should never identify any individuals 

### Multiple testing

<aside>
💡 When multiple tests are conducted on a dataset, the probability of finding a significant result by chance alone increases.

</aside>

**Why?** 

1. When conducting multiple tests on a dataset, there is a higher probability of finding a significant result by chance alone, even if all of the tests are based on null hypotheses that are true. This is because, by chance alone, some of the tests are likely to yield p-values that are smaller than the specified significance level.
2. To understand why this occurs, consider the case where a set of 20 independent statistical tests are conducted, each with a significance level of 0.05. If all of the null hypotheses are true, then the probability of obtaining a significant result by chance alone in any one test is 0.05. However, the probability of obtaining at least one significant result by chance alone across all 20 tests is: **1 - (1 - 0.05)^20 = 0.6415**
3. This means that there is a **64.15% chance of finding at least one significant result by chance alone, even if all of the tests are based on null hypotheses that are true.** As the number of tests increases, this probability also increases, making it more likely that at least one significant result will be found by chance alone.
4. Therefore, to control the overall Type I error rate when conducting multiple tests, it is necessary to adjust the significance level of each individual test to account for the increased probability of false positives.

**Bonferroni inequity and correction**  

> The Bonferroni correction is a statistical adjustment made to control for the likelihood of making false positive errors (Type I errors) when conducting multiple statistical tests on a single dataset. When multiple tests are conducted on a dataset, the probability of finding a significant result by chance alone increases. The Bonferroni correction addresses this issue by adjusting the significance threshold for each individual test, to reduce the overall probability of making a Type I error across all tests.
> 
- Bonferroni's inequality is a mathematical property that arises from the Bonferroni correction, a method used to **adjust the significance level of multiple statistical tests to control the family-wise error rate.**
- The inequality states that when the significance level is adjusted using the Bonferroni correction, the resulting overall Type I error rate (the probability of making at least one false positive error across all tests) **is no greater than the specified significance level.** However, the inequality also states that the resulting overall Type I error rate is usually less than the specified significance level, due to the conservative nature of the Bonferroni correction.
- For example, if 10 independent statistical tests are conducted, each with a significance level of 0.05, the Bonferroni correction would adjust the significance level of each test to 0.05/10 = 0.005, in order to control the overall Type I error rate at 0.05. The Bonferroni inequality guarantees that **the overall Type I error rate is no greater than 0.05**, but in practice, it may be lower than 0.05 due to the conservative nature of the correction.
- However, the inequality also implies that the Bonferroni correction may be overly conservative in some situations, leading to a **loss of statistical power**. Therefore, alternative methods that are less conservative, such as the false discovery rate control or the Benjamini-Hochberg procedure, may be more appropriate in some cases.

![Untitled](https://github.com/Yura-Qu/Statistical-Data-Mining-/assets/143141778/da3791ee-1822-4525-8fe6-cfd272a60e84)


Example: To understand why this occurs, consider the case where a set of 20 independent statistical tests are conducted, each with a significance level of 0.05. If all of the null hypotheses are true, then the probability of obtaining a significant result by chance alone in any one test is 0.05. However, the probability of obtaining at least one significant result by chance alone across all 20 tests is: 

- **1 - (1 - 0.05)^20 = 0.6415**
- **64.15% chance of finding at least one significant result by chance alone, even if all of the tests are based on null hypotheses that are true.**
- Type 1 error:  When the null hypothesis is true but falsely rejects the null hypothesis
    - solution 1: choose a small p-value — but this will increase type 2 error
- Type 2 error: When the null hypothesis is false but falsely accepts the null hypothesis

**Benjamini-Hochberg method** 

- The Benjamini-Hochberg method is a multiple-testing correction technique that **controls the false discovery rate (FDR)** when conducting multiple statistical hypothesis tests simultaneously. It was proposed by Yoav Benjamini and Yosef Hochberg in 1995.
    - **The FDR is the expected proportion of false positives among all significant tests**, and the Benjamini-Hochberg method provides a way to control it while **minimizing the number of rejected hypotheses.** The method works by ranking the p-values of all tests in increasing order and comparing them to a series of thresholds that depend on the number of tests and the desired FDR level. The hypotheses with p-values below the threshold are declared significant, and those above it are not.
        1. Conduct multiple hypothesis tests, each with a corresponding p-value.
        2. **Rank** the p-values in ascending order.
        3. For each p-value, calculate the corresponding **Benjamini-Hochberg critical value (also known as the adjusted p-value)** using the formula: (i/m) * q
            
            where i is the rank of the p-value, m is the total number of tests conducted, and q is the desired FDR level (typically set to 0.05).
            
        4. Starting from the smallest p-value, compare each p-value to its corresponding Benjamini-Hochberg critical value. Stop when you reach the first p-value that is greater than its critical value.
        5. Reject all null hypotheses corresponding to p-values smaller than or equal to the p-value from step 4.
        
        By setting a critical value for each p-value based on its rank and the desired FDR level, the Benjamini-Hochberg method **controls the FDR while minimizing the number of rejected null hypotheses.** This is because the method provides a less stringent threshold for significance compared to the Bonferroni correction, which controls the family-wise error rate (FWER) instead of the FDR.
        
    - Example:
        
        Suppose you conduct 100 hypothesis tests, each with a corresponding p-value. You want to control the **false discovery rate at 0.05**, which means that no more than 5% of the rejected null hypotheses should be false positives. Here are the p-values and their ranks:
        
        | Rank | P-value |
        | --- | --- |
        | 1 | 0.001 |
        | 2 | 0.002 |
        | 3 | 0.003 |
        | ... | ... |
        | 98 | 0.98 |
        | 99 | 0.99 |
        | 100 | 0.995 |
        
        | Rank | P-value | Critical value |
        | --- | --- | --- |
        | 1 | 0.001 | 0.0005 |
        | 2 | 0.002 | 0.001 |
        | 3 | 0.003 | 0.0015 |
        | ... | ... | ... |
        | 98 | 0.98 | 0.049 |
        | 99 | 0.99 | 0.0495 |
        | 100 | 0.995 | 0.05 |
        
        To apply the Benjamini-Hochberg method, we first calculate the Benjamini-Hochberg critical value for each p-value using the formula: $**(i/m) * q$,**  where i is the rank of the p-value, m is the total number of tests conducted (in this case, 100), and q is the desired FDR level (0.05 in this example). The critical values for each rank are:
        
        Starting from the smallest p-value, we **compare each p-value to its corresponding critical value**. We stop when we reach the **first p-value that is greater than its critical value**. In this case, the critical value for rank 3 is 0.0015, which is greater than the p-value of 0.003. Therefore, **we reject all null hypotheses corresponding to p-values smaller than or equal to 0.003.**
        
- The Benjamini-Hochberg method is widely used in fields such as genomics, neuroscience, and finance, where researchers often conduct thousands or even millions of tests simultaneously. It is more powerful than the Bonferroni correction, which is another commonly used multiple-testing correction method because it is less conservative and thus can detect more true positives. However, it is important to note that the Benjamini-Hochberg method assumes that the tests are independent or positively correlated, and may not perform well if the tests are negatively correlated or have complex dependencies.

Example If H01, . . . , H05 have p-values 0.006, 0.918, 0.012, 0.601, 0.0756 respectively and we choose a FWER at level 5% then show that we
reject H01 and H03, while Bonferroni will reject H01 only.

Answer:

p(1) = 0.006 < 0.05/(5+1−1)=0.01,

p(2) = 0.012 < 0.05/(5 + 1 − 2) = 0.0125,

p(3) = 0.601 > 0.05/(5 + 1 − 3) = 0.167,

so L = 3. Hence we reject H01 and H03. 

For the Bonferroni method, only 0.006 < 0.05/5 = 0.01 so we only reject H01.

### Under-fitting and over-fitting

Under-fitting: too few parameters in the model, not flexible, overlooking important structure 

Over-fitting: too many parameters, can not be generalised to new sample  

Model selection: 

1. regression 

   ![Untitled (1)](https://github.com/Yura-Qu/Statistical-Data-Mining-/assets/143141778/770369d8-59c7-4777-af82-4405b4e0a947)

    
    linear model: a lot of **bias, but stable, little variance —** underfit ****
    
    quadratic model: residual — patternless horizontal band
    
    20 degree polynomial: little bias — small residuals, but will have a lot of variances — overfit  
    
    In machine learning, bias and variance are two types of errors that can occur in a model's predictions.
    
    Bias refers to the error that is introduced by approximating a real-life problem with a simplified model. In other words, it is the difference between the expected or target value and the value predicted by the model. A model with high bias will underfit the data, which means it will be too simple and will not be able to capture the complexity of the data. This can result in poor performance on both the training and test data.
    
    Variance, on the other hand, refers to the error that is introduced by the model being too complex and overfitting the data. In this case, the model is too sensitive to small fluctuations in the training data and does not generalize well to new, unseen data. A model with high variance will have good performance on the training data but poor performance on the test data.
    
    The goal of machine learning is to find the right balance between bias and variance to achieve good performance on both the training and test data. This can be done by adjusting the complexity of the model, increasing or decreasing the amount of regularization, or using techniques like cross-validation to evaluate the model's performance on different subsets of the data.
    
3. classification: decision boundary 
    
    
   ![Untitled (2)](https://github.com/Yura-Qu/Statistical-Data-Mining-/assets/143141778/9a3cc9c7-6bbb-4cda-8d74-b9b29a080d28)

    
    Underfit 
    
   ![Untitled (3)](https://github.com/Yura-Qu/Statistical-Data-Mining-/assets/143141778/ec948c2e-e46b-42eb-ad17-794ae5cca906)

    
    Overfit: Classification boundaries uses too many parameters  
    
4. Summary: Complexity and error rates  — minimizes the error on unseen data.
    
    - As the number of parameters increases, the complexity in the training data model increases, and the error rate of the training dataset is low
    - For future data: unseen data — as the model complexity increases, the error rate will decrease first but after a certain point, the error rate will increase
    - error: residual sum of square
    
    - Size of the tree: number of leaves — complexity
    - error: missclasification rate
    - 1 standard error rule
    

**1 SE rule:** 

- The one SE rule is a pruning method used in decision tree algorithms to determine the optimal size of the tree. It is based on the idea that **smaller trees are generally preferred over larger trees** because they are less complex and less prone to overfitting.
- To apply the one SE rule, the first step is to construct a sequence of trees of different sizes, by using a range of values for a pruning parameter that controls the level of complexity of the tree. For each tree in the sequence, we evaluate its performance on a validation set (test set) by calculating the misclassification rate, which measures the proportion of incorrectly classified cases.
- Next, we identify the minimum misclassification rate among all the tree sizes, denoted by p*. This is the performance benchmark that we aim to achieve.
- Then, we interpret the classifications as Bernoulli events, which are binary outcomes (e.g., success or failure). This allows us to model the misclassification rate as a probability distribution, and estimate its standard error, which reflects the **uncertainty of the estimate.**
- Finally, we choose the smallest tree size whose misclassification rate is no more than one estimated standard error above the minimum misclassification rate p*. This is known as the "one SE rule", and it ensures that the selected tree is not significantly worse than the optimal tree size, while avoiding overfitting.
- In other words, we choose the smallest tree size that has a misclassification rate smaller than p* + SE, where SE is the estimated standard error of the misclassification rate. This threshold is chosen to balance between bias (choosing a suboptimal tree) and variance (choosing an overly complex tree).

**The Penalty Function Approach**

- When fitting a curve or function to a set of data points (xi,yi), one approach is to use a smoother to estimate the underlying trend or relationship between the variables. The goal is to find a curve that accurately captures the pattern in the data, while also being smooth and generalizable to new data.
- One way to evaluate the goodness of fit of the smoother is to calculate the residual sum of squares (ResSS), which measures the squared difference between the predicted values (y-hat) and the actual values (yi) of the response variable. A smaller ResSS indicates a better fit of the curve to the data.
- However, if we choose a curve that makes ResSS approach 0, we risk overfitting the data. This means that the curve will fit the data too closely and capture the noise or random variation in the data, rather than the underlying trend. This will result in a curve that is too wiggly and will not generalize well to new data.
- To address this problem, we can include a penalty term in the objective function that measures the wiggliness or complexity of the curve. This penalty term is usually a function of the second derivative of the curve, which measures the rate of change of the slope or curvature of the curve. A higher penalty value means that the curve is more wiggly, while a lower penalty value means that the curve is smoother.
- By adding the penalty term to the objective function, we seek to balance between the goodness of fit (measured by ResSS) and the smoothness of the curve (measured by the penalty term). This results in a curve that is more moderate and generalizable, and is less likely to overfit the data. This technique is often called regularization or shrinkage, and is commonly used in statistical modeling, machine learning, and data analysis.
    
    
    **minθ A+λB** 
    
    - θ is the vector of parameters
    - λ (≥ 0) is the balancing or trade-off parameter.
    - The smaller quantity A is, the closer the fit is with the data. Simply minimizing A would result in an extreme fit that would not generalize well for future data.
    - But if we add a quantity B to the objective function that increases as A decreases then we can regularize the fit.
    
    - As λ → 0+ the fit will become complicated because B becomes negligible.
    - As λ → ∞ the fit becomes simpler because λB is forced to remain small relative to A, i.e., the penalty B is forced to decrease quicker relative to the increase in A.

**Smooth splines** 

Model assessment:  the process of evaluating a model’s performance,

- Fit a smoother that explains the data perfectly — residual = 0

**AIC / BIC** 

- The Akaike Information Criterion (AIC) and Bayesian Information Criterion (BIC) are two commonly used statistical measures for comparing different models based on their goodness of fit and complexity. Both criteria balance the fit of the model (measured by the log-likelihood) with the number of parameters in the model (measured by p).
- The AIC is defined as -2log(L) + 2p, where L is the likelihood function and p is the number of parameters in the model. The AIC penalizes models with more parameters, but rewards models that fit the data well. The goal is to choose the model with the lowest AIC value, which indicates the best balance between fit and complexity.
- The BIC is similar to the AIC, but includes a larger penalty for the number of parameters. The BIC is defined as -2log(L) + p log(n), where n is the sample size. The BIC is more conservative than the AIC, and tends to favor simpler models with fewer parameters. The goal is to choose the model with the lowest BIC value.
- Both the AIC and BIC are widely used in model selection, especially in cases where multiple models are being compared that are not nested (i.e., the models do not have a hierarchical relationship). The AIC and BIC have known properties and can be used to compare models across different fields of study. However, they should be used with caution and should not be the sole basis for model selection. Other factors, such as theoretical considerations and prior knowledge, should also be taken into account.

**LASSO — variable selection**

- The Least Absolute Shrinkage and Selection Operator (LASSO) is a technique used in regression analysis to perform variable selection and regularization. The LASSO penalty is an L1 norm, which adds a term to the objective function that is proportional to the sum of the absolute values of the regression coefficients.
- As the value of λ (the tuning parameter that controls the amount of shrinkage) increases, the LASSO shrinks the magnitude of the regression coefficients  (i.e. Bk) towards zero, effectively eliminating some of the variables from the model. When λ is sufficiently large, all the coefficients become zero (except for the intercept term, which is unpenalized).
- In contrast to traditional regression analysis, which includes all variables in the model, regardless of their importance, LASSO allows for automatic variable selection, identifying a smaller subset of variables that are most important for predicting the response variable. This leads to a more interpretable and parsimonious model.
- In the example on slide 110, the paths of the LASSO coefficients are traced for a linear regression model fitted to the azpro data frame in COUNT. The first plot shows λ on a log scale as its x-axis, while the second plot shows the L1 norm of the coefficient vector (β2,...,βp)T. As λ increases, the coefficients are shrunk towards zero, and some variables are dropped from the model. The L1 norm plot shows how the variables are added and dropped as λ changes, allowing for a visual representation of the variable selection process.

### Statistical Methods in Data Mining

1. Supervised learning and unsupervised learning 
    - Supervised learning is a type of machine learning in which the algorithm learns from labelled data. The labeled data consists of inputs (features) and corresponding outputs (labels). The goal of supervised learning is to learn a function that maps inputs to outputs, so that the algorithm can make predictions on new, unseen data. Some common examples of supervised learning algorithms are linear regression, logistic regression, decision trees, and neural networks.
    - Unsupervised learning, on the other hand, is a type of machine learning in which the algorithm learns from unlabeled data. The unlabeled data consists of inputs (features) without corresponding outputs (labels). The goal of unsupervised learning is to learn patterns and structures in the data, without the need for explicit supervision. Some common examples of unsupervised learning algorithms are clustering, principal component analysis (PCA), and autoencoders.
    - The main difference between supervised and unsupervised learning is that supervised learning requires labeled data, while unsupervised learning does not. In supervised learning, the algorithm is trained to predict a specific output based on the input, whereas in unsupervised learning, the algorithm is trained to identify patterns and structures in the data without a specific output in mind.
2. Classification or Regression: 
    1. Classification is a learning function that maps (classifies) a data item into one of several predefined classes using the values of explanatory variables.
    2. Regression A response variable is modeled as a function of explanatory variables.
    3. Prediction The “future” value of the of the response variable is predicted using the explanatory variables. Can be implemented in a variety of ways (e.g., using classification or regression).
    4. Link Analysis † (aka dependency modeling, affinity grouping) is used to determine which things go together. That is, finding a model which describes significant dependencies between the variables.
    5. Clustering is the task of splitting a heterogeneous population into a number of more homogeneous subgroups (clusters). It seeks to identify a finite set of categories or clusters to describe the data. Clustering is an example of **unsupervised** or undirected learning—there is no response variable. The objectives of unsupervised learning are more fuzzy, and it is difficult to know how well you are doing.
    6. Summarization (aka description) involves simply describing what is going on in a complicated database in a way that increases our understanding of the people, products or processes that produced the data. A good description will often suggest an explanation.

**Traditional methods vs pure prediction algorithms**
Prediction, estimation, and attribution are three important concepts in statistics.

- Prediction refers to the process of using a statistical model to make predictions or forecasts about future events or outcomes. This is often done using data from the past to make predictions about the future. Prediction can be done using a variety of statistical models, including linear regression, time series analysis, and machine learning algorithms.
- Estimation refers to the process of using statistical methods to estimate unknown parameters or characteristics of a population or process. This is often done using sample data to make inferences about the population from which the sample was drawn. Estimation can be done using a variety of statistical methods, including maximum likelihood estimation, method of moments, and Bayesian inference.
- Attribution refers to the process of determining the causes or sources of variation or differences in data. This is often done by analyzing the relationships between variables and identifying the factors that contribute to observed patterns or differences. Attribution can be done using a variety of statistical methods, including analysis of variance (ANOVA), regression analysis, and factor analysis.
