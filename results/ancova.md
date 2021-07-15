ANCOVA test for `pos.score`\~`pre.score`+`scenario`\*`social`
================
Geiser C. Challco <geiser@alumni.usp.br>

-   [Initial Variables and Data](#initial-variables-and-data)
    -   [Descriptive statistics of initial
        data](#descriptive-statistics-of-initial-data)
-   [Checking of Assumptions](#checking-of-assumptions)
    -   [Assumption: Symmetry and treatment of
        outliers](#assumption-symmetry-and-treatment-of-outliers)
    -   [Assumption: Normality distribution of
        data](#assumption-normality-distribution-of-data)
    -   [Assumption: Linearity of dependent variables and covariate
        variable](#assumption-linearity-of-dependent-variables-and-covariate-variable)
    -   [Assumption: Homogeneity of data
        distribution](#assumption-homogeneity-of-data-distribution)
-   [Saving the Data with Normal Distribution Used for Performing ANCOVA
    test](#saving-the-data-with-normal-distribution-used-for-performing-ancova-test)
-   [Computation of ANCOVA test and Pairwise
    Comparison](#computation-of-ancova-test-and-pairwise-comparison)
    -   [ANCOVA test](#ancova-test)
    -   [Pairwise comparison](#pairwise-comparison)
    -   [Descriptive Statistic of Estimated Marginal
        Means](#descriptive-statistic-of-estimated-marginal-means)
    -   [Ancova plots for the dependent variable
        “pos.score”](#ancova-plots-for-the-dependent-variable-pos.score)
    -   [Textual Report](#textual-report)
-   [Tips and References](#tips-and-references)

## Initial Variables and Data

-   R-script file: [../code/ancova.R](../code/ancova.R)
-   Initial table file:
    [../data/initial-table.csv](../data/initial-table.csv)
-   Data for pos.score
    [../data/table-for-pos.score.csv](../data/table-for-pos.score.csv)
-   Table without outliers and normal distribution of data:
    [../data/table-with-normal-distribution.csv](../data/table-with-normal-distribution.csv)
-   Other data files: [../data/](../data/)
-   Files related to the presented results: [../results/](../results/)

### Descriptive statistics of initial data

| scenario     | social | variable  |   n |  mean | median | min | max |    sd |    se |    ci | iqr | symmetry | skewness | kurtosis |
|:-------------|:-------|:----------|----:|------:|-------:|----:|----:|------:|------:|------:|----:|:---------|---------:|---------:|
| gamified     | lower  | pos.score |   8 | 8.875 |      9 |   7 |  10 | 1.126 | 0.398 | 0.941 | 2.0 | YES      |   -0.320 |   -1.574 |
| gamified     | upper  | pos.score |   8 | 9.000 |      9 |   8 |  10 | 0.756 | 0.267 | 0.632 | 0.5 | few data |    0.000 |    0.000 |
| non.gamified | lower  | pos.score |   7 | 7.286 |      7 |   6 |   8 | 0.756 | 0.286 | 0.699 | 1.0 | few data |    0.000 |    0.000 |
| non.gamified | upper  | pos.score |   7 | 7.000 |      7 |   5 |   8 | 1.155 | 0.436 | 1.068 | 1.5 | NO       |   -0.557 |   -1.393 |
| NA           | NA     | pos.score |  30 | 8.100 |      8 |   5 |  10 | 1.296 | 0.237 | 0.484 | 2.0 | YES      |   -0.270 |   -0.528 |

![](/home/rstudio/report/ancova/ad5c4ed0b7cf3764/results/ancova_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

## Checking of Assumptions

### Assumption: Symmetry and treatment of outliers

#### Applying transformation for skewness data when normality is not achieved

#### Dealing with outliers (performing treatment of outliers)

### Assumption: Normality distribution of data

#### Removing data that affect normality (extreme values)

``` r
non.normal <- list(

)
sdat <- removeFromDataTable(rdat, non.normal, wid)
```

#### Result of normality test in the residual model

|           | var       |   n | skewness | kurtosis | symmetry | statistic | method       |     p | p.signif | normality |
|:----------|:----------|----:|---------:|---------:|:---------|----------:|:-------------|------:|:---------|:----------|
| pos.score | pos.score |  30 |    0.324 |   -1.023 | YES      |     0.949 | Shapiro-Wilk | 0.154 | ns       | YES       |

#### Result of normality test in each group

This is an optional validation and only valid for groups with number
greater than 30 observations

| scenario     | social | variable  |   n |  mean | median | min | max |    sd |    se |    ci | iqr | normality | method       | statistic |     p | p.signif |
|:-------------|:-------|:----------|----:|------:|-------:|----:|----:|------:|------:|------:|----:|:----------|:-------------|----------:|------:|:---------|
| gamified     | lower  | pos.score |   8 | 8.875 |      9 |   7 |  10 | 1.126 | 0.398 | 0.941 | 2.0 | YES       | Shapiro-Wilk |     0.882 | 0.197 | ns       |
| gamified     | upper  | pos.score |   8 | 9.000 |      9 |   8 |  10 | 0.756 | 0.267 | 0.632 | 0.5 | NO        | NA           |        NA | 1.000 | NA       |
| non.gamified | lower  | pos.score |   7 | 7.286 |      7 |   6 |   8 | 0.756 | 0.286 | 0.699 | 1.0 | NO        | NA           |        NA | 1.000 | NA       |
| non.gamified | upper  | pos.score |   7 | 7.000 |      7 |   5 |   8 | 1.155 | 0.436 | 1.068 | 1.5 | YES       | Shapiro-Wilk |     0.856 | 0.139 | ns       |

**Observation**:

As sample sizes increase, parametric tests remain valid even with the
violation of normality \[[1](#references)\]. According to the central
limit theorem, the sampling distribution tends to be normal if the
sample is large, more than (`n > 30`) observations. Therefore, we
performed parametric tests with large samples as described as follows:

-   In cases with the sample size greater than 100 (`n > 100`), we
    adopted a significance level of `p < 0.01`

-   For samples with `n > 50` observation, we adopted D’Agostino-Pearson
    test that offers better accuracy for larger samples
    \[[2](#references)\].

-   For samples’ size between `n > 100` and `n <= 200`, we ignored the
    normality test, and our decision of validating normality was based
    only in the interpretation of QQ-plots and histograms because the
    Shapiro-Wilk and D’Agostino-Pearson tests tend to be too sensitive
    with values greater than 200 observation \[[3](#references)\].

-   For samples with `n > 200` observation, we ignore the normality
    assumption based on the central theorem limit.

### Assumption: Linearity of dependent variables and covariate variable

``` r
ggscatter(sdat[["pos.score"]], x=covar, y="pos.score", facet.by=between, short.panel.labs = F) + 
 stat_smooth(method = "lm", span = 0.9)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](/home/rstudio/report/ancova/ad5c4ed0b7cf3764/results/ancova_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

### Assumption: Homogeneity of data distribution

|             | var       | method         | formula                      |   n | DFn.df1 | DFd.df2 | statistic |     p | p.signif |
|:------------|:----------|:---------------|:-----------------------------|----:|--------:|--------:|----------:|------:|:---------|
| pos.score.1 | pos.score | Levene’s test  | `.res`\~`scenario`\*`social` |  30 |       3 |      26 |     1.429 | 0.257 | ns       |
| pos.score.2 | pos.score | Anova’s slopes | `.res`\~`scenario`\*`social` |  30 |       3 |      22 |     0.157 | 0.924 | ns       |

## Saving the Data with Normal Distribution Used for Performing ANCOVA test

``` r
ndat <- sdat[[1]]
for (dv in names(sdat)[-1]) ndat <- merge(ndat, sdat[[dv]])
write.csv(ndat, paste0("../data/table-with-normal-distribution.csv"))
```

Descriptive statistics of data with normal distribution

|             | scenario     | social | variable  |   n |  mean | median | min | max |    sd |    se |    ci | iqr |
|:------------|:-------------|:-------|:----------|----:|------:|-------:|----:|----:|------:|------:|------:|----:|
| pos.score.1 | gamified     | lower  | pos.score |   8 | 8.875 |      9 |   7 |  10 | 1.126 | 0.398 | 0.941 | 2.0 |
| pos.score.2 | gamified     | upper  | pos.score |   8 | 9.000 |      9 |   8 |  10 | 0.756 | 0.267 | 0.632 | 0.5 |
| pos.score.3 | non.gamified | lower  | pos.score |   7 | 7.286 |      7 |   6 |   8 | 0.756 | 0.286 | 0.699 | 1.0 |
| pos.score.4 | non.gamified | upper  | pos.score |   7 | 7.000 |      7 |   5 |   8 | 1.155 | 0.436 | 1.068 | 1.5 |

![](/home/rstudio/report/ancova/ad5c4ed0b7cf3764/results/ancova_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

## Computation of ANCOVA test and Pairwise Comparison

### ANCOVA test

| var       | Effect          | DFn | DFd |    SSn |    SSd |      F |     p |   ges | p.signif |
|:----------|:----------------|----:|----:|-------:|-------:|-------:|------:|------:|:---------|
| pos.score | pre.score       |   1 |  25 | 13.903 | 10.401 | 33.417 | 0.000 | 0.572 | \*\*\*\* |
| pos.score | scenario        |   1 |  25 | 22.578 | 10.401 | 54.269 | 0.000 | 0.685 | \*\*\*\* |
| pos.score | social          |   1 |  25 |  0.442 | 10.401 |  1.063 | 0.312 | 0.041 | ns       |
| pos.score | scenario:social |   1 |  25 |  0.014 | 10.401 |  0.033 | 0.857 | 0.001 | ns       |

### Pairwise comparison

| var       | scenario     | social | group1   | group2       | estimate | conf.low | conf.high |    se | statistic |     p | p.adj | p.adj.signif |
|:----------|:-------------|:-------|:---------|:-------------|---------:|---------:|----------:|------:|----------:|------:|------:|:-------------|
| pos.score | NA           | lower  | gamified | non.gamified |    1.784 |    1.093 |     2.475 | 0.336 |     5.316 | 0.000 | 0.000 | \*\*\*\*     |
| pos.score | NA           | upper  | gamified | non.gamified |    1.696 |    1.000 |     2.392 | 0.338 |     5.019 | 0.000 | 0.000 | \*\*\*\*     |
| pos.score | gamified     | NA     | lower    | upper        |   -0.210 |   -0.875 |     0.455 | 0.323 |    -0.651 | 0.521 | 0.521 | ns           |
| pos.score | non.gamified | NA     | lower    | upper        |   -0.298 |   -1.037 |     0.442 | 0.359 |    -0.828 | 0.415 | 0.415 | ns           |

### Descriptive Statistic of Estimated Marginal Means

| var       | scenario     | social |   n | emmean |  mean | conf.low | conf.high |    sd | sd.emms | se.emms |
|:----------|:-------------|:-------|----:|-------:|------:|---------:|----------:|------:|--------:|--------:|
| pos.score | gamified     | lower  |   8 |  8.807 | 8.875 |    8.337 |     9.277 | 1.126 |   0.646 |   0.228 |
| pos.score | gamified     | upper  |   8 |  9.017 | 9.000 |    8.547 |     9.487 | 0.756 |   0.645 |   0.228 |
| pos.score | non.gamified | lower  |   7 |  7.023 | 7.286 |    6.512 |     7.534 | 0.756 |   0.656 |   0.248 |
| pos.score | non.gamified | upper  |   7 |  7.321 | 7.000 |    6.806 |     7.836 | 1.155 |   0.662 |   0.250 |

### Ancova plots for the dependent variable “pos.score”

``` r
plots <- twoWayAncovaPlots(sdat[["pos.score"]], "pos.score", between
, aov[["pos.score"]], pwc[["pos.score"]], addParam = c("jitter"), font.label.size=16, step.increase=0.25)
```

#### Plot for: `pos.score` \~ `scenario`

``` r
plots[["scenario"]]
```

![](/home/rstudio/report/ancova/ad5c4ed0b7cf3764/results/ancova_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->

#### Plot for: `pos.score` \~ `social`

``` r
plots[["social"]]
```

![](/home/rstudio/report/ancova/ad5c4ed0b7cf3764/results/ancova_files/figure-gfm/unnamed-chunk-26-1.png)<!-- -->

### Textual Report

After controlling the linearity of covariance “pre.score”, ANCOVA tests
with independent between-subjects variables “scenario” (gamified,
non.gamified) and “social” (upper, lower) were performed to determine
statistically significant difference on the dependent varibles
“pos.score”. For the dependent variable “pos.score”, there was
statistically significant effects in the factor “pre.score” with
F(1,25)=33.417, p&lt;0.001 and ges=0.572 (effect size) and in the factor
“scenario” with F(1,25)=54.269, p&lt;0.001 and ges=0.685 (effect size).

Pairwise comparisons using the Estimated Marginal Means (EMMs) were
computed to find statistically significant diferences among the groups
defined by the independent variables, and with the p-values ajusted by
the method “bonferroni”. For the dependent variable “pos.score”, the
mean in the scenario=“gamified” (adj M=8.807 and SD=1.126) was
significantly different than the mean in the scenario=“non.gamified”
(adj M=7.023 and SD=0.756) with p-adj&lt;0.001; the mean in the
scenario=“gamified” (adj M=9.017 and SD=0.756) was significantly
different than the mean in the scenario=“non.gamified” (adj M=7.321 and
SD=1.155) with p-adj&lt;0.001.

## Tips and References

-   Use the site <https://www.tablesgenerator.com> to convert the HTML
    tables into Latex format

-   \[2\]: Miot, H. A. (2017). Assessing normality of data in clinical
    and experimental trials. J Vasc Bras, 16(2), 88-91.

-   \[3\]: Bárány, Imre; Vu, Van (2007). “Central limit theorems for
    Gaussian polytopes”. Annals of Probability. Institute of
    Mathematical Statistics. 35 (4): 1593–1621.
