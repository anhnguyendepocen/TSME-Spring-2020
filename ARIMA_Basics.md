ARIMA Basics
================
Steve Midway
Spring 2020

## What is Time?

### The Philosophical Interpretation

> “You cannot think of eternity, Think of it like time, You try to
> think, You try to count, You just mess up your mind.” –Willie
Dixon

<img src="dixon.png" width="20%" style="display: block; margin: auto 0 auto auto;" />

<br> <br> <br>

### The Practical Interpretation

> “Time is what keeps everything from happening at once.” – Ray
Cummings

<img src="cummings.jpg" width="25%" style="display: block; margin: auto 0 auto auto;" />

<br> <br> <br>

### The Non-Believer Interpretation

> “’Cause tomorrow’s just another day, And I don’t believe in time”
> –Hootie and the
Blowfish

<img src="hootie.jpg" width="30%" style="display: block; margin: auto 0 auto auto;" />

<br> <br> <br>

### The Auto-Regressive Poetic Interpretation

> “I can tell your future, Look what’s in your hand.” –Robert
Hunter

<img src="hunter.png" width="30%" style="display: block; margin: auto 0 auto auto;" />

<br> <br> <br>

### The Statistician’s Interpretation

> “It’s like linear regression using least squares, but it is much
> harder and the results are not explicit.” –David
Stoffer

<img src="stoffer.jpg" width="25%" style="display: block; margin: auto 0 auto auto;" />

<br> <br> <br>

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
  - Why do we need time series models?
      - We want to describe the time series phenomenon, forcast or
        predict the future, and need to account for error structure in
        the data
      - Many statistical tools, like the linear model, assume no
        auto-correlation or statistical independence of the errors,
        which is violated with time series data

**Time series: we regress today against yesterday**

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

## ARIMA(*p*,*d*,*q*) models

ARIMA – **A**uto**R**egressive **I**ntegrated **M**oving **A**verage
models (also ARMA models)

Stationary assumption

1.  Mean of the *ts* should be constant (not a function of time)
2.  Variance of the *ts* should be constant (not a function of time)
3.  Covariance of the *ts* should be constnt (not a function of time)

There are tests for stationarity and **differencing**,
**transformations**, and other approaches to make non-stationary time
series stationary.

## (*p*,*d*,*q*) ?

ARIMA models are typically accompanied by three numbers that are refered
to as (*p*,*d*,*q*)

Each of these corresponds to part of the ARIMA model that will be
explored in more detail.

  - AR(*p*) where *p* indicates the order of the autoregressive term

  - I(*d*) where *d* indicates the amount of differencing (if
    differencing)

  - MA(*q*) where *q* indicates the order of the moving average

*If all three terms = 0, then you don’t have a time series model. At
least one of the AR or MA terms needs to be 1 as a minimum for a time
series model.*

## AR(*p*)

  - AutoRegressive refers to the serially-correlated measurements and
    the fact that the model estimates a coefficent(s) for this (lag)
    effect.
  - The notation *p* indicates the *order* or dimension of the
    autoregressive component
      - When *p*=0 there is no dependence and the process is often
        referred to as *white noise*
      - Typically *p*=1 is the minimal *p* for an ARMA model

Example AR(1)

  
![X\_t = \\phi X\_{t-1} +
\\epsilon\_t](https://latex.codecogs.com/png.latex?X_t%20%3D%20%5Cphi%20X_%7Bt-1%7D%20%2B%20%5Cepsilon_t
"X_t = \\phi X_{t-1} + \\epsilon_t")  

  - ![X\_t](https://latex.codecogs.com/png.latex?X_t "X_t") is
    obervation at time ![t](https://latex.codecogs.com/png.latex?t "t")
  - ![\\phi](https://latex.codecogs.com/png.latex?%5Cphi "\\phi") is the
    coefficient
  - ![X\_{t-1}](https://latex.codecogs.com/png.latex?X_%7Bt-1%7D
    "X_{t-1}") are the prior obervations
  - ![\\epsilon](https://latex.codecogs.com/png.latex?%5Cepsilon
    "\\epsilon") common error component, in this case refered to as
    *white noise*

**Note that time series notation and equations is not straightforward to
the novice, combined with the fact that it appears not to be very
standardized. Although I always caution students to not code a model
they could not also express in statistical notation, this seminar will
not force the notation issue.**

## MA(*q*)

  - Moving Average refers to the serially-correlated error and the fact
    that the model estimates a coefficent(s) for this (lag) effect.
  - The notation *q* indicates the order of the MA term

Example MA(1)   
![\\epsilon\_t = W\_t + \\theta
W\_{t-1}](https://latex.codecogs.com/png.latex?%5Cepsilon_t%20%3D%20W_t%20%2B%20%5Ctheta%20W_%7Bt-1%7D
"\\epsilon_t = W_t + \\theta W_{t-1}")  

## ARMA

When we combine the AR and MA models, we get an ARMA(1,1) model
expressed as:

  
![X\_t = \\phi X\_{t-1} + W\_t + \\theta
W\_{t-1}](https://latex.codecogs.com/png.latex?X_t%20%3D%20%5Cphi%20X_%7Bt-1%7D%20%2B%20W_t%20%2B%20%5Ctheta%20W_%7Bt-1%7D
"X_t = \\phi X_{t-1} + W_t + \\theta W_{t-1}")  

## Stationarity

**stationarity** – mean is stable, correlation structure is contestant

  - Stationarity is a very important aspect of the time series data you
    will model
  - You need to know if your data are stationary or non-stationary
    before starting
  - If your data are stationary, you can easily estimate mean and use
    that for understanding correlation; in other words, we can lag
    observations because we assume the underlying mean is not changing,
    and we can use different lags
  - If your data are not stationary, you can *difference* (![x\_t -
    x\_{t1}](https://latex.codecogs.com/png.latex?x_t%20-%20x_%7Bt1%7D
    "x_t - x_{t1}")) and then model the differenced data (e.g., random
    walk)
  - If your data are non-stationary and heteroscedastic suggest log
    transform to stabilize the variance and then difference to stabilize
    the trend (see Random Walk example below)
  - Differencing adds the **I** to ARMA and makes it an ARIMA model; in
    other words, ARMA models are for data that are already stationary
  - If the differenced data has ARMA properties, then continue with
    ARIMA

**ARMA for stationary data
only**

![](ARIMA_Basics_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->![](ARIMA_Basics_files/figure-gfm/unnamed-chunk-8-2.png)<!-- -->![](ARIMA_Basics_files/figure-gfm/unnamed-chunk-8-3.png)<!-- -->

## Understanding the Model Orders

  - You cannot typically determine ARMA(*p*,*q*) structure visually, so
    you will need to diagnose, examine, and figure it out
  - Auto-Correlation (ACF) and Partial Auto-Correlation are the
    diagostic tools used to identify model orders
  - ACF displays the correlation between today and previous time points;
    ACF interprets MA order
  - PACF displays the correlation between today and previous time
    points, but accounts for (and removes the effect of) past
    correlations
  - PACF is the “real” correlation because it controls for outside
    influences; PACF interprets AR order
  - Although ACF and PACF display the correlations, there is still a bit
    of estimation and evaluation that you will need to consider.

This table will help interprect ACF anf PACF output. This table is taken
from Schumway and Stoffer (20XX).

|      | AR(p)             | MA(q)             | ARMA(p,q) |
| ---- | ----------------- | ----------------- | --------- |
| ACF  | Tails off         | Cuts off at lag q | Tails off |
| PACF | Cuts off at lag p | Tails off         | Tails off |

**I think with time you will understand how to interpret tailing off and
cutting off, but be aware that these are not technical terms and there
is some subjectivity to their interpretation\!**

Using simulated data, let’s see some examples of ACF and PACF plots with
known ARMA data.

![](ARIMA_Basics_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

The above plots were generated from simulated data for an AR(1) model.
We can see that the ACF tails off and the PACF cuts off abruptly after 1
lag. Based on the table above, our diagnostics describe an AR(*q*),
where *q* = 1 as suggested by the cut off.

![](ARIMA_Basics_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

These ACF and PACF plots were generated based on an AR(2) model You
should be able ot tell that they fit the description in the table above
to be an AR(*p*) model, but with 2 significant lag effects in the PACF,
we can correctly assume that the AR order is 2.

![](ARIMA_Basics_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

Here we see plots that show a cut off in the ACF after 1 and a PACF that
appears to tail off. These characteristics suggest an MA(*q*) model, and
the cutoff at 1in the ACF further points to an MA(1) model.

![](ARIMA_Basics_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

Finally, these are diagnostic plots from an ARMA model. You might be
able to tell that both appear to tail off. (If that is not clear, there
are additional diagnostics that can be explored. Also, note that in this
case and all the above examples, the data is simulated and different
simulation runs may result in different diagnostic plots.)

## Model Selection and Diagnostics

  - Always good to go with the simpler model and add from there if
    needed
  - Use AIC and BIC for model selection
  - Look at fitted models and ‘significant’ parameters
  - Check model residuals

<!-- end list -->

    ## initial  value 0.674653 
    ## iter   2 value -0.055096
    ## iter   3 value -0.064343
    ## iter   4 value -0.066561
    ## iter   5 value -0.070968
    ## iter   6 value -0.070970
    ## iter   6 value -0.070970
    ## final  value -0.070970 
    ## converged
    ## initial  value -0.013426 
    ## iter   2 value -0.014557
    ## iter   3 value -0.020773
    ## iter   4 value -0.022728
    ## iter   5 value -0.022834
    ## iter   6 value -0.022856
    ## iter   7 value -0.022856
    ## iter   8 value -0.022856
    ## iter   9 value -0.022856
    ## iter  10 value -0.022856
    ## iter  10 value -0.022856
    ## iter  10 value -0.022856
    ## final  value -0.022856 
    ## converged

![](ARIMA_Basics_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

    ## $fit
    ## 
    ## Call:
    ## stats::arima(x = xdata, order = c(p, d, q), seasonal = list(order = c(P, D, 
    ##     Q), period = S), xreg = xmean, include.mean = FALSE, transform.pars = trans, 
    ##     fixed = fixed, optim.control = list(trace = trc, REPORT = 1, reltol = tol))
    ## 
    ## Coefficients:
    ##           ar1    xmean
    ##       -0.9078  -0.0181
    ## s.e.   0.0465   0.0510
    ## 
    ## sigma^2 estimated as 0.9389:  log likelihood = -139.61,  aic = 285.22
    ## 
    ## $degrees_of_freedom
    ## [1] 98
    ## 
    ## $ttable
    ##       Estimate     SE  t.value p.value
    ## ar1    -0.9078 0.0465 -19.5378  0.0000
    ## xmean  -0.0181 0.0510  -0.3547  0.7236
    ## 
    ## $AIC
    ## [1] 2.852165
    ## 
    ## $AICc
    ## [1] 2.853402
    ## 
    ## $BIC
    ## [1] 2.93032

The above is a typical set of plots that can be used to check model fit.
In the top panel, the standardized residuals appear to center on 0 with
asymptotic variance, which is what we want. The ACF of the residuals
checks out of we see few to no residuals beyond the dashed blue lines.
The Q-Q plot is interpreted as a typical Q-Q plot in linear diagnostics.
Finally, the bottom panel is quickly interpreted as good when all (or
nearly all) of the values are above the dashed blue line.
