# DuncanAnalysisAndVisual
The carData R package provides various dataset in particular I will analyse the Duncan Dataset, which contain data on the prestige and other characteristics of 45 U. S. occupations in 1950.

## Installation

```R
install.packages("carData")
install.packages("car")
library(carData)
library(car)
```

## Usage:

Visualize the first 6 rows of the data just uploaded


```R
data(Duncan)
head(Duncan)
```


<table class="dataframe">
<caption>A data.frame: 6 × 4</caption>
<thead>
	<tr><th></th><th scope=col>type</th><th scope=col>income</th><th scope=col>education</th><th scope=col>prestige</th></tr>
	<tr><th></th><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><th scope=row>accountant</th><td>prof</td><td>62</td><td>86</td><td>82</td></tr>
	<tr><th scope=row>pilot</th><td>prof</td><td>72</td><td>76</td><td>83</td></tr>
	<tr><th scope=row>architect</th><td>prof</td><td>75</td><td>92</td><td>90</td></tr>
	<tr><th scope=row>author</th><td>prof</td><td>55</td><td>90</td><td>76</td></tr>
	<tr><th scope=row>chemist</th><td>prof</td><td>64</td><td>86</td><td>90</td></tr>
	<tr><th scope=row>minister</th><td>prof</td><td>21</td><td>84</td><td>87</td></tr>
</tbody>
</table>





Every row is an observation, the first column correspond to a profession, the second is the income and it is the percetuage of people that gain 3500$ or more in a year for the determined profession, the education represent the precetuage of people doing that word with an high academy grade and the prestige is the precentuage of peolpe that in a survey said that the profession were great  


Generic information divide for column:


```R
summary(Duncan)
str(Duncan)
```


       type        income        education         prestige    
     bc  :21   Min.   : 7.00   Min.   :  7.00   Min.   : 3.00  
     prof:18   1st Qu.:21.00   1st Qu.: 26.00   1st Qu.:16.00  
     wc  : 6   Median :42.00   Median : 45.00   Median :41.00  
               Mean   :41.87   Mean   : 52.56   Mean   :47.69  
               3rd Qu.:64.00   3rd Qu.: 84.00   3rd Qu.:81.00  
               Max.   :81.00   Max.   :100.00   Max.   :97.00  


    'data.frame':	45 obs. of  4 variables:
     $ type     : Factor w/ 3 levels "bc","prof","wc": 2 2 2 2 2 2 2 2 3 2 ...
     $ income   : int  62 72 75 55 64 21 64 80 67 72 ...
     $ education: int  86 76 92 90 86 84 93 100 87 86 ...
     $ prestige : int  82 83 90 76 90 87 93 90 52 88 ...
    

We examine the distributions of the prestige, education and income, along with the relationship between the three


```R
scatterplotMatrix ( ~ prestige + education + income, id=list (n=3), data=Duncan)
```


![png](images/output_10_0.png)


Like prestige, education appears to have a bimodal distribution. The distribution of income, in contrast, is perhaps best characterized as irregular. The pairwise relationships among the variables seem reasonably linear, which means that as we move from left to right across the plot, the average y values of the points more or less trace out a straight line. The scatter around the regression lines appears to have reasonably constant vertical variability and to be approximately
symmetric.
In addition, two or three cases stand out from the others. In the scatterplot of income versus education, ministers are unusual in combining relatively low income with a relatively high level of education, and railroad conductors and engineers are unusual in combining relatively high levels of income with relatively low education. Because education and income are predictors in Duncan’s regression, these three occupations will have relatively high leverage on the regression coefficients. None of these cases, however, are outliers in the univariate distributions of the three variables.

## Regession Model

We load and analise the linear model to study prestige using income and education as predictor:


```R
modello<-lm(Duncan$prestige ~ Duncan$income + Duncan$education)
summary(modello)
```


    
    Call:
    lm(formula = Duncan$prestige ~ Duncan$income + Duncan$education)
    
    Residuals:
        Min      1Q  Median      3Q     Max 
    -29.538  -6.417   0.655   6.605  34.641 
    
    Coefficients:
                     Estimate Std. Error t value Pr(>|t|)    
    (Intercept)      -6.06466    4.27194  -1.420    0.163    
    Duncan$income     0.59873    0.11967   5.003 1.05e-05 ***
    Duncan$education  0.54583    0.09825   5.555 1.73e-06 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    
    Residual standard error: 13.37 on 42 degrees of freedom
    Multiple R-squared:  0.8282,	Adjusted R-squared:   0.82 
    F-statistic: 101.2 on 2 and 42 DF,  p-value: < 2.2e-16
    
## Diagnostic:
Calculate Studentized residuals for the model and plot the distribution
```R
residual<-rstudent(modello)
densityPlot(rstudent(modello))
```
![png](images/output_17_0.png)

We can see that the errors in the regression are nearly normally distributed with zero means and costant variance thanks to the fact theat the graph resemble a t-distribution.
The qqPlot () function extracts the Studentized residuals and plots them against the quantiles of the appropriate t-distribution. If the Studentized residuals are t-distributed, then the plotted points should lie close to a straight line.
```R
qqPlot(modello, id=list(n=3))
```
<ol class=list-inline>
	<li>6</li>
	<li>9</li>
	<li>17</li>
</ol>

![png](images/output_20_1.png)

In this case, the residuals pull away slightly from the comparison line at both ends, suggesting that the residual distribution is a bit heavy-tailed. This effect is more pronounced at the upper end of the distribution, indicating a slight positive skew. The residuals stay nearly within the boundaries of the envelope at both ends of the distribution, with the exception of the occupation minister.
To analise in a better way we use outlierTest(), that is atest based on the largest (absolute) Studentized residual, and it suggests that the residual for ministers is not terribly unusual, with a Bonferroni-corrected p-value of 0.14:
```R
outlierTest(modello)
```
    No Studentized residuals with Bonferroni p < 0.05
    Largest |rstudent|:
      rstudent unadjusted p-value Bonferroni p
    6 3.134519          0.0031772      0.14297
We proceed to check for high-leverage and influential cases by using the influenceIndexPlot () function from the car package to plot hat-values against the case indices. We ask to identify the three biggest values in each plot.
```R
influenceIndexPlot (modello, vars=c ("Cook", "hat"), id=list(n=3))
```
![png](images/output_24_0.png)

Because the cases in a regression can be jointly as well as individually influential, we also examine added-variable plots for the predictors, using the avPlots () function in the car package
```R
avPlots (modello, id=list (cex=0.75, n=3, method="mahal"))
```
![png](images/output_26_0.png)

method= "mahal" indicates that unusualness is quantified by Mahalanobis distance from the center of the point-cloud.
Mahalanobis distances from the center of the data take account of the standard deviations of the variables and the correlation between them. 
Each added-variable plot displays the conditional, rather than the marginal, relationship between the response and one of the predictors. Points at the extreme left or right of the plot correspond to cases that have high leverage on the corresponding coefficient and consequently are potentially influential. The graphs confirms and strengthens our previous observations: we should be concerned about the occupations minister and conductor, which work jointly to increase the education coefficient and decrease the income coefficient.
Occupation RR.engineer has relatively high leverage on these coefficients but is more in line with the rest of the data.
We next use the crPlots () function, also in the car package, to generate component-plus-residual plots for education and income
```R
crPlots(modello)
```
![png](images/output_29_0.png)

Each plot includes a least-squares line, representing the regression plane viewed edge-on in the direction of the corresponding predictor, and a loess nonparametric-regression smooth.51 The purpose of these plots is to detect nonlinearity, evidence of which is slight here.
Component-plus-residual plots for education and income in Duncan’s occupational-prestige regression. The solid line in each panel shows a loess nonparametric-regression smooth; the broken line in each panel is the least-squares line.
Using the ncvTest () function, we compute score tests for nonconstant variance, checking for an association of residual variability with the fitted values and with any linear combination of the predictors:
```R
ncvTest(modello)
ncvTest(modello,var.formula=~ Duncan$income+Duncan$education)
```
    Non-constant Variance Score Test 
    Variance formula: ~ fitted.values 
    Chisquare = 0.3810967, Df = 1, p = 0.53702
    Non-constant Variance Score Test 
    Variance formula: ~ Duncan$income + Duncan$education 
    Chisquare = 0.6976023, Df = 2, p = 0.70553
Both tests yield large p-values, indicating that the assumption of constant variance is tenable.
Finally, on the basis of the influential-data diagnostics, we try removing the cases minister and conductor from the regression:
```R
whichNames(c("minister", "conductor"), Duncan)
```
<dl class=dl-horizontal>
	<dt>minister</dt>
		<dd>6</dd>
	<dt>conductor</dt>
		<dd>16</dd>
</dl>
```R
modello2<-update(modello, subset=-c(6,16))
summary(modello2)
```
    
    Call:
    lm(formula = Duncan$prestige ~ Duncan$income + Duncan$education, 
        subset = -c(6, 16))
    
    Residuals:
        Min      1Q  Median      3Q     Max 
    -28.612  -5.898   1.937   5.616  21.551 
    
    Coefficients:
                     Estimate Std. Error t value Pr(>|t|)    
    (Intercept)      -6.40899    3.65263  -1.755   0.0870 .  
    Duncan$income     0.86740    0.12198   7.111 1.31e-08 ***
    Duncan$education  0.33224    0.09875   3.364   0.0017 ** 
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    
    Residual standard error: 11.42 on 40 degrees of freedom
    Multiple R-squared:  0.876,	Adjusted R-squared:  0.8698 
    F-statistic: 141.3 on 2 and 40 DF,  p-value: < 2.2e-16
    


The compareCoefs () function, also from the car package, is convenient for comparing the estimated coefficients and their standard errors across the two regressions fit to the data:


```R
compareCoefs(modello, modello2)
```

    Calls:
    1: lm(formula = Duncan$prestige ~ Duncan$income + Duncan$education)
    2: lm(formula = Duncan$prestige ~ Duncan$income + Duncan$education, subset =
       -c(6, 16))
    
                     Model 1 Model 2
    (Intercept)        -6.06   -6.41
    SE                  4.27    3.65
                                    
    Duncan$income      0.599   0.867
    SE                 0.120   0.122
                                    
    Duncan$education  0.5458  0.3322
    SE                0.0983  0.0987
                                    
    

The coefficients of education and income changed substantially with the deletion of the occupations minister and conductor. The education coefficient is considerably smaller and the income coefficient considerably larger than before.

## Model using also type as predictor:


```R
modello3<-lm(Duncan$prestige ~ Duncan$income + Duncan$education+Duncan$type)
summary(modello3)
```


    
    Call:
    lm(formula = Duncan$prestige ~ Duncan$income + Duncan$education + 
        Duncan$type)
    
    Residuals:
        Min      1Q  Median      3Q     Max 
    -14.890  -5.740  -1.754   5.442  28.972 
    
    Coefficients:
                      Estimate Std. Error t value Pr(>|t|)    
    (Intercept)       -0.18503    3.71377  -0.050  0.96051    
    Duncan$income      0.59755    0.08936   6.687 5.12e-08 ***
    Duncan$education   0.34532    0.11361   3.040  0.00416 ** 
    Duncan$typeprof   16.65751    6.99301   2.382  0.02206 *  
    Duncan$typewc    -14.66113    6.10877  -2.400  0.02114 *  
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    
    Residual standard error: 9.744 on 40 degrees of freedom
    Multiple R-squared:  0.9131,	Adjusted R-squared:  0.9044 
    F-statistic:   105 on 4 and 40 DF,  p-value: < 2.2e-16
    
## Redo Diagnostic Analysis:
Calculate Studentized residuals for the model and plot the distribution
```R
residual<-rstudent(modello3)
densityPlot(rstudent(modello3))
```
![png](images/output_43_0.png)

We can easily see that in this case the graph is nomore centered in zero, and the maximum value is negative, moreover the distribution does not resemble anymore a t-student, mostly on the left part of the curve.
```R
par(mfrow=c(1,2))
qqPlot(modello3, id=list(n=3))
qqPlot(modello, id=list(n=3))
```
<ol class=list-inline>
	<li>6</li>
	<li>28</li>
	<li>33</li>
</ol>
<ol class=list-inline>
	<li>6</li>
	<li>9</li>
	<li>17</li>
</ol>

![png](images/output_45_2.png)

```R
```
