# Posterior samples from a logistic model


```r
library(brms)
```

```
## Warning: package 'brms' was built under R version 4.2.3
```

```
## Loading required package: Rcpp
```

```
## Warning: package 'Rcpp' was built under R version 4.2.3
```

```
## Loading 'brms' package (version 2.19.0). Useful instructions
## can be found by typing help('brms'). A more detailed introduction
## to the package is available through vignette('brms_overview').
```

```
## 
## Attaching package: 'brms'
```

```
## The following object is masked from 'package:stats':
## 
##     ar
```

```r
set.seed(456)
N <- 50
Ni <- 10
x <- rnorm(N)
id <- sample(Ni,N,replace=TRUE)
id.f <- factor(id)
id.vals <- rnorm(Ni)
lin.y <- x + id.vals[id]
p.y <- plogis(lin.y)
y <- rbinom(N,1,p.y)

# 1. A model without random effect

# 2. A model with random effect
```
