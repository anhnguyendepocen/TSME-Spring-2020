ARIMA Basics
================
Steve Midway
Spring 2020

## What is Time?

“You cannot think of eternity, Think of it like time, You try to think,
You try to count, You just mess up your
mind.”

<img src="dixon.png" title="Willie Dixon" alt="Willie Dixon" width="20%" style="display: block; margin: auto;" />

“I can tell your future, Look what’s in your
hand.”

<img src="hunter.png" title="Robert Hunter" alt="Robert Hunter" width="30%" style="display: block; margin: auto;" />

“It’s like linear regression using least squares, but it is much harder
and the results are not explicit”

<img src="tsaa.png" width="15%" style="display: block; margin: auto;" />

## Background to Time Series

Motivating questions:

  - What is time series data?
  - Why do we need time series
models?

<img src="djia.png" title="Time series of the DJIA" alt="Time series of the DJIA" width="75%" style="display: block; margin: auto;" />

## Time Series

  - What is time series data?
      - Sequences of measurements that follow a non-random order
      - Measurements are related (correlated) to each other and
        non-independent; e.g., a measurement at
        ![t\_2](https://latex.codecogs.com/png.latex?t_2 "t_2") is
        related to a measurment at
        ![t\_1](https://latex.codecogs.com/png.latex?t_1 "t_1"), or, the
        conditions of the measure at
        ![t\_1](https://latex.codecogs.com/png.latex?t_1 "t_1") will
        influence the measure at
        ![t\_2](https://latex.codecogs.com/png.latex?t_2 "t_2")
      
    ![y=x](https://latex.codecogs.com/png.latex?y%3Dx "y=x")  
  - Why do we need time series models?
      - We want to describe the time series phenomenon, forcast or
        predict the future, and need to account for error structure in
        the data
      - Many statistical tools, like the linear model, assume no
        auto-correlation or statistical independence of the errors,
        which is violated with time series data

*Time series: we regress today against yesterday*

## Some Time Series Terms

**trend** – a general, systematic, and non-repeating change

**seasonality** – repeated and systematic change over an interval

**smoothing** – local averaging of data to help with errors and improve
fitting

**autoregression** – the need to regress observations against previous
(lagged) observations

**autocorrelation** – the preserved signal that generates similarity
between observations and lagging
observations

<img src="keeling.png" width="50%" style="display: block; margin: auto;" />

## ARIMA(![p](https://latex.codecogs.com/png.latex?p "p"),![d](https://latex.codecogs.com/png.latex?d "d"),![q](https://latex.codecogs.com/png.latex?q "q")) models

ARIMA – **A**uto**R**egressive **I**ntegrated **M**oving **A**verage
models (also ARMA models)

Stationary assumption

1.  Mean of the ts should be constant (not a function of time)
2.  Variance of the ts should be constant (not a function of time)
3.  Covariance of the ts should be constnt (not a function of time)

There are tests for stationarity and *differencing*, *transformations*,
and other approaches to make non-stationary time series
stationary.

## (![p](https://latex.codecogs.com/png.latex?p "p"),![d](https://latex.codecogs.com/png.latex?d "d"),![q](https://latex.codecogs.com/png.latex?q "q")) ?

ARIMA models are typically accompanied by three numbers that are refered
to as (![p](https://latex.codecogs.com/png.latex?p
"p"),![d](https://latex.codecogs.com/png.latex?d
"d"),![q](https://latex.codecogs.com/png.latex?q "q"))

Each of these corresponds to part of the ARIMA model that will be
explored in more detail.

  - AR(![p](https://latex.codecogs.com/png.latex?p "p")) where
    ![p](https://latex.codecogs.com/png.latex?p "p") indicates the order
    of the autoregressive term

  - I(![d](https://latex.codecogs.com/png.latex?d "d")) where
    ![d](https://latex.codecogs.com/png.latex?d "d") indicates the
    amount of differencing (if differencing)

  - MA(![q](https://latex.codecogs.com/png.latex?q "q")) where
    ![q](https://latex.codecogs.com/png.latex?q "q") indicates the order
    of the moving average

*If all three terms = 0, then you don’t have a time series model. At
least one of the AR or MA terms needs to be 1 as a minimum for a time
series model.*

## AR(![p](https://latex.codecogs.com/png.latex?p "p"))

  - AutoRegressive refers to the serially-correlated measurements and
    the fact that the model estimates a coefficent(s) for this (lag)
    effect.
  - The notation ![p](https://latex.codecogs.com/png.latex?p "p")
    indicates the *order* or dimension of the autoregressive component
      - When ![p=0](https://latex.codecogs.com/png.latex?p%3D0 "p=0")
        there is no dependence and the process is often referred to as
        *white noise*
      - Typically ![p=1](https://latex.codecogs.com/png.latex?p%3D1
        "p=1") is the minimal
        ![p](https://latex.codecogs.com/png.latex?p "p") for an ARMA
        model

## AR(*p*) notation

Example AR(3) notation

  
![x\_t = \\beta\_0 + \\beta\_1 x\_{t-1} + \\beta\_2 x\_{t-2} + \\beta\_p
x\_{t-p} +
\\epsilon](https://latex.codecogs.com/png.latex?x_t%20%3D%20%5Cbeta_0%20%2B%20%5Cbeta_1%20x_%7Bt-1%7D%20%2B%20%5Cbeta_2%20x_%7Bt-2%7D%20%2B%20%5Cbeta_p%20x_%7Bt-p%7D%20%2B%20%5Cepsilon
"x_t = \\beta_0 + \\beta_1 x_{t-1} + \\beta_2 x_{t-2} + \\beta_p x_{t-p} + \\epsilon")  

  - ![x\_t](https://latex.codecogs.com/png.latex?x_t "x_t") is
    obervation at time ![t](https://latex.codecogs.com/png.latex?t "t")
  - ![\\beta\_0](https://latex.codecogs.com/png.latex?%5Cbeta_0
    "\\beta_0") is the intercept
  - ![x\_{t-1}](https://latex.codecogs.com/png.latex?x_%7Bt-1%7D
    "x_{t-1}"),
    ![x\_{t-2}](https://latex.codecogs.com/png.latex?x_%7Bt-2%7D
    "x_{t-2}"),
    ![x\_{t-p}](https://latex.codecogs.com/png.latex?x_%7Bt-p%7D
    "x_{t-p}") are the prior obervations
  - ![\\beta\_1](https://latex.codecogs.com/png.latex?%5Cbeta_1
    "\\beta_1"),
    ![\\beta\_2](https://latex.codecogs.com/png.latex?%5Cbeta_2
    "\\beta_2"),
    ![\\beta\_p](https://latex.codecogs.com/png.latex?%5Cbeta_p
    "\\beta_p") are the autoregressive coefficients (model parameters)
  - ![\\epsilon](https://latex.codecogs.com/png.latex?%5Cepsilon
    "\\epsilon") common error component

## AR(*p*) examples

``` r
x <- arima.sim(model = list(ar = -0.9), n = 100)
plot.ts(x)
```

![](ARIMA_Basics_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

## MA(![q](https://latex.codecogs.com/png.latex?q "q"))

  - Moving Average refers to the serially-correlated error and the fact
    that the model estimates a coefficent(s) for this (lag) effect.
  - The notation ![q](https://latex.codecogs.com/png.latex?q "q")
    indicates the order of the MA
    term
  - <http://people.cs.pitt.edu/~milos/courses/cs3750/lectures/class16.pdf>
