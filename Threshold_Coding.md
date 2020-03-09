Changepoint Coding Tutorial
================
Steve Midway
Spring 2020

### Introduction

Although not an explicit time series model, a type of changepoint model
may be useful if you are trying to estimate one or a few points in your
data when a response changes. Note that what I am calling changepoint
models are also called *threshold models*, *breakpoint models*,
*segmented regression*, *piecewise regression*, and the *hockey stick
model*, among other names. There may be differences among these terms;
however, for our purposes these are all the same model. Also note that
what is presented here are not time series models in the sense that they
account for autocorrelated observations. Despite this fact, they may
still be appropriate and useful for analyzing data that takes place over
time.

### Estimating a Single Changepoint

Let’s create a data set that has some obvious trends of dynamics that we
want to see our models capture. Please work with this simulated data,
but feel free to come back to this step and simulate data with different
characteristics to explore. To start, let’s simulate a data set of 20
observations where the first 10 values increase with time and the final
10 values decrease.

``` r
library(data.table)
obs1 <- c(2,2,5,4,6,7,6,8,9,9)
obs2 <- c(10,9,7,7,8,6,4,3,3,1)
y <- c(obs1, obs2)
x <- 1:20
df <- data.table(x,y)
```

And let’s plot to make sure it looks like a series of observations with
a changepoint.

``` r
plot(y ~ x,
     data = df,
     pch = 19, 
     las = 1)
```

![](Threshold_Coding_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

One simple package for estimaing a changepoints is `segmented`. Using
this package, you will save your linear model and then include that
model object into the `segmented()` function, along with the covariate
for which you want to estimate the changepoint and a starting value for
where the function will look for the changepoint.

``` r
library(segmented)
lin.mod <- lm(y~x, data = df)
segmented.mod <- segmented(lin.mod, seg.Z = ~x, psi=10)
```

If everything is running correctly, you should have fit your segmented
regression. You can extract an AIC value (for comparisons, if needed),
and index the segmented object for `psi` to see the changepoint
estimate.

``` r
#AIC(segmented.mod)
segmented.mod$psi
```

    ##        Initial     Est.    St.Err
    ## psi1.x      10 10.84722 0.4257324

Finally, the function `slope()` will print out coefficient information
for the estimated slopes. How are the slopes interpreted?

``` r
slope(segmented.mod)
```

    ## $x
    ##            Est. St.Err.  t value CI(95%).l CI(95%).u
    ## slope1  0.81212 0.09096   8.9284    0.6193   1.00490
    ## slope2 -0.93333 0.09096 -10.2610   -1.1262  -0.74051

There is also a function to plot the segmented model object; however,
you might have more command by simply plotting the data and adding the
regression lines (as segments, unrelated to the model fitting function).

``` r
plot(segmented.mod)
```

![](Threshold_Coding_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

### Estimating Multiple Changepoints

Let’s say we have a data set with three trends and two changepoints. It
might look like this data:

``` r
library(data.table)
y <- c(2,2,5,4,7,6,5,4,5,4,6,8,7,9,10)
x <- 1:15
df2 <- data.table(x,y)
```

And let’s take a look.

``` r
plot(y ~ x,
     data = df2,
     pch = 19, 
     las = 1)
```

![](Threshold_Coding_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

We can use the same machinery in as above, but add a changepoint
estimate to the `psi` argument.

``` r
lin.mod2 <- lm(y~x, data = df2)
segmented.mod2 <- segmented(lin.mod2, seg.Z = ~x, psi=c(5,10))
```

We see that `psi` produces two changepoint estimates.

``` r
segmented.mod2$psi
```

    ##        Initial     Est.    St.Err
    ## psi1.x       5 5.272727 0.6596229
    ## psi2.x      10 8.370370 0.6481275

And `slope()` now gives us all three estimated slope coefficients.

``` r
slope(segmented.mod)
```

    ## $x
    ##            Est. St.Err.  t value CI(95%).l CI(95%).u
    ## slope1  0.81212 0.09096   8.9284    0.6193   1.00490
    ## slope2 -0.93333 0.09096 -10.2610   -1.1262  -0.74051

You are encouraged to check out the package `mcp`, which stands for
multiple changepoints, and is a package that also provides functions for
estimating models with multiple changepoints. Note that `mcp()` can
support generalized linear models.

### Changepoint Parameter as a Random Effect

Perhaps you have a situation where you have a single changepoint, but it
should be treated as a random effect. For example, maybe you have a
phenological dataset where some response increases and then decreases
based on an environmental variable, although each year that
environmental variable (i.e., changepoint) might not occur at the same
exact time. In this example, the general function is conserved, but the
changepoint may shift in its estimate of *x*. You might think of similar
examples were the random effects are related to species, spatial units,
or other classic random effects units.

Let’s simulate data for four groups that each have a shared functional
response, but different chngepoints.

``` r
raw <- c(1,2,3,4,5,5,4,3,2,1)
obs1 <- raw + rnorm(n = 10,mean = 0,sd = 0.5)
obs2 <- raw + rnorm(n = 10,mean = 0,sd = 0.5)
obs3 <- raw + rnorm(n = 10,mean = 0,sd = 0.5)
y <- c(obs1, obs2, obs3)
g <- c(rep("A",10),rep("B",10),rep("C",10))
x <- c(1:10, 3:12, 6:15)
df3 <- data.table(x,y,g)
```

And, of course, a quick visual.

``` r
library(lattice)
xyplot(y ~ x | g, 
       data = df3, 
       type = c("p", "smooth"), 
       lwd = 4,
       pch = 16)
```

![](Threshold_Coding_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

Using the `nlme` library and function, we can write a formula for our
changepoint model with random effects. We can specify what is fixed and
random, but the important part here is to make sure the changepoint can
be random.

``` r
library(nlme)
cp.nlme <- nlme(y ~ b0 + b1 * x + b2 * pmax(0, (x - cp)),
                        data = df3,
                        fixed = b0 + b1 + b2 + cp ~ 1,
                        random = pdDiag(b0 + b1 + b2 + cp ~ 1),
                        groups = ~ g,
                        control = list(msVerbose = TRUE, niterEM = 200,
                                      opt = "nlminb", tol = 1e-5),
                        start = c(b0 = 1, b1 = 1, b2 = -1, cp = 8))
```

    ##   0:     60.440637: -0.199409  4.39063  3.58862 -0.732998
    ##   1:     60.440126: -0.199379  4.40621  3.60526 -0.732971
    ##   2:     60.435920: -0.199132  4.55730  3.76642 -0.732795
    ##   3:     60.429176: -0.198811  4.98055  4.21716 -0.731702
    ##   4:     60.426928: -0.196460  5.29041  4.54674 -0.736081
    ##   5:     60.426550: -0.209802  5.59996  4.87588 -0.717883
    ##   6:     60.425036: -0.196430  5.90973  5.20512 -0.729399
    ##   7:     60.424776: -0.201371  6.15655  5.46742 -0.736892
    ##   8:     60.424576: -0.197361  6.40343  5.72978 -0.734252
    ##   9:     60.424460: -0.197636  6.65033  5.99215 -0.732773
    ##  10:     60.424374: -0.198455  7.01678  6.38154 -0.731866
    ##  11:     60.424339: -0.198590  7.33308  6.71764 -0.731868
    ##  12:     60.424320: -0.198518  7.64937  7.05373 -0.732023
    ##  13:     60.424311: -0.198469  7.96567  7.38982 -0.732104
    ##  14:     60.424306: -0.198462  8.28197  7.72591 -0.732113
    ##  15:     60.424303: -0.198469  8.59827  8.06200 -0.732100
    ##  16:     60.424302: -0.198473  8.91457  8.39809 -0.732091
    ##  17:     60.424301: -0.198473  9.23087  8.73418 -0.732090
    ##  18:     60.424300: -0.198473  9.54716  9.07027 -0.732091
    ##  19:     60.424300: -0.198472  9.86346  9.40636 -0.732092
    ##  20:     60.424300: -0.198472  10.1798  9.74245 -0.732092
    ##  21:     60.424300: -0.198472  10.4961  10.0785 -0.732092
    ##  22:     60.424300: -0.198472  10.8124  10.4146 -0.732092
    ##  23:     60.424300: -0.198473  11.1287  10.7507 -0.732092
    ##  24:     60.424300: -0.198473  11.4450  11.0868 -0.732092
    ##   0:     39.068338: -1.90713  11.4450  11.0868 -1.72000
    ##   1:     39.068338: -1.90713  11.4450  11.0868 -1.72000

``` r
summary(cp.nlme)
```

    ## Nonlinear mixed-effects model fit by maximum likelihood
    ##   Model: y ~ b0 + b1 * x + b2 * pmax(0, (x - cp)) 
    ##  Data: df3 
    ##        AIC      BIC    logLik
    ##   79.23707 91.84784 -30.61853
    ## 
    ## Random effects:
    ##  Formula: list(b0 ~ 1, b1 ~ 1, b2 ~ 1, cp ~ 1)
    ##  Level: g
    ##  Structure: Diagonal
    ##               b0           b1           b2       cp  Residual
    ## StdDev: 2.476652 3.936674e-06 5.632131e-06 2.053992 0.3677993
    ## 
    ## Fixed effects: b0 + b1 + b2 + cp ~ 1 
    ##        Value Std.Error DF    t-value p-value
    ## b0 -3.014838 1.5862520 24  -1.900605  0.0694
    ## b1  1.102028 0.0717991 24  15.348773  0.0000
    ## b2 -2.178985 0.1017739 24 -21.410052  0.0000
    ## cp  8.006599 1.2809066 24   6.250728  0.0000
    ##  Correlation: 
    ##    b0     b1     b2    
    ## b1 -0.241              
    ## b2  0.170 -0.705       
    ## cp  0.014 -0.069  0.006
    ## 
    ## Standardized Within-Group Residuals:
    ##        Min         Q1        Med         Q3        Max 
    ## -1.8376553 -0.6127800  0.0330443  0.4766855  2.0413453 
    ## 
    ## Number of Observations: 30
    ## Number of Groups: 3

The fixed and random effects can be found in the model object.

``` r
cp.nlme$coefficients$random
```

    ## $g
    ##           b0            b1            b2         cp
    ## A  2.4785100  2.892945e-10  2.148040e-10 -2.2401969
    ## B  0.8945889  1.162725e-10  1.542495e-10 -0.4759691
    ## C -3.3730989 -4.055669e-10 -3.690535e-10  2.7161660

There are many, many more libraries, functions, and model for working
with changepoint data. Please do not limit yourself to the above
suggestions, but use them to help gain an understanding of what datasets
they may be useful and applicable for.

### Some Readings

[T. Wagner, S.R. Midway (2014). Modeling spatially varying landscape
change points in species occurrence thresholds.
Ecosphere 5(11).](https://www.stevemidway.com/publication/wagner2014ecosphere/wagner2014Ecosphere.pdf)

[Qian, S. S. (2014). Ecological threshold and environmental management:
a note on statistical methods for detecting thresholds. Ecological
indicators, 38, 192-197.](https://www.sciencedirect.com/science/article/pii/S1470160X13004263)

[Qian, S. S., & Cuffney, T. F. (2012). To threshold or not to threshold?
That’s the question. Ecological
Indicators, 15(1), 1-9.](https://www.sciencedirect.com/science/article/pii/S1470160X11002640)

[Thomson, J. R., Kimmerer, W. J., Brown, L. R., Newman, K. B., Nally, R.
M., Bennett, W. A., … & Fleishman, E. (2010). Bayesian change point
analysis of abundance trends for pelagic fishes in the upper San
Francisco Estuary. Ecological
Applications, 20(5), 1431-1448.](https://esajournals.onlinelibrary.wiley.com/doi/pdf/10.1890/09-0998.1)
