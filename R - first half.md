# Quick Notes

### Hypothesis Testing

1. NULL hypothesis
   1. always clearly clarify what H0 and Ha is
2. State the criteria
3. test statistic
4. p-value - need to explain what the p-value is
   1. probability of the test stats given the NULL is true
   2. some cases will use pt()
5. if the probability of the extremes give the NULL < 0.05, reject $H_0$

### Statistically Significant

- if p-value < 0.01, reject $H_0$, results are **highly significant**
- if 0.01 < p-value < 0.05, reject $H_0$, results are **statistically significant**
- if 0.05 < p-value < 0.10, do not reject $H_0$, results **trending towards significant**

### Parametric or nonparametric?

Page 628 says for Kruskal

> Better to use non-parametric test when between the treatments, there's a difference in variance (tight vs loose), and eye ball raw data on normal probability plot to see whether any points show signs of deviation.
>
> <u>the resulting image look close to a straight line if the data are approximately normally distributed. Deviations from a straight line suggest departures from normality</u>. - wiki

Page 633 says for Friedman

Plot treatment vs residual and a normal probability plot. If variance or slope shows any inconsistency, it might be more appropriate to use nonparametric.



- Parametric
  - assumes the observations are normally distributed
- Non-parametric
  - has no assumptions of normality; distribution free
  - 

### Treatment and Block Design

- **treatment groups treatment**
  - if treatment is column, everything data in the same column gets the same value
  - When data is combined vertically we use below
  - `rep(c(1:6), each = 4) #111122223333....`
- **block groups block**
  - if treatment is row, everything data in same row gets same value
  - Since data is combined vertically, we use....
  - `as.factor(rep(c(1:4), 6))  #123412341234...`

### Experiment Designs

- Completely randomized design
  - randomly assign people to groups
  - random treatment given to groups
  - same group receives same treatment.
  - e.g. conduct experiment to determine which environment is best for studying: library, own room, outside. 30 students participate in the study.
    - treatments: library, own room, outside
    - 30 students randomly assigned to these treatment groups.
- Block design
  - to avoid random sample focusing on a group, we split men and women into blocks before sampling
  - idea is to reduce biasness of those within the sample who might share some common characteristics
  - e.g. same example above, but add "the researcher believes gender has an effect on the results"
    - 30 students split to blocks, based on characteristics believed to have an effect.
    - then proceed with randomizing
- Matched Pair Design
  - e.g. what gasoline is more efficient? type-a or type-b? 3 cars used
    - 3 cars and each car will receive both treatments of type-A and type-B.
    - which treatment goes first is random

### One tail or two tail?

- One tail?
  - only justified for a specific prediction about the <u>direction of the difference</u>
  - one-tailed test <u>has more statistical power</u> than a two-tailed test <u>at the same significance</u> (alpha) level
  - words: "less than", "smaller than", "greater", "more than"
  - results are more likely to be significant for a one-tailed test if there truly is a difference between the groups in the direction that you have predicted.
- Two tail?
  - When in doubt, it is almost always more appropriate to use a two-tailed test. 
  - words: "is", "is equal to", "is the same as"
  - e.g. determine if there is any difference between the groups you are comparing
  - e.g. if Group A scored higher or lower than Group B, then you would want to use a two-tailed test

### Normality Test?

https://towardsdatascience.com/6-ways-to-test-for-a-normal-distribution-which-one-to-use-9dcf47d8fa93

### How to pick a test?

![](https://www.researchgate.net/profile/Alexandru-Ionut_Petrisor/publication/298166641/figure/tbl1/AS:670702474637317@1536919339380/Parametric-tests-and-nonparametric-equivalents.png)

1. #### Parametric

   1. ##### 2 groups?

      1. ###### Unpaired $\rightarrow$ Sample t-test

         - lab 2.1

         - ```R
           bottles <- c(484.11, 459.49, 471.38, 512.01, 494.48, 528.63, 493.64, 485.03, 473.88,
           501.59, 502.85, 538.08, 465.68, 495.03, 475.32, 529.41, 518.13, 464.32, 449.08, 489.27)
           mean(bottles)
           sd(bottles)
           
           # mu = true mean
           # alternative = greater/less
           # conf.level, used to show confident interval. Does not affect t, df, p-value
           t.test(bottles, mu=500, alternative="less", conf.level=0.90)
           ```

         - ```R
           [1] 491.5705
           [1] 24.79368
           
           	One Sample t-test
           
           data:  bottles
           t = -1.5205, df = 19, p-value = 0.07243
           alternative hypothesis: true mean is less than 500
           90 percent confidence interval:
                -Inf 498.9315
           sample estimates:
           mean of x 
            491.5705 
           ```

         - lab 2.2

         - ```R
           salmonlv <- c(0.593,0.142, 0.329, 0.691, 0.231, 0.7930,.519,0.392,0.418)
           t.test(salmonlv, alternative="greater", mu=0.3)
           ```

         - ```R
           	One Sample t-test
             
           data:  salmonlv
           t = 2.2051, df = 8, p-value = 0.02927
           alternative hypothesis: true mean is greater than 0.3
           95 percent confidence interval:
            0.3245133       Inf
           sample estimates:
           mean of x 
           0.4564444 
           ```

      2. ###### Paired $\rightarrow$ <u>Paired</u> t-test

         - lab 2.3

         - ```R
           #install.packages('MASS')
           library('MASS')
           t.test(immer$Y1,immer$Y2, paired=TRUE, conf.level = 0.95)
           ```

         - ```R
           	Paired t-test
             
           data:  immer$Y1 and immer$Y2
           t = 3.324, df = 29, p-value = 0.002413
           alternative hypothesis: true difference in means is not equal to 0
           95 percent confidence interval:
             6.121954 25.704713
           sample estimates:
           mean of the differences 
                          15.91333
           ```

      3. ###### 2 sample unequal lengths $\rightarrow$ Welch t-test

         - lab 2.4

         - ```R
           mpg.auto <- mtcars[mtcars$am == 0,]$mpg
           mpg.manual <- mtcars[mtcars$am == 1,]$mpg
           t.test(mpg.auto, mpg.manual)
           ```

         - ```R
           Welch Two Sample t-test
           
           data:  mpg.auto and mpg.manual
           t = -3.7671, df = 18.332, p-value = 0.001374
           alternative hypothesis: true difference in means is not equal to 0
           95 percent confidence interval:
            -11.280194  -3.209684
           sample estimates:
           mean of x mean of y 
            17.14737  24.39231
           ```

   2. ##### 2+ groups?

      1. ###### Sample design $\rightarrow$ 1-way ANOVA (equivalent to running multiple t-test)

         - lab 3.1, 3.2

         - ```R
           group1 <- c(18.2, 20.1, 17.6, 16.8, 18.8, 19.7, 19.1)
           group2 <- c(17.4, 18.7, 19.1, 16.4, 15.9, 18.4, 17.7)
           group3 <- c(15.2, 18.8, 17.7, 16.5, 15.9, 17.1, 16.7)
           
           mean(group1)
           mean(group2)
           mean(group3)
           
           observe <- c(group1, group2, group3)
           
           group_numbers = rep(1:3, c(7,7,7))
           group_numbers = as.factor(group_numbers)
           
           data_groups <- data.frame(observe, group_numbers)
           
           summary(aov(data_groups$observe~data_groups$group_numbers,data=data_groups))
           ```

         - ```R
           Warning messages:
           1: In readChar(file, size, TRUE) : truncating string with embedded nuls
           2: In readChar(file, size, TRUE) : truncating string with embedded nuls
           [1] 18.61429
           [1] 17.65714
           [1] 16.84286
                                     Df Sum Sq Mean Sq F value Pr(>F)  
           data_groups$group_numbers  2  11.01   5.503   3.968 0.0373 *
           Residuals                 18  24.96   1.387                 
           ---
           Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
           ```

      2. ###### Block design $\rightarrow$ Block design 2-way ANOVA

         - | Soil Prep     | Loc 1 (blk) | Loc 2 (blk) | Loc 3 (blk) | Loc 4 (blk) | $\Large T_j$            |
           | ------------- | ----------- | ----------- | ----------- | ----------- | ----------------------- |
           | A (treatment) | 11          | 13          | 16          | 10          | **50**                  |
           | B (treatment) | 15          | 17          | 20          | 12          | **64**                  |
           | C (treatment) | 10          | 15          | 13          | 10          | **48**                  |
           | $\Large B_j$  | **36**      | **45**      | **49**      | **32**      | **162**$=\Sigma x_{ij}$ |

         - ```R
           loc1 <- c(11,15,10)
           loc2 <- c(13,17,15)
           loc3 <- c(16,20,13)
           loc4 <- c(10,12,10)
           location <- c(loc1,loc2,loc3,loc4)
           location
           
           locTreat <- as.factor(rep(c(1:3), 4)) #123123123123
           locBlock <- as.factor(rep(c(1:4), each = 3)) #111222333444
           locTreat
           locBlock
           
           df.location <- data.frame(location, locTreat, locBlock)
           summary(aov(location~locTreat+locBlock, data=df.location))
           ```

         - ```R
            [1] 11 15 10 13 17 15 16 20 13 10 12 10
            [1] 1 2 3 1 2 3 1 2 3 1 2 3
           Levels: 1 2 3
            [1] 1 1 1 2 2 2 3 3 3 4 4 4
           Levels: 1 2 3 4
                       Df Sum Sq Mean Sq F value  Pr(>F)   
           locTreat     2  38.00  19.000   10.06 0.01212 * 
           locBlock     3  61.67  20.556   10.88 0.00769 **
           Residuals    6  11.33   1.889                   
           ---
           Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
           ```

           

2. #### Non-Parametric

   1. ##### 2 groups?

      1. ###### Unpaired $\rightarrow$ Wilcoxon rank-sum

         - lab 5.1

         - ```R
           deTree <- c(75,79,69,78,65,87,74,81,77,88)
           logReg <- c(85, 68, 78, 73, 69, 76, 69, 80, 73, 67, 78, 82)
           
           #H0 - both algorithms are the same
           wilcox.test(deTree,logReg, exact = F)
           
           # exact	- a logical indicating whether an exact p-value should be computed.
           # Since p-value > 0.05, we cannot reject H0.
           ```

      2. ###### Paired $\rightarrow$ Wilcoxon <u>signed</u> rank-sum

         - lab 5.2, 5.3

         - ```R
           ziptime <- c(10, 44, 65, 77, 43, 44, 22, 66, 50, 100, 55, 99, 44, 23, 100, 88, 200,
           220, 110, 551)
           tartime <- c(20, 55, 75, 60, 55, 88, 35, 33, 35, 80, 65, 82, 47, 35, 97, 110, 250,
           190, 111, 600)
           
           wilcox.test(ziptime, tartime, paired = T)
           # Since p-value > 0.05, we cannot reject H0.
           ```

   2. ##### 2+ groups?

      1. ###### Sample design $\rightarrow$ Kruskal-Wallis (rank-equivalent of one-way ANOVA test)

         - lab 6.2

         - ```R
           kruskal.test(Ozone~Month, data=airquality)
           ```

      2. ###### Block design $\rightarrow$ Friedman test (equivalent of two-way ANOVA F test)

         - lab 6.3

         - ```R
           dtree <- c(75, 79, 69, 78, 65, 87, 74, 81, 77, 85)
           lreg <- c(85, 68, 78, 73, 69, 76, 69, 80, 73, 67)
           nbayes <- c(86, 75, 79, 82, 68, 69, 77, 81, 80, 79)
           
           mean(dtree)
           mean(lreg)
           mean(nbayes)
           
           combined_ml <- c(dtree, lreg, nbayes)
           group_x <- as.factor(rep(c('dt','lt','nb'),each=10)) # 1....2....3...
           blocks <- as.factor(rep(c(1:10),3)) # repeat 123123123...
           
           friedman.test(combined_ml, group_x, blocks)
           ```

           

### z-test

**Null**: Sample mean is same as the population mean

**Alternate**: Sample mean is not same as the population mean

The statistics used for this hypothesis testing is called z-statistic, the score for which is calculated as

z = (x — μ) / (σ / √n), where
x= sample mean
μ = population mean
σ / √n = population standard deviation

### t-test/t-distribution

- used to compare the mean of two given samples
- when <u>sample is < 30</u> and <u>we don't know</u> the mean, variance, SD of the population.
- small sample risk we miss or over-emphasize variations
- 3 versions
  1. **independent** sample: compares mean for two groups
  2. **paired** sample: compares means from the same group at different times
     - e.g. when giving one group of people the same survey, twice
     - lets us know the mean changed between 1st and 2nd survey
     - pretty much same as 1-sample test
  3. **1-sample** t-test: tests the mean of a single group against a known mean

The statistic for this hypothesis testing is called t-statistic, the score for which is calculated as
t = (x1 — x2) / (σ / √n1 + σ / √n2), where
x1 = mean of sample 1
x2 = mean of sample 2
n1 = size of sample 1
n2 = size of sample 2

### ANOVA

- ANOVA or aka completely randomized design
- use ANOVA because we wish to...
  - compare multiple (three or more) samples with a single test
  - compare the means of 2+ population; answers they come from a *common* population
  - allows us to "account for variation" <u>at the row level</u> due to some other factors or grouping
  - adding blocks, we can remove the row variance from the overall error variance
  - allows greater focus on columns on group difference; easier to detect group difference
- <u>equivalent to running multiple t-test</u>
- Null: All pairs of samples are same i.e. all sample means are equal
- Alternate: At least one pair of samples is significantly different
- use F-statistic
- one-way
  - calculates: mean per column; grand mean
  - aside from ERROR, we are only working with one potential source
- two-way
  - purpose of blocking is to remove or isolate the *block-to-block variability* that might otherwise hide the effect of the treatments
  - one unit within each block randomly assigned to each treatment
  - e.g.. 5 assembly operators (blocks), 3 assembly lines (treatment)
    i.e.. operator 1-CAB; operator 2-ABC, operator 3-BAC...
  - partitioning the total/overall variance into different parts; assigning parts of the overall variance to different sources
  - more powerful hypothesis test
  - **note**: the $df$ for the ERROR part is tricky

The statistics used to measure the significance, in this case, is called F-statistics. The F value is calculated using the formula

**F= ((SSE1 — SSE2)/m)/ SSE2/n-k**, where
SSE = residual sum of squares
m = number of restrictions
k = number of independent variables

### Chi-Square

- assist to determine the role of random chance variation between our categorical variables
- to understand the relationship between two *categorical variables*
  - e.g. grade level, sex, age group, years
- involves frequency of events; count
- compare *observed* vs *expected*
  - e.g. we expected a die 1/6 roll, but what was the outcome?
- for two independent variables is used to compare two variables in a contingency table
  - small chi-sq = fits
  - high chi-sq = doesn't fit
- Null: Variable A and Variable B are independent
- Alternate: Variable A and Variable B are not independent.
- for goodness of fit tests
- Example: a sample of n=11 measurements, 95% of the area under the chi-square distribution lies to the right of 3.94.
  table reads: df=10, x^2.950 = 3.94030
- calculated > table, we reject $H_0$

### Wilcoxon Rank

- test for location or comparing two medians
- can use for not normal, but assume similar shapes
- use RANKS
- Null: two pop are the same
  Alternative: two pop are not the same
- Reject $H_0$
  - 2 pop are far apart
- Fail to reject $H_0$
  - 2 pop are close, almost overlapping
- Quick sample: students doing poorly in sem 1 and must improve in sem 2. Test to see if there's a statistically significant difference.
  - We use lower tail test.
  - $H_0: Sem_1 - Sem_2 \geq 0$ i.e. worst, sem1 > sem2
  - $H_a:Sem_1-Sem_2<0$ i.e. <u>better</u>, sem2 > sem1

# Cleaning Data

#### How to clean data

Imputation: process to replace missing data with substituted values

1. Missing at Random (MAR): 
   - Should <u>impute</u> this data since a pattern exists in how the data is missing. 
   - Can <u>use averages by groups</u> for example (e.g. average number of movies seen by 12-15 year olds vs. 40-55 year olds)
2. Missing Completely at Random (MCAR)
   - Usually this type of missing data will not result in bias, so these records should be <u>deleted</u>.
3. Missing Not at Random (MNAR)
     - most challenging to deal 
     - the missing-ness is dependent on another survey variable. 
     - Sometimes we can impute this, however if the data is critical we would need to find another way to collect it.

#### How to impute

There are a couple of ways we think about imputing missing data:

- use metrics like mean or median.
   - first check the data distributed and what logical segmentation makes sense (for example, age groups, gender, etc.).
- model the missing data - for example, if every 5th element was missing out of 200 respondents, you could create a mathematical function that can estimate the value.

# Test of Hypothesis

- Required: Chapter 10: Inference from Small Samples

General Info

- critical value: point beyond we reject H0
- P-value: probability of respective statistic (Z, T, Chi)
- 

## **Large Sample Test of Hypothesis**

- single population mean $\mu$

- different of 2 population mean $(\mu_1-\mu_2)$

- single population variance $\sigma^2$

- comparison of 2 population variance $\sigma^2_1-\sigma^2_2$

- **student t-distribution**

  - density function for small samples

  - mound-shaped and symmetric about $t=0$, just like $z$.

  - has heavier tails

  - curve doesn't approach horizontal axis as quickly as z does

  - shape depends on sample size $n$.  As $n$ increases, the variability of $t$ decreases

  - when $n$ is infinity large, $t$ and $z$ distributions are identical.

  - e.g. For a sample of size n=10, find value of $t$ s.t. only 1% of all values of $t$ will be smaller

    - $df=n-1=10-1$
    - $t_{0.010}=2.821$
    - Since its smaller so area to the left, therefore $-2.821$

  - <u>Sample must meet following requirements</u>

    - sample **must be randomly selected**
    - **population** from which you are sampling **must be normally distributed**.

  - to tell whether sample is from a normal population, graphic it. As long as plot mound up, its fairly safe to use $t$ statistics.

  - > $\LARGE{t=\frac{\bar x - \mu}{s \sqrt{n}}}$ for $n<30$


## **Small-Sample Hypothesis Test for** $\mu$. (page 369)

Assuming: sample is randomly selected from normal distribution population.

1. Null Hypothesis H0
2. Alternative Hypothesis: one-tailed test or two-tailed test
3. Test statistics: t-distribution
4. Rejection region: Reject H0 when p-value < $\alpha$

- Small-Sample $(1-\alpha)$ 100% confidence interval for $\mu$

  - $\Large{\bar x \pm t_{\alpha/2}\frac{s}{\sqrt{n}}}$
- lower one-sided confidence bound

  - $\Large{\mu\geq\bar x - t_\alpha\frac{s}{\sqrt{n}}}$
  - to calculate area left of the bounds

- Upper one-sided confidence bound

  - $\Large{\mu\leq\bar x + t_\alpha\frac{s}{\sqrt{n}}}$

- Recap: There are two ways to conduct a test of hypothesis

  1. critical value approach: set up rejection region based on critical values of the statistic's sampling distribution. If the test statistics falls in the rejection region, you reject $H_0$
  2. p-value approach: calculate p-value based on observed value of the test statistic. If p-value < significance level $\alpha$, we reject $H_0$.


## Topic: **Inference from Small Samples**

**Small Sample Inference for difference between two population means**

- if sample sizes are small, the statistic doesn't have an approximate normal distribution

- another assumption has to be made: <u>both population have exactly the same shape</u>

  - $\Large{\sigma^2_1=\sigma^2_2=\sigma^2_3}$

- then the SE (standard error) = $\Large{\sqrt{\sigma^2(\frac{1}{n_1}+\frac{1}{n_2})}}$

  - Standard Error (SE): SD of its sampling distribution or an estimate of that SD

- > $\LARGE{s^2=\frac{(n_1-1)s^2_1+(n_2-1)s^2_2}{n_1+n_2-2}}$
  >
  > $\LARGE{t=\frac{(\bar x_1-\bar x_2)-(\mu_1-\mu_2)}{\sqrt{s^2(\frac{1}{n_1}+\frac{1}{n_2})}}}$

## **10.5 Paired T-test** (Welch Two Sample t-test)

> - $\LARGE{t=\frac{\bar d}{s_d/\sqrt{n}}}$
> - $n=\text{number of paired differences}$
> - $\bar d = \text{mean of the sample differences}$
> - $s_d = \text{SD of the sample differences}$
> - $\LARGE{t=\sqrt{\frac{\Sigma(d_i-\bar d)^2}{n-1}}=\sqrt{\frac{\Sigma d_i^2-(\frac{\Sigma d_i}{n})^2}{n-1}}}$

Example: Do the data present sufficient evidence to indicate a different in average wear for two tire types?

```
A: tire1 <- c(10.6,9.8,12.3,9.7,8.8)
B: tire2 <- c(10.2,9.4,11.8,9.1,8.3)

dBar <- mean(tire1-tire2)
Sd <- sd(tire1-tire2)
tFinal <- dBar/(Sd/sqrt(5))
```

## 10.x Chi-Squared

- random independent variables taken from a normal and square that number

- DF determines how many of these variables are taken, and sum them up

- > $\Chi^2 = \Large{\Sigma\frac{(\text observed - \text expected)^2}{\text expected}}$ It gives the p-value; the area to the right of $\Chi^2$

- $DF=n-1$

- There are two Chi-square tables, upper and lower tail. Usually use upper tail.

- $\alpha$ = 0.05 means we'll look at the chart 0.05 (not 0.95)

  - the number from table is 11.07, and calculated is 11.44
  - Since calculated > table, we reject $H_0$ 
    - Explanation: the observed frequency different significantly from the expected theoretical. Therefore the variation is too great to be explained by *chance alone*. Therefore we must reject $H_0$
  - we got 11.44, and it is more extreme than critical level 11.07. Therefore we reject.
  - OR, the value we get from calculation, on the line of DF, we figure which P value column it is

- $H_0$ in layman's terms

  - the variation in our observed data simply due to chance
  - is the variation beyond what random chance should allow?
  - how far can our data vary before we have to reject our NULL?

#### R implementation

```R
# chi-square
# confidence level: 95%
# error probability: P-value <= 0.05
# DF: (6-1) = 5
# detail: We apply the quantile function qchisq of the Chi-Squared distribution against the decimal values 0.95.

> qchisq(.95, 5)
[1] 11.0705

# OR
# R's default is lower.tail = TRUE
> qchisq(.05, 5, lower.tail = F)
[1] 11.0705
```

- Lab 2

# Analysis of Variance (ANOVA)

## Overview

- a statistical method used to test differences between two or more means.
- the name is appropriate because <u>inferences about means are made by analysing variance</u>.

#### Example #1:

 A group of people is randomly divided into an experimental and a control group. The control group is given an aptitude test after having eaten a full breakfast. The experimental group is given the same test without having eaten any breakfast.

- experimental unit = people
- response = test score
- 1x factor = meal
- 2x treatments/levels = breakfast and no breakfast

#### Example 2

by randomly selecting 20 men and 20 women for the experiment. These two groups were then randomly divided into 10 each for the experimental and control groups.

- 2x factors of interest = Gender, Meal
- 2x levels = men, women, breakfast, no breakfast

#### Why ANOVA and not $t$-test?

- in t-test requires c($n$,2) per tests needed
- each test contribute to error
- increasing tests increasing chances of wrong conclusion
- ANOVA provides one overall tests to just the equality of the $k$ population means.
- After detecting actual difference in means, then we can proceed to other procedures

## Topic: ANOVA #1 - **one-way classification**

##### Assumptions

- Variation Between Sample Means
- Assumptions
  - <u>population is normal</u>
  - <u>same variances</u>

##### Equation

**SST** = SS for treatment - measuring the variation among the k sample means (between SSB)
**SSE** = {SS for Errors - measure variation within the k samples (within SSW)
**Total** SS = SST + SSE
$df$(total) = $df$(treatments) + $df$(error)



##### ANOVA Table

| Source            | Df    | Sum Sq   | Mean Sq            | F-statistic |
| ----------------- | ----- | -------- | ------------------ | ----------- |
| Treatments        | $k-1$ | SST      | $\text{SST}/(k-1)$ | MST/MSE     |
| Error (residuals) | $n-k$ | SSE      | $\text{SSE}/(n-k)$ |             |
| Total             | $n-1$ | Total SS |                    |             |

$$
\Large{\text {Total SS (Total Sum of Squares)}=\Sigma(x_{ij}-\bar x)^2}\\
df=(k-1)+(n-k)=n-1\\
$$



> $$
> \LARGE{
> \text{CM}: \text {Correction of the means}\\\text{MST}: \text {mean square of treatments}\\\text{MSE}: \text {mean square of errors}\\
> 
> 
> \text{Total SS}=\Sigma X_{ij}^2-\text {CM}\\
> \text {CM}=\frac{(\Sigma x_{ij})^2}{n}\\
> \text {SST}=\Sigma\frac{T_i^2}{n_i}-\text{CM}\\
> \text {SSE}=\text {Total SS}-\text {SST}\\
> }
> $$

#### Example:

- | No Breakfast         | Light Breakfast | Full Breakfast |
  | -------------------- | --------------- | -------------- |
  | 8                    | 14              | 10             |
  | 7                    | 16              | 12             |
  | 9                    | 12              | 16             |
  | 13                   | 17              | 15             |
  | 10                   | 11              | 12             |
  | $T_1=8+7+9+13+10=47$ | $T_2=70$        | $T_3=65$       |

  $$
  \begin{aligned}
  G &=\text{Total of all data points}=182\\
  \text {CM}&=\frac{182^2}{15}=2208\\
  \text {Total SS} &=8^2+7^2+...+12^2-\text{CM}=129.73\\
  \text {SST} &=\frac{47^2}{5}+\frac{70^2}{5}+\frac{65^2}{5}-\text{CM}=58.53\\
  \text {SSE} &= \text {Total SS} - \text {SST}=71.2\\
  \text{(columns) } k &= 3\\
  \text{(total data points) }n &= 15\\
  \end{aligned}
  $$

- ANOVA Table

  - | Source            | Df    | Sum Sq   | Mean Sq            | F-statistic |
    | ----------------- | ----- | -------- | ------------------ | ----------- |
    | Treatments        | $k-1$ | SST      | $\text{SST}/(k-1)$ | MST/MSE     |
    | Error (residuals) | $n-k$ | SSE      | $\text{SSE}/(n-k)$ |             |
    | Total             | $n-1$ | Total SS |                    |             |

  - | Source     | Df   | Sum Sq | Mean Sq | F-statistic     |
    | ---------- | ---- | ------ | ------- | --------------- |
    | Treatments | 2    | 58.53  | 29.27   | 29.27/5.93=4.93 |
    | Errors     | 12   | 71.2   | 5.93    |                 |
    | Total      | 14   | 129.73 |         |                 |

  - $$
    \begin{aligned}
    \text {We use }\alpha&=0.05.\\
    \text F&=4.93 \text { with F-table} \\
    df_1 &= 2 \text{ and } df_2=12\\
    \because F>F_{0.05}&=3.89 \therefore\text{We reject } H_0
    \end{aligned}
    $$

- R

  1. assuming there's groupA, B, and C
  2. merge all 3 groups to one long dataframe
     add another column with values `as.factor`; it shall assign each observation to a group
  3. to observe as a boxplot: `boxplot(y~x,data=thedataframe)`
  4. to get ANOVA: `summary(aov(y~x, data=thedataframe))`

## Topic: ANOVA #2 - Two-way ANOVA

Randomized Block Design: Two-way classification

- design uses <u>blocks</u> of $k$ experimental units which are <u>relatively similar</u>
  - assign blocks that are similar to each other
- one unit within each block randomly assigned to each treatment
- Treatment and blocks
  - total number of observation: $n=bk$ where $k$ are treatments, $b$ are blocks.
  - purpose is to remove or isolate block-to-block variability

> $$
> \LARGE{\text {Total SS} = \Sigma(X_{ij}-\bar x)^2=\text {SST}+\text {SSB}+\text {SSE}}\\
> $$
>
> - SST (sum of squares for **treatments**): measures the variation among the $k$ treatment means
> - SSB (sum of squares for **blocks**): measures the variation among the $b$ block means
> - SSE (sum of square for **error**): measures the random variation or experimental error
>   - SSE = Total SS - SST - SSB
>
> $$
> \LARGE
> {
> \text {CM} =\frac{(\Sigma x_{ij})^2}{n}\\
> \text{Total SS}=\Sigma X_{ij}^2-\text {CM}\\
> \text {SST} =\Sigma\frac{T_i^2}{b}-\text{CM}\\
> \text {SSB} =\Sigma\frac{B_i^2}{k}-\text{CM}\\
> \text {SSE} =\text {Total SS}-\text {SST}-\text {SSB}\\
> }
> $$
>
> - $T_i$ = total for treatment $i$
> - $B_j$ = total for block $j$

##### Example

| Soil Prep    | Loc 1  | Loc 2  | Loc 3  | Loc 4  | $\Large T_j$            |
| ------------ | ------ | ------ | ------ | ------ | ----------------------- |
| A            | 11     | 13     | 16     | 10     | **50**                  |
| B            | 15     | 17     | 20     | 12     | **64**                  |
| C            | 10     | 15     | 13     | 10     | **48**                  |
| $\Large B_j$ | **36** | **45** | **49** | **32** | **162**$=\Sigma x_{ij}$ |

$$
CM=2187\\
Total SS=11^2+15^2+...+10^2-2187=111\\
SST=38\\
SSB=61.6667\\
SSE=111-38-61.6667=11.3333
$$

##### ANOVA Table

| Source     | df           | SS       | MS                      | F-statistic |
| ---------- | ------------ | -------- | ----------------------- | ----------- |
| Treatments | $k-1$        | SST      | $\text{SST}/(k-1)$      | MST/MSE     |
| Blocks     | $b-1$        | SSB      | $\text{SSB}/(b-1)$      | MSB/MSE     |
| Error      | $(b-1)(k-1)$ | SSE      | $\text{SSE}/(b-1)(k-1)$ |             |
| Total      | $n-1$        | Total SS |                         |             |

> If calculated F-statistics > $\Large{F_{0.05}} \rightarrow \text {reject } H_0$ 

##### Other

- Required: Chapter 11: The Analysis of Variance
- Lab 3

# **Multiple Linear Regression**

#### Simple Linear Regression

- Least Square Estimators

> 
> $$
> \LARGE
> {
> \bar y = a+ b \bar x\\
> }
> \\x_1, x_2, ..., x_n\\
> y_1, y_2, ..., y_n\\
> \LARGE
> {
> b=\frac{S_{xy}}{S_{xx}}\\
> S_{xy}=\sum_{i=1}^n{(x_i-\bar x)(y_i-\bar y)}=\Sigma x_iy_i-\frac{(\Sigma x_i)(\Sigma y_i)}{n}\\
> S_{xx}=\sum_{i=1}^{n}{(x_i-\bar x)^2}=\Sigma x_i^2-\frac{(\Sigma x_i)^2}{n}\\
> \hat y=a+bx\\
> }\\
> \hat y \text{ = values on the fitted line}
> $$
> 

#### Multiple Linear Regression

- an extension of the simple linear regression
- many-to-one relationship
  - 2+ independent-to-1-dependent
- A lot of prep work to do before running the MLR
  - idea is to clean the data so each independent variable does not rely each other - multicollinearity

#### General Linear Model

$\large{y=\beta_0+\beta_1x_1+beta_2x_2+...+beta_kx_k+\varepsilon}$

- $\LARGE y$ is the dependent variable; the response variable we want to predict.
- $\LARGE x$ is the independent predictor variables, measured without error
- $\beta_0, \beta_1, \beta_2,...,\beta_k$ are unknown constants.
- $\LARGE\varepsilon$ is the random error
  - value of $\varepsilon$ should be...
    - independent
    - have a mean of 0, and a common variance for any set $x_1, x_2, ... x_k$
    - normally distributed
- use `anova()` with `lm()` to check the strength of each attribute

##### Other

- Chapter 12: Linear Regression and Correlation
- Chapter 13: Multiple Regression Analysis
- Lab 4

# The Wilcoxon Tests

Topics: The Wilcoxon Tests

- parametric tests assumes...
  - data have <u>same variance</u>, regardless of the treatments/conditions in the experiment
  - data is <u>normally distributed</u>
- non parametric statistics
  - can't be measured on a quantitative scale
    - becomes <u>ranking</u>
  - parametric assumptions are seriously violated
- Wilcoxon
  - test for location or comparing two medians
  - can use for not normal, but assume similar shapes

#### **Wilcoxon's Rank Sum Test**

- nonparametric alternative to T-test for a comparison of population means

- rank combined sample from small $\rightarrow$ large

- Let $T_1$ represent sum of the ranks of the first sample $n_1$

  > - $$
  >   \Large
  >   {T^*_1 \text {value of the rank sum for } n_1 \text { ranked from large $\rightarrow$ small}\\
  >   T^*_1=n_1(n_1+n_2+1)-T_1
  >   }
  >   $$

- Hypothesis $n_1<n_2$

  - $H_0$: the two population distributions are same
  - $H_a$: two pop. are different (2-tailed)
  - $H_a$: pop. #1 lies left of pop. #2 (left-tailed)
  - $H_a$: pop. #1 lies right of pop. #2 (right-tailed)

##### How?

- Rank all $n_1+n_2$ observations from small $\rightarrow$ large
- test stats for left-tailed: $T_1$
  - to test pop 2 shifting to left
- test stats for right-tailed: $T_1^*$
  - to test pop 1 shifting to right
  - e.g. Suppose you want to use the Wilcoxon rank sum test to detect a shift in distribution 1 to the right of distribution 2 based on samples of size  n1=6  and  n2=8 . Use $T^*_1$.
- test stats for two-tailed: $T=min(T_1,T_1^*)$

##### Rejection?

- If observed test is $\leq$ to critical value from table, **reject** $H_0$

Example

| Species 1 | Species 2 |
| --------- | --------- |
| 235 (10)  | 180 (3.5) |
| 225 (9)   | 169 (1)   |
| 190 (8)   | 180 (3.5) |
| 188 (7)   | 185 (6)   |
|           | 178 (2)   |
|           | 182 (5)   |

since $n_1=4$ and $n_2=6$, $n_1 < n_2$
$$
T_1=(7+8+9+10)=34\\
T^*_1=n_1(n_1+n_2+1)-T_1=4(4+6+1)-34=10\\
T=min(T_1,T^*_1)=10\\
$$
Checking [Wilcoxon Rank-Sum Table](http://www.real-statistics.com/statistics-tables/wilcoxon-rank-sum-table-independent-samples/), $\alpha=0.5$ 2-tailed, $n_1=4$ and $n_2=6$: 
$T \leq 12$ and since 10 < 12 therefore we reject $H_0$.

Large sample

- $n_1, n_2 > 10$
- Null hypothesis: H0 : The population distributions are identical
- Alternative hypothesis: 
  Ha : The two population distributions are not identical (a two-tailed test). 
  Ha : The distribution of population 1 is shifted to the right (or left) of the distribution of population 2 (a one-tailed test).
- Rejection region:
  a. For a two-tailed test, reject H0 if $z > z_a/2$ or $z < z_a/2$.
  b. For a one-tailed test in the right tail, reject H0 if $z > z_a$.
  c. For a one-tailed test in the left tail, reject H0 if $z < z_a$.

#### **Wilcoxon's "Signed"-Rank Test for Paired Experiment**

##### How?

- compare 2 pop.; samples consists of paired observations
- For each pair, calculate $d=x_1-x_2$, eliminated 0 differences
- Rank absolute values of difference from 1 to $n$.
- Tied observations assigned average

##### Hypothesis

- $H_0$: two population relative frequency distributions are identical
- $H_a$: the dist. differ (two-tailed)
- $H_a$: pop #1 dist. is shifted to the right of pop #2 dist.
- Test statistic for one-tailed: $T^-$
- Test statistic for two-tailed: $T=min(T^+,T^-)$

##### Rejection?

- 1-tailed: if $T^-$ is < then critical value from table, **reject** $H_0$
- 2-tailed: if $T$ is < then critical value from table, **reject** $H_0$
- note: to detect shift of dist #2 to right of dist #1, use $T^+$ as test stats
  If $T^+$ is < critical value from table, **reject** $H_0$

##### Example

| Loc                | 1     | 2      | 3      | 4      | 5      | 6      |
| ------------------ | ----- | ------ | ------ | ------ | ------ | ------ |
| Cake mix A         | .234  | .176   | .170   | .244   | .227   | .249   |
| Cake mix B         | .223  | .208   | .194   | .263   | .234   | .282   |
| $d=x_A-x_B$        | 0.011 | -0.032 | -0.024 | -0.019 | -0.007 | -0.033 |
| Rank (ignore sign) | 2     | 5      | 4      | 3      | 1      | 6      |

$$
T^+=2\\
T^-=5+4+3+1+6=19\\
\text {test statistics using the smaller one } T=2\\
$$



##### Others

- Required: Chapter 15: Nonparametric Statistics
- Lab 5

# Nonparametric Statistics Test

- Topics

#### Pearson Correlation

#### for mean based, normally distributed, parametric)

- measures the strength between 2 random variables

- > $$
  > \LARGE{
  > r=\frac{S_{xy}}{\sqrt{S_{xx}S_{yy}}}\\
  > -1 \leq r \leq 1\\
  > r \approx 0 \rightarrow \text {no linear relationship}\\
  > r \approx \pm1 \rightarrow \text {a strong $+$ or $-$ relationship}
  > }
  > $$

#### Spearman Correlation (for mean based, not normally distributed, non-parametric) Chap 15.8 Pg. 638

- same as Pearson but **for nonparametric**
- use for non-normally dist. and presence of outliers

##### How?

- need to change values to ranks as $x_i$ and $y_i$.

##### Textbook

- pg 638
- uses $n$
- reject $H_0$ if $\LARGE r_s > \text {spearman critical value}$

#### Kruskal-Wallis Test

- nonparametric, <u>rank-equivalent of one-way ANOVA test</u> F-test for a completely randomized design
- used to detect difference in locations among more than two pop. distributions based on independent random sampling

##### How?

- $n=n_1 + n_2 + ... + n_k$ one for each sample

- rank the measurements

  - all $k$ samples from 1 to $n$.
  - tied observations are averaged
  - Calculate $T_i$: rank sum for the $i$th sample, $i=1,2,...k$

- sums of the ranks of the $k$ samples, are used to compare the distributions

- > $$
  > \LARGE{
  > \text {test statistic: } H = \frac{12}{n(n+1)}\Sigma \frac{T^2_i}{n_i}-3(n+1)
  > }
  > $$

##### Hypothesis

- $H_0$: the $k$ pop. dist. are identical
  - if $H_0$ = true, the test stat $H$ has an approx chi-square dist. with $df=k-1$.
    use right-tailed rejection region, or p-value based on chi-square distribution
    $k$ is the number of sample, e.g. 4 in 4 students of 5 marks each.
- $H_a$: at least two of the $k$ dist. differ
- Rejection:  if $H \geq \text{chi-square distribution}$, reject $H_0$

#### Friedman Test

- nonparametric, rank-equivalent of randomized block design, $k$ treatment, $b$ blocks
- used to compare three or more matched groups.
- <u>aka Friedman's two-way ANOVA</u>, although it is really a one-way ANOVA. A true nonparametric two-way ANOVA does not exist.
- all $k$ measurements within a block are ranked
- the sums of ranks of $k$ treatment, compared with $k$ treatment distribution
- $F_r$ is at minimum when rank sums are equal, that is $T_1=T_2=...=T_k$. Increase as rank sums increase.
- When either $k$ of treatments or $b$ of blocks is 5+, the $F$ approx. to chi-square distribution with $df=(k-1)$

##### How?

- rank $k$ measurements **within each block** from 1 to $k$.

  - E.g. 4 students 3 stimuli.
  - $b=\text {row}=\text {Students}=4$
  - $k=\text {column}=\text {Stimuli}=3$
  - we rank row by row (per student)
  - then we sum the ranks per column (per stimuli) to get $T_1, T_2, T_3$

- tied observations averaged

- Calculate $T_i$: rank sum for the $i$th sample, $i=1,2,...k$

- > $$
  > \LARGE{
  > F_r=\frac{12}{bk(k+1)}\Sigma T^2_i-3b(k+1)\\
  > }\\
  > \normalsize
  > {
  > \text {b = # of block}\\
  > \text {k = # of treatment}\\
  > }
  > $$

##### Hypothesis

- $H_0$: $k$ population treatment are identical
  - If $H_0$ is true, $F_r$ has approx chi-square dist. with $df=k-1$
    Use right-tailed rejection region, or p-value based chi-square distribution
- $H_a$: 2+ $k$ population differ
- Test stats: $F_r$
- Rejection:  if $F_r \geq \text{chi-square distribution}$, reject $H_0$

- Required: Chapter 15: Nonparametric Statistics
- Lab 6







# Statistics Stuff

|                                               |                                                              |
| --------------------------------------------- | ------------------------------------------------------------ |
| $\Large\mu$                                   | single population mean                                       |
| $\Large{(\mu_1-\mu_2)}$                       | difference between two population means                      |
| $\Large p$                                    | population parameter                                         |
| $\Large{\sigma^2}$                            | single population variance                                   |
| binomial variable                             | - made up of independent trials<br />- each trial can be classified as either success or failure<br />- fixed # of trials<br />- probability of success on each trial is constant |
| $\Large{\sigma=\sqrt{\frac{p(1-p)}{n}}}$      | Binomial distribution. SD. E.g. $\sqrt{(0.6)(0.4)}$          |
| $\Large{\sigma_1^2}$ and $\Large{\sigma_2^2}$ | comparison of two population variances                       |
| $\Large{\bar{x}}$                             | sample mean                                                  |
| $\Large\alpha$                                | significance level                                           |
| $\hat p$                                      | sample statistic                                             |
| $\LARGE{t=\frac{\bar{x}-\mu_0}{s\sqrt{n}}}$   | t-distribution                                               |

## What are Normal conditions?

Approximately normal if..
$$
\begin{aligned}
np\geq10\\
n(1-p)\geq10
\end{aligned}
$$

## 3. Mean, Median and Mode: Central Tendency

mean - good at measuring things that are normally distributed. Meaning roughly the same amount of data on either side of the middle, and has the most common values around the middle of the data.

outlier - give unusually large or small values, the outliers, less influence on our measures, we can use median

mode - the most fashionable. Most useful when there's a relatively large sample, so that you have large number of the popular values. Or used on data that isn't numerical.

Median == mean ? distribution is symmetric, that there's equal amount of data on either side of the median, and equal amounts on either side of the mean : the distribution is skewed, where mode is highest point, median then mean.

## 4. Measures of Spread

tells us how data spread in the middle, and trust conclusions based on mean and median.

**Inner Quartile - IQR** looks at the spread of the middle 50% of the data, gives us a better idea of the core. Q3 - Q1

#### **Variance** 

- tells us how spread out the whole data set is
- $deviation = x_i - \bar{x} $
  - $x_i$ = the value of one observation
  - $\bar{x}$ = mean of values of all observation
  - the distance of a data point away from the mean.
- $(\Sigma deviation^2) / (n-1)$
- If used n, it will be consistently be smaller - biased. Therefore we take n-1. 
- The mean is already one observation, so we have to take it out
  - check https://youtu.be/R4yfNi_8Kqw?t=335)

#### SD

- is $\sqrt{variance}$
- the *average* amount we expect a point to differ (deviate) from the mean
- low SD = less spread
- high SD = more spread

**Deviation** the difference from data point to the mean

## Data Visualization Part 1

- categorical data doesn't have a meaningful order
- simplest way is to put them into frequency table, then change it to probability
- 

## Correlation != Causation

##### Regression Coefficient

Regression line allows us to pretty accurately predict one variable based on the value of another

- a non-zero slope
- a sign there's some kind of relationship
- but we don't know how strong that relationship
- positive = line goes up
  - most dots at bottom left and top right
- negative = line goes down
  - most dots at top left and bottom right

##### $r$ - correlation coefficient

- measures the way **two variables move together**, both the **direction** and **closeness** of their movement
- Units can affect the calculation of correlation 
- So we use the standard deviation $\sigma$ to scale our correlation, so that its always between -1 and 1. 
- r = 1 goes up
- r = -1 goes down
- as r goes to 0, the dots are more spread out around our regression line, eventually no linear relationship at all

##### $r^2$ 

- always between $0$ and $1$
- tells us how much of the **variance** in one variable is **predicted** by the other.
- how well we can predict one variable if we know the other
- how accurate your guesses will be
- e.g. 0.7 = 70%, 1 = perfectly

##### Correlation != Causation, there can be...

1. A causes B
2. B causes A
3. Variable C causes A and B
   - e.g. high AC sales, and drownings due to variable C, heat.
4. No relationship at all.

##### Chi-Square

$$
\text{statistical test}:\frac{\text{observed difference - what we expect if the null is true}}{\text{average variation}}
$$

- for categorical, frequencies
- 

##### Regression

if f(x) = regression line, g(x) = mean-line, point(x) = each individual point

- total sum of squares = point(x_n) - g(x_n)
  - $SST=SSR+SSE$
  - represents: all the info we have from our data on youtube likes
- sum of square for regression  = f(x_n) - g(x_n)
  - $\text{DATA}=MODEL+ERROR$
  - represents: the proportion of the info that we can explain using the model we've created.
  - $DF=n-1$ one used 
- sums of squares for error = point(x_n) - f(x_n)
  - $\Large{\text{F-STATS}: \frac{\frac{SSR}{DF_R}}{\frac{SSE}{DF_E}}}$
  - represents: the leftover info; the portion of total sums of squares the model cannot explain
  - $DF = n - 2$ - one used for y-intercept, and one used for regression coefficient

##### T-Test

##### F-STATISTICS

$$
\Large{F=\frac{\frac{SS_{variable}}{df_{variable}}}{\frac{SS_{error}}{df_{error}}}=\frac{MS_{variable}}{MS_{error}}}
$$

##### ANOVA

Analysis of Variance

## Hypothesis

#### [Dave Your Tutor](https://www.youtube.com/channel/UCUUfJ6KNPvBiJwtLBX1jGSw)'s explanation

- e.g. Sam claims his bowling average is 150+
- you play 3 games with him, and his average of those 3 games is 40. 
- your cutoff score for sam 120
  - if its above 120+ you'll believe
  - if its under 120, you'll believe he's a liar
- There are 2 claims
  - sam's bowling avg is < 150
  - sam's bowling avg is 150+
- We write down two claims
  - $\mu < 150$ - his bowling avg is < 150
    - without the equal sign is Alternate Hypothesis
  - $\mu \geq 150$ - his bowling avg is 150+
    - with the equal sign is Null Hypothesis
  - $\mu$ is the <u>population means</u> but we will never know since we haven't seen everything game that he has played.
- Convention in Stats
  - $H_0$: $\mu\geq 150$
  - $H_1$:  $\mu < 150$
  - Simple Rules
    - Always write $H_0$ and $H_1$. Make sure $H_0$ is on top of $H_1$
    - $H_0$ is the claim with the equal sign
    - $H_1$ is the other claim

#### Khan's explanation

Example 1

- a machine is designed to dispense 530ml of liquid on medium size setting
- owner suspects that the machine may be dispensing too much in medium drinks
- decided to take a sample of 30 medium drinks to see if the average amount is significant greater than 530ml

> $H_0$
>
> - things a that are happening as expected; **no difference** hypothesis; something people have been assuming all along.
> - the statement that nothing is going on, that there is no effect, “nothin’ to see here. Move along, folks!” It is an equation, saying that *p*, the proportion in the population (which you don’t know), equals some number.

> $H_a$
>
> - if there's evidence to back up the claim, it would be new news.
> - the statement that something is going on, that there is an effect. It is an inequality, saying that *p* is different from the number mentioned in H0.
>
> $$
> \Large{H_0:\mu=530ml\\
> H_a:\mu>530ml}\\
> \text{where $\mu$ is the average amount of liquid dispensed on this setting}
> $$

Example 2

- National Sleep Foundation recommends teens age 14-17  get at least 8 hours of sleep per night for proper health
- stats class suspects that students at school are getting less than 8hrs on average
- to test their theory, random sample 42 of these students and ask how many hrs of sleep they get per night
- the mean from the sample is $\bar{x}=7.5hours$

$$
H_0:\mu\geq8\text{ hours}\\
H_a:\mu<8\text{ hours}
$$

### Hypothesis Testing (detailed)

[Example 1](https://www.khanacademy.org/math/ap-statistics/tests-significance-ap/idea-significance-tests/v/p-values-and-significance-tests)

Scenario: I have a website that is white and visitors have been spending 20 minutes in it. I like to change it to yellow to increase visotors.

1. $H_0:\mu=20\text{ minutes}$ after change
   $H_a:\mu>20\text{ minutes}$ after change

2. Significant level: $\alpha=0.05$

3. Take sample: $n=100, \bar{x}=25\text{ minutes}, S$

4. p-value: $\large{p(\bar x\geq25 \text{ minutes}|H_0 \text{ true})}$

   > - p-value is: **the probability we got the result for our sample, given that null is true**.
   > - if we <u>assume the null hypothesis is true</u>, what is the probability that we got the result we did for our sample.
   > - So a p value of .02 means that if the null hypothesis were true, a sample result this extreme would occur only 2% of the time.

5. p-value: if p-value: 0.03, then **p-value < $\alpha$ therefore reject $H_0$**
   **p**-value: if **p-value $\geq\alpha$ then do not reject $H_0$**

   - if the probability is **lower than the threshold** we've set, then we **reject**  $H_0$

     > - In plain text: if probability of something *else* happens given $H_0$ is higher than threshold $\alpha$, then we can't meet threshold to say $H_0$ won't happen. Hence fail to reject $H_0$.

> **note**: use $P$ which means proportion, for percentage values.

### P-values

P-value = $\Large{P(\hat p\geq20\%|H_0 \text{ true})}$ 

- = the probability that your sample proportion is going to be >= 20%, given the null hypothesis is true 
- = if you assume the null hypothesis is true, that the school is really 6% veggie, yet when we took a sample of 25 students and we got 20%, what is the probability of getting 20%+ for this to happen?
- if we are given a sampling distribution, we just have to count the amount of records over 20% / all records.
  - sampling distribution through simulation: 40 samples of $n=25$ students from a large population where 6% of the students were vegetarian

Q: What's the approximate p-value of the test?

A: Given that we are looking for $\geq20%$, we count how many dots that are over 0.20%. Which is 3 out of 40. Therefore $\approx0.075$.

### Type 1 and 2 errors

- We start with Null and Alternative Hypothesis

  - something as usual, and hey something is different

- P-value is the probability of getting a statistic at least as extreme as the one we observed when the null hypothesis is true.

- > $P(\text{statistics}|H_0 \text{ true})$

- > $\text{p-value} < \alpha \Rightarrow \text{reject} H_0$
  >
  > $\text{p-value} \geq \alpha \Rightarrow \text {fail to reject} H_0$

- if we are wrong in reject or fail to reject, is when Type I and II errors comes to place

|                      |                       real $H_0$ true                        |                       real $H_0$ false                       |
| -------------------- | :----------------------------------------------------------: | :----------------------------------------------------------: |
| fail to reject $H_0$ |                        true positive                         | Type 2 error<br />Didn't reject $H_0$ but should've<br />false negative |
| reject $H_0$         | Type 1 error<br />rejected $H_0$ but $H_0$ is true<br />false positive |                        true negative                         |

**Example**: Poll shows an unemployement rate of 9%, and mayor wants to know whether this is true for her town. She plans to take a sample to see whether there's a significant difference than 9% in her town. Which condition would the mayor commit a Type I error?

1. <u>She concludes the unemployment is not 9% when it actually is</u>
2. She concludes the unemployment is not 9% when it actually is not.
3. She concludes the unemployment is 9% when it actually is
4. She concludes the unemployment is 9% when it actually is not

#### Type 1 error (false positive)

- **meaning**: it is to reject $H_0$ when $H_0$ is true.
- We always assume $H_0$ to be true by default
- **explanation**: If we get a result that is so rare for $H_0$, lower than usual threshold of 0.05, a Type 1 error occurs.

### Power in Significance Test





# Quick notes

deep into data analytics



machinelearningplus.com/machine-learning/complete-introduction-linear-regression-r

https://www.machinelearningplus.com/machine-learning/complete-introduction-linear-regression-r/





z score

t-test

f-table

wilcoxon, rank-sum table

non-parametric

chi-square

# Definition

- **experimental unit**: the object which measurements is taken from
- **factor**: an independent variable who's values are controlled/changed by experimenter
- **level**: the intensity setting of *factor*
- **treatment**: a specific combo of *factor levels*
- **response**: the variable measured by experimenter
- **random assignment**: a fixed number chosen for treatment, and each is assigned a random number
- **variance**: the average squared deviation (difference) of a data point from the distribution mean
  - the distance of each data point from the mean, squared the distance, add them together, and find the average
  - sample variance: we divide by  $n-1$
- **sum of squares (SS)**: variance without the last average part; a foundation of ANOVA
  - the distance of each data point from the mean, squared the distance, add them together
- **SST**: Total/overall sum of squares; 
  - $SST=SSC+SSE$ - one way
  - $SST = SSC+SSB+SSE$ - two way
  - each <u>individual data point</u>, against the <u>grand mean</u>
- **SSC**: column/between/treatment sum of squares
  - each <u>individual column mean</u> against the <u>grand mean</u>.
- **SSE**: within/error sum of squares
  - the *within* sum of squares
  - each <u>individual data point</u>, against the <u>column mean</u>.
- SSB: block sound of squares
  - eats up some of SSE's errors; since SSC compares with SSE; hence the smaller SSE is, the better.
  - each <u>individual row mean</u> against the <u>grand mean</u>.
  - see ANOVA table above

































# 