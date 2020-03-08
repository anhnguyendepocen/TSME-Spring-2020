Threshold Coding Tutorial
================
Steve Midway
Spring 2020

### Introduction

Although not an explicit time series model, a type of threshold model
may be useful if you are trying to estimate a single point in your data
when a response changed. Note that what I am calling threshold models
are also called changepoint models, segmented regression, piecewise
regression, and the hockey stick model. There may be differences among
these terms; however, for our purposes these are all the same model.

### Libraries

Dynamic Linear Models (DLMs) can be fit using a number of different R
packages, although `MARSS` and `DLM` are the two dominant packages.

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
sim.df <- data.table(date, obs)
```
