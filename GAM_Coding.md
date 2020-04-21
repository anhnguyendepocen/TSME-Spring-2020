GAM Coding
================
Michael Dance
Spring 2020

First, we will simulate some data, using the `gamSim()` function in the
`mgcv` package. See [help
file](https://cran.r-project.org/web/packages/mgcv/mgcv.pdf) for
additional information on `mgcv` and `gamSim`. This code will simulate a
data set with 3 independent variables and 1000 observations. The `scale`
parameter controls the amount of “noise” in the data, so play around
with this and see how this affects the data. The `dist` parameter
controls the distribution of the data (normal, poisson, or binary). The
first command is the `eg` term which specifies 1 of 7 types of example
data formats that you can choose from.

``` r
library(mgcv)
```

    ## Loading required package: nlme

    ## This is mgcv 1.8-31. For overview type 'help("mgcv-package")'.

``` r
library(ggplot2)
set.seed(0)
dat <- gamSim(1,n=1000,scale=5,dist="normal", verbose=TRUE)
```

    ## Gu & Wahba 4 term additive model

``` r
head(dat)
```

    ##           y        x0        x1        x2        x3         f        f0
    ## 1  9.208620 0.8966972 0.2655367 0.3871130 0.5313889  6.986891 0.6377368
    ## 2  4.628387 0.2655087 0.5308088 0.8718050 0.8023495  4.568740 1.4814113
    ## 3  5.728797 0.3721239 0.6848609 0.9671970 0.4794952  5.775197 1.8407682
    ## 4  2.819287 0.5728534 0.3832834 0.8669163 0.1774016  4.331175 1.9478442
    ## 5 13.147123 0.9082078 0.9549880 0.4377153 0.3971333 10.685348 0.5687870
    ## 6  7.831545 0.2016819 0.1183566 0.1919378 0.8142270 10.845143 1.1841037
    ##         f1           f2 f3
    ## 1 1.700757 4.6483967648  0
    ## 2 2.891044 0.1962848079  0
    ## 3 3.934256 0.0001726522  0
    ## 4 2.152364 0.2309669356  0
    ## 5 6.752927 3.3636342078  0
    ## 6 1.267078 8.3939613564  0

``` r
####dat <- na.omit(dat)    If you have and NAs, you will want to remove
```

Now we can fit a GAM model to this data. In this first example, we will
use the most simple set up possible, using mostly defaults for basis
dimension (k = 10), smoothing function (`bs = "tp"`), and distribution
(`family = 'gaussian'`). Play around with different smoothers (e.g.
`"cr"`) and k values.

``` r
a<-gam(y~s(x1) + s(x2) + s(x3),data=dat)
summary(a)
```

    ## 
    ## Family: gaussian 
    ## Link function: identity 
    ## 
    ## Formula:
    ## y ~ s(x1) + s(x2) + s(x3)
    ## 
    ## Parametric coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)    8.223      0.152   54.09   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Approximate significance of smooth terms:
    ##         edf Ref.df      F p-value    
    ## s(x1) 2.519  3.135 49.801  <2e-16 ***
    ## s(x2) 7.182  8.218 47.401  <2e-16 ***
    ## s(x3) 1.907  2.372  0.721   0.618    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## R-sq.(adj) =  0.355   Deviance explained = 36.3%
    ## GCV = 23.402  Scale est. = 23.107    n = 1000

``` r
b<-gam(y~s(x1,bs='tp') + s(x2,bs='tp') + s(x3,bs='tp'),data=dat, family="gaussian")
summary(b)
```

    ## 
    ## Family: gaussian 
    ## Link function: identity 
    ## 
    ## Formula:
    ## y ~ s(x1, bs = "tp") + s(x2, bs = "tp") + s(x3, bs = "tp")
    ## 
    ## Parametric coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)    8.223      0.152   54.09   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Approximate significance of smooth terms:
    ##         edf Ref.df      F p-value    
    ## s(x1) 2.519  3.135 49.801  <2e-16 ***
    ## s(x2) 7.182  8.218 47.401  <2e-16 ***
    ## s(x3) 1.907  2.372  0.721   0.618    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## R-sq.(adj) =  0.355   Deviance explained = 36.3%
    ## GCV = 23.402  Scale est. = 23.107    n = 1000

Gam.check provides model diagnostics. Look closely at the `edf` values
for each variable. Do we need to increase the basis dimension?

``` r
gam.check(a)
```

![](GAM_Coding_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

    ## 
    ## Method: GCV   Optimizer: magic
    ## Smoothing parameter selection converged after 28 iterations.
    ## The RMS GCV score gradient at convergence was 4.176716e-05 .
    ## The Hessian was positive definite.
    ## Model rank =  28 / 28 
    ## 
    ## Basis dimension (k) checking results. Low p-value (k-index<1) may
    ## indicate that k is too low, especially if edf is close to k'.
    ## 
    ##         k'  edf k-index p-value  
    ## s(x1) 9.00 2.52    0.95    0.05 *
    ## s(x2) 9.00 7.18    1.01    0.65  
    ## s(x3) 9.00 1.91    0.99    0.46  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Finally, we can plot our response
plots.

``` r
plot(a)
```

![](GAM_Coding_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->![](GAM_Coding_files/figure-gfm/unnamed-chunk-4-2.png)<!-- -->![](GAM_Coding_files/figure-gfm/unnamed-chunk-4-3.png)<!-- -->

There is a new package available called `gratia` that plots gam objects
in a `ggplot` style
format.

``` r
plot(a)
```

![](GAM_Coding_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->![](GAM_Coding_files/figure-gfm/unnamed-chunk-5-2.png)<!-- -->![](GAM_Coding_files/figure-gfm/unnamed-chunk-5-3.png)<!-- -->

Now we can try using cubic regression splines and restricting
k.

``` r
c<-gam(y~s(x1,bs='cr', k=3) + s(x2,bs='cr',k=3) + s(x3,bs='cr',k=3),data=dat,method="REML", family="gaussian")
summary(c)
```

    ## 
    ## Family: gaussian 
    ## Link function: identity 
    ## 
    ## Formula:
    ## y ~ s(x1, bs = "cr", k = 3) + s(x2, bs = "cr", k = 3) + s(x3, 
    ##     bs = "cr", k = 3)
    ## 
    ## Parametric coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)   8.2225     0.1651    49.8   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Approximate significance of smooth terms:
    ##         edf Ref.df      F p-value    
    ## s(x1) 1.919  1.993 59.898  <2e-16 ***
    ## s(x2) 1.984  2.000 89.673  <2e-16 ***
    ## s(x3) 1.414  1.657  0.321    0.71    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## R-sq.(adj) =  0.239   Deviance explained = 24.3%
    ## -REML = 3074.3  Scale est. = 27.266    n = 1000

``` r
gam.check(c)
```

![](GAM_Coding_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

    ## 
    ## Method: REML   Optimizer: outer newton
    ## full convergence after 10 iterations.
    ## Gradient range [-0.0001473405,3.027461e-05]
    ## (score 3074.286 & scale 27.26649).
    ## Hessian positive definite, eigenvalue range [0.08579346,498.001].
    ## Model rank =  7 / 7 
    ## 
    ## Basis dimension (k) checking results. Low p-value (k-index<1) may
    ## indicate that k is too low, especially if edf is close to k'.
    ## 
    ##         k'  edf k-index p-value    
    ## s(x1) 2.00 1.92    0.95    0.03 *  
    ## s(x2) 2.00 1.98    0.86  <2e-16 ***
    ## s(x3) 2.00 1.41    0.98    0.30    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Now let’s revist the Nile time series data that we used in the first
ARMA coding exercise.

``` r
library(astsa)
Nile
```

    ## Time Series:
    ## Start = 1871 
    ## End = 1970 
    ## Frequency = 1 
    ##   [1] 1120 1160  963 1210 1160 1160  813 1230 1370 1140  995  935 1110  994 1020
    ##  [16]  960 1180  799  958 1140 1100 1210 1150 1250 1260 1220 1030 1100  774  840
    ##  [31]  874  694  940  833  701  916  692 1020 1050  969  831  726  456  824  702
    ##  [46] 1120 1100  832  764  821  768  845  864  862  698  845  744  796 1040  759
    ##  [61]  781  865  845  944  984  897  822 1010  771  676  649  846  812  742  801
    ##  [76] 1040  860  874  848  890  744  749  838 1050  918  986  797  923  975  815
    ##  [91] 1020  906  901 1170  912  746  919  718  714  740

``` r
class(Nile)
```

    ## [1] "ts"

``` r
Nile2<-data.frame(Flow=as.matrix(Nile), Year=time(Nile))
summary(Nile2)
```

    ##       Flow             Year     
    ##  Min.   : 456.0   Min.   :1871  
    ##  1st Qu.: 798.5   1st Qu.:1896  
    ##  Median : 893.5   Median :1920  
    ##  Mean   : 919.4   Mean   :1920  
    ##  3rd Qu.:1032.5   3rd Qu.:1945  
    ##  Max.   :1370.0   Max.   :1970

``` r
Gam<-gamm(Flow~s(Year,bs='cr',k=30), data=Nile2, family="gaussian")
summary(Gam$gam)
```

    ## 
    ## Family: gaussian 
    ## Link function: identity 
    ## 
    ## Formula:
    ## Flow ~ s(Year, bs = "cr", k = 30)
    ## 
    ## Parametric coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)    919.4       13.7   67.12   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Approximate significance of smooth terms:
    ##          edf Ref.df     F  p-value    
    ## s(Year) 3.44   3.44 14.67 1.19e-08 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## R-sq.(adj) =  0.338   
    ##   Scale est. = 18574     n = 100

``` r
gam.check(Gam$gam)
```

![](GAM_Coding_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

    ## 
    ## 'gamm' based fit - care required with interpretation.
    ## Checks based on working residuals may be misleading.
    ## Basis dimension (k) checking results. Low p-value (k-index<1) may
    ## indicate that k is too low, especially if edf is close to k'.
    ## 
    ##            k'   edf k-index p-value   
    ## s(Year) 29.00  3.44    0.77    0.01 **
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
plot(Gam$gam)
```

![](GAM_Coding_files/figure-gfm/unnamed-chunk-8-2.png)<!-- -->

Check the ACF and PACF.

``` r
layout(matrix(1:2, ncol = 2))
acf(resid(Gam$lme), lag.max = 36, main = "ACF")
pacf(resid(Gam$lme), lag.max = 36, main = "pACF")
```

![](GAM_Coding_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

``` r
layout(1)
```

We can also try ARMA error structure.

``` r
ctrl <- list(niterEM = 0, msVerbose = TRUE, optimMethod="L-BFGS-B")
Gam1<-gamm(Flow~s(Year,bs='tp'), data=Nile2, correlation = corARMA(p = 1))
Gam2<-gamm(Flow~s(Year,bs='tp'), data=Nile2, correlation = corARMA(p = 2))
Gam3<-gamm(Flow~s(Year,bs='tp'), data=Nile2, correlation = corARMA(p = 3))

anova(Gam$lme, Gam1$lme, Gam2$lme, Gam3$lme)
```

    ##          Model df      AIC      BIC    logLik   Test  L.Ratio p-value
    ## Gam$lme      1  4 1279.812 1290.233 -635.9060                        
    ## Gam1$lme     2  5 1277.609 1290.635 -633.8046 1 vs 2 4.202676  0.0404
    ## Gam2$lme     3  6 1279.120 1294.751 -633.5598 2 vs 3 0.489695  0.4841
    ## Gam3$lme     4  7 1281.103 1299.340 -633.5517 3 vs 4 0.016123  0.8990

``` r
plot(Gam1$gam)
```

![](GAM_Coding_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

For additional data exploration see below, check out cdcfluview for some
interesting data.
