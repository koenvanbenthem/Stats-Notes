# When not to use a PCA
The following is a brief example of when using a PCA to replace variables in your dataset might not be ideal. 

Firstly, let's create some dummy explanatory data. We will generate four predictors, with negative correlations between the first and second and between the third and fourth:

``` r
set.seed(3)
library(mvtnorm)
x <- rmvnorm(50,mean=rep(0,4),
        sigma=matrix(c(1,-0.8,0,0,
                       -0.8,1,0,0,
                       0,0,1,-0.8,
                       0,0,-0.8,1),
                     nrow=4,byrow=TRUE))
```
When we now try to calculate the principal components, we find;

``` r
pcs <- prcomp(x)
summary(pcs)
```

```
## Importance of components:
##                           PC1    PC2     PC3     PC4
## Standard deviation     1.5386 1.2615 0.41082 0.38645
## Proportion of Variance 0.5535 0.3721 0.03946 0.03492
## Cumulative Proportion  0.5535 0.9256 0.96508 1.00000
```
Hence, the first two axes together already explain over 90% of the data. We could thus decide in a predictive model to only use these two axes. 

``` r
newx <- predict(pcs)
```
Let's next make a response variable, which is equal to the sum of the 4 explanatory variable with some added noise on top:

``` r
y <- apply(x,1,sum) + rnorm(nrow(x),0,0.5)
d <- as.data.frame(cbind(y=y,newx))
head(d)
```

```
##            y        PC1        PC2        PC3         PC4
## 1 -1.6267028 -1.5775951 -0.2800516 -0.3727663 -0.35502422
## 2  0.7772536  0.5864081  0.8836272  0.3464420 -0.04036141
## 3 -0.2745109 -2.1333463  1.5687629 -0.7284356  0.21787624
## 4 -1.0983262 -1.1603281  0.5778170 -0.1408519 -0.13538958
## 5 -0.3078073 -1.0937142 -0.1890704  0.3171644 -0.64608988
## 6 -0.7859098 -0.8988454 -1.0151972 -0.6220823 -0.40436840
```
We can use dredge (should we?) to compare all models:

``` r
library(MuMIn)
```

```
## Warning: package 'MuMIn' was built under R version 4.2.1
```

``` r
m.1 <- lm(y~.,data=d,na.action="na.fail")
modSel <- as.data.frame(dredge(m.1))
```

```
## Fixed term is "(Intercept)"
```

``` r
options(digits=3)
#best models
head(modSel,3)
```

```
##    (Intercept)     PC1    PC2  PC3  PC4 df logLik AICc delta weight
## 13      0.0899      NA     NA 1.74 1.14  4  -40.3 89.4 0.000  0.355
## 15      0.0899      NA 0.0816 1.74 1.14  5  -39.4 90.1 0.670  0.254
## 14      0.0899 -0.0634     NA 1.74 1.14  5  -39.5 90.3 0.856  0.231
```

``` r
# worst models
tail(modSel,3)
```

```
##   (Intercept)     PC1    PC2 PC3 PC4 df logLik AICc delta   weight
## 3      0.0899      NA 0.0816  NA  NA  3  -70.4  147  57.8 9.88e-14
## 2      0.0899 -0.0634     NA  NA  NA  3  -70.4  147  57.9 9.62e-14
## 4      0.0899 -0.0634 0.0816  NA  NA  4  -70.1  149  59.7 3.85e-14
```
This is an interesting finding: the model that performs best is the model that contains the two axes that explain the least amount of variation, while the model that explains worst is the model that contains the two axes that explain the most variation. We can look at this sligthly more indepth:

``` r
m.12 <- lm(y~PC1+PC2,data=d)
summary(m.12)
```

```
## 
## Call:
## lm(formula = y ~ PC1 + PC2, data = d)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -2.1006 -0.6951 -0.0293  0.7041  2.0347 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)
## (Intercept)   0.0899     0.1435    0.63     0.53
## PC1          -0.0634     0.0942   -0.67     0.50
## PC2           0.0816     0.1149    0.71     0.48
## 
## Residual standard error: 1.01 on 47 degrees of freedom
## Multiple R-squared:  0.02,	Adjusted R-squared:  -0.0217 
## F-statistic: 0.479 on 2 and 47 DF,  p-value: 0.622
```

``` r
m.34 <- lm(y~PC3+PC4,data=d)
summary(m.34)
```

```
## 
## Call:
## lm(formula = y ~ PC3 + PC4, data = d)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -1.1886 -0.3907 -0.0126  0.3651  1.2402 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)   0.0899     0.0790    1.14     0.26    
## PC3           1.7429     0.1942    8.98  9.3e-12 ***
## PC4           1.1439     0.2064    5.54  1.3e-06 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.558 on 47 degrees of freedom
## Multiple R-squared:  0.703,	Adjusted R-squared:  0.69 
## F-statistic: 55.6 on 2 and 47 DF,  p-value: 4.05e-13
```
Hence, we might conclude that at least in this case using a PCA to reduce variables and then fit would not be ideal. That is not to say that it is never a useful approach, I just think that one should be cautious.
