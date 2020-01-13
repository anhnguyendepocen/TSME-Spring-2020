ARIMA Coding
================
Steve Midway
Spring 2020

As with most things in R, there is a rich environments of packages and
functions beyond the basics presented here. This coding tutorial is not
intended to be comprehensive, but to simply serve as a way to get you
coding ARIMA models and understanding their mechanics. This document is
intended for you to work through. Much of the code is there, but much of
it will need to be built on.

### Libraries

``` r
library(astsa) # Applied Statistical Time Series Analysis
```

### ’Time Series Objects

When working with time series data in R, we typically want to have the
observations in a time series object, or `ts()`.
