Changeppint Coding Tutorial
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

    ##   0:     49.507015: -0.414254  4.32982  3.26855 -1.24133
    ##   1:     49.506451: -0.414139  4.34567  3.28647 -1.24135
    ##   2:     49.501846: -0.413177  4.49937  3.45978 -1.24147
    ##   3:     49.494754: -0.411401  4.91555  3.92776 -1.24144
    ##   4:     49.492367: -0.412546  5.21775  4.26678 -1.24207
    ##   5:     49.491206: -0.403774  5.52006  4.60559 -1.24092
    ##   6:     49.490630: -0.417820  5.82233  4.94424 -1.23668
    ##   7:     49.490547: -0.419559  6.12457  5.28273 -1.25504
    ##   8:     49.489968: -0.408064  6.31993  5.50146 -1.24257
    ##   9:     49.489874: -0.409701  6.48119  5.68202 -1.24109
    ##  10:     49.489776: -0.410995  6.82718  6.06940 -1.24096
    ##  11:     49.489731: -0.410921  7.14327  6.42329 -1.24143
    ##  12:     49.489708: -0.410732  7.45936  6.77718 -1.24166
    ##  13:     49.489697: -0.410695  7.77546  7.13106 -1.24170
    ##  14:     49.489691: -0.410715  8.09155  7.48494 -1.24167
    ##  15:     49.489688: -0.410731  8.40764  7.83882 -1.24165
    ##  16:     49.489686: -0.410734  8.72374  8.19270 -1.24164
    ##  17:     49.489685: -0.410732  9.03983  8.54658 -1.24164
    ##  18:     49.489685: -0.410731  9.35593  8.90046 -1.24165
    ##  19:     49.489684: -0.410730  9.67202  9.25434 -1.24165
    ##  20:     49.489684: -0.410730  9.98812  9.60823 -1.24165
    ##  21:     49.489684: -0.410730  10.3042  9.96211 -1.24165
    ##  22:     49.489684: -0.410731  10.6203  10.3160 -1.24165
    ##  23:     49.489684: -0.410731  10.9364  10.6699 -1.24165
    ##  24:     49.489684: -0.410731  11.2525  11.0237 -1.24165
    ##  25:     49.489684: -0.410730  11.5686  11.3776 -1.24165
    ##   0:     40.910591: -1.44721  11.5686  11.3776 -1.52337
    ##   1:     40.910591: -1.44721  11.5686  11.3776 -1.52337

``` r
summary(cp.nlme)
```

    ## Nonlinear mixed-effects model fit by maximum likelihood
    ##   Model: y ~ b0 + b1 * x + b2 * pmax(0, (x - cp)) 
    ##  Data: df3 
    ##        AIC      BIC    logLik
    ##   82.92157 95.53235 -32.46079
    ## 
    ## Random effects:
    ##  Formula: list(b0 ~ 1, b1 ~ 1, b2 ~ 1, cp ~ 1)
    ##  Level: g
    ##  Structure: Diagonal
    ##               b0           b1           b2       cp  Residual
    ## StdDev: 1.806038 4.018247e-06 4.863743e-06 1.948957 0.4248271
    ## 
    ## Fixed effects: b0 + b1 + b2 + cp ~ 1 
    ##        Value Std.Error DF    t-value p-value
    ## b0 -1.659563 1.2088818 24  -1.372809  0.1825
    ## b1  0.893315 0.0823688 24  10.845302  0.0000
    ## b2 -1.828088 0.1171570 24 -15.603743  0.0000
    ## cp  7.809097 1.2226704 24   6.386919  0.0000
    ##  Correlation: 
    ##    b0     b1     b2    
    ## b1 -0.363              
    ## b2  0.255 -0.703       
    ## cp  0.028 -0.091 -0.003
    ## 
    ## Standardized Within-Group Residuals:
    ##          Min           Q1          Med           Q3          Max 
    ## -1.904737336 -0.656367602  0.002111879  0.674024303  1.523407117 
    ## 
    ## Number of Observations: 30
    ## Number of Groups: 3

The fixed and random effects can be found in the model object.

``` r
cp.nlme$coefficients$random
```

    ## $g
    ##           b0            b1            b2         cp
    ## A  2.0649309 -3.884751e-11 -1.346506e-10 -2.2051566
    ## B  0.2486689 -2.525717e-10 -1.862525e-10 -0.3181826
    ## C -2.3135998  2.914192e-10  3.209031e-10  2.5233392

There are many, many more libraries, functions, and model for working
with changepoint data. Please do not limit yourself to the above
suggestions, but use them to help gain an understanding of what datasets
they may be useful and applicable for.
