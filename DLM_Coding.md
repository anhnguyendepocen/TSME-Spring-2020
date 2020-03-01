DLM Coding Tutorial
================
Steve Midway
Spring 2020

*Note that this tutorial was developed from code previously written by
Kenneth Erickson.*

### Libraries

Dynamic Linear Models (DLMs) can be fit using a number of different R
packages, although `MARSS` and `DLM` are the two dominant packages.

``` r
library(MARSS) # Multivariate Autoregressive State-Space Modeling
library(dlm) # Dynamic Linear Models
```

### Simulate Some Data

Let’s create a data set that has some obvious trends of dynamics that we
want to see our models capture. Please work with this simulated data,
but also come back to this step and simulate data with different
characteristics to explore. To start, let’s simulate a data set of 20
observations where the first 15 values are stable and then decline near
the end of the series.

``` r
library(data.table)
set.seed(4)
obs <- c(rnorm(15, 0, 0.5), -4, -6, -11, -8, -9)
date <- 1:20
sim.decl <- data.table(date, obs)
```

And let’s make sure it looks like what we think we coded.

``` r
plot(obs ~ date, 
     data = sim.decl,
     ylim = c(-15,5),
     pch = 19,
     type = 'b',
     las = 1)
```

![](DLM_Coding_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->
