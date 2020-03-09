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
    ## psi1.x       5 5.000046 0.8048849
    ## psi2.x      10 9.502767 0.7128859

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

    ##   0:     58.999244: -0.0384370  2.85867  3.76117 -1.03062
    ##   1:     58.999004: -0.0384847  2.86002  3.77672 -1.03054
    ##   2:     58.997026: -0.0389751  2.87243  3.92758 -1.02969
    ##   3:     58.993825: -0.0393495  2.90557  4.35692 -1.03009
    ##   4:     58.993256: -0.0466292  2.92608  4.68466 -1.01365
    ##   5:     58.992212: -0.0424361  2.94669  5.01270 -1.02416
    ##   6:     58.992009: -0.0418946  2.95541  5.23669 -1.02671
    ##   7:     58.991883: -0.0413606  2.95950  5.46084 -1.02676
    ##   8:     58.991749: -0.0431268  2.95846  5.78593 -1.02694
    ##   9:     58.991648: -0.0398936  2.95121  6.11092 -1.02531
    ##  10:     58.991557: -0.0412041  2.93965  6.43581 -1.02612
    ##  11:     58.991427: -0.0403605  2.90735  7.15202 -1.02756
    ##  12:     58.991396: -0.0393319  2.88435  7.62464 -1.02882
    ##  13:     58.991395: -0.0390786  2.88038  7.71507 -1.02915
    ##  14:     58.991395: -0.0390506  2.88030  7.72749 -1.02919
    ##  15:     58.991394: -0.0389929  2.88031  7.77717 -1.02929
    ##  16:     58.991394: -0.0389066  2.88055  7.93150 -1.02945
    ##  17:     58.991393: -0.0388943  2.88084  8.08582 -1.02948
    ##  18:     58.991393: -0.0389605  2.88148  8.45535 -1.02938
    ##  19:     58.991392: -0.0390648  2.88179  8.81999 -1.02921
    ##  20:     58.991392: -0.0391306  2.88189  9.12484 -1.02910
    ##  21:     58.991392: -0.0391503  2.88191  9.35576 -1.02907
    ##  22:     58.991392: -0.0391500  2.88190  9.58668 -1.02907
    ##  23:     58.991392: -0.0391324  2.88187  9.98176 -1.02910
    ##  24:     58.991392: -0.0391164  2.88184  10.3156 -1.02912
    ##  25:     58.991392: -0.0391089  2.88183  10.6189 -1.02914
    ##   0:     47.845486: -1.49538  1.12031  10.6189 -1.36579
    ##   1:     47.845486: -1.49538  1.12031  10.6189 -1.36579

``` r
summary(cp.nlme)
```

    ## Nonlinear mixed-effects model fit by maximum likelihood
    ##   Model: y ~ b0 + b1 * x + b2 * pmax(0, (x - cp)) 
    ##  Data: df3 
    ##        AIC      BIC    logLik
    ##   96.79136 109.4021 -39.39568
    ## 
    ## Random effects:
    ##  Formula: list(b0 ~ 1, b1 ~ 1, b2 ~ 1, cp ~ 1)
    ##  Level: g
    ##  Structure: Diagonal
    ##              b0       b1          b2       cp  Residual
    ## StdDev: 2.22717 0.162845 1.22062e-05 1.956463 0.4992487
    ## 
    ## Fixed effects: b0 + b1 + b2 + cp ~ 1 
    ##        Value Std.Error DF    t-value p-value
    ## b0 -3.007210 1.4843761 24  -2.025909   0.054
    ## b1  1.126759 0.1399953 24   8.048554   0.000
    ## b2 -2.245302 0.1377796 24 -16.296333   0.000
    ## cp  7.775722 1.2261231 24   6.341714   0.000
    ##  Correlation: 
    ##    b0     b1     b2    
    ## b1 -0.240              
    ## b2  0.243 -0.487       
    ## cp  0.025 -0.060 -0.004
    ## 
    ## Standardized Within-Group Residuals:
    ##         Min          Q1         Med          Q3         Max 
    ## -1.86003104 -0.63225801  0.07861182  0.60818791  1.80369515 
    ## 
    ## Number of Observations: 30
    ## Number of Groups: 3

The fixed and random effects can be found in the model object.

``` r
cp.nlme$coefficients$random
```

    ## $g
    ##           b0          b1            b2         cp
    ## A  2.8468818 -0.12372324 -1.429991e-09 -1.9979789
    ## B -0.4674356  0.18975351  1.720856e-09 -0.6314792
    ## C -2.3794462 -0.06603027 -2.908649e-10  2.6294581

There are many, many more libraries, functions, and model for working
with changepoint data. Please do not limit yourself to the above
suggestions, but use them to help gain an understanding of what datasets
they may be useful and applicable for.
