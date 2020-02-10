DLM Basics
================
Steve Midway
Spring 2020

## State-Space Models

State-space models (SSMs) are a class of hierarchical models that
involve two, coupled models. The two models allow for separate
estimation of **process** and **observation**. In other words, the
process model estimates the condition of the state while the observation
model is the observation of that state used to estimate the process
model. Another critically important aspect of these models is that the
error associated with understanding the state/process and the error
associated with observations are kept separate and therefore
independently estimated.

A classic SSM application is population modeling.

SSM properties:

  - Autoregressive process (i.e., Markovian)
  - Observations are imperfect
  - Observation and state are correlated (a large true population
    results in a large observed population)

State Equation:

  
![N\_{t+1} = N\_t
\\lambda\_t](https://latex.codecogs.com/png.latex?N_%7Bt%2B1%7D%20%3D%20N_t%20%5Clambda_t
"N_{t+1} = N_t \\lambda_t")  
(![\\lambda](https://latex.codecogs.com/png.latex?%5Clambda "\\lambda")
is population growth rate)

Observation Equation:

  
![y\_t = N\_t +
e\_i](https://latex.codecogs.com/png.latex?y_t%20%3D%20N_t%20%2B%20e_i
"y_t = N_t + e_i")  

<div class="figure">

<img src="schaub.png" alt="From Kery and Schaub (2012)" width="80%" />

<p class="caption">

From Kery and Schaub (2012)

</p>

</div>

<div class="figure">

<img src="auger.png" alt="From Auger-Methe (2020)" width="90%" />

<p class="caption">

From Auger-Methe (2020)

</p>

</div>

## Dynamic Linear Models

Dynamic linear models are a subset of state-space models, but for our
purposes we won’t get into the details of any differences. Think of them
as a very popular flavor of SSMs.

DLMs can be conceptually simple, but technically complex.

In order to start thinking about dynamic linear models (DLMs), let’s
recall a linear model.

  
![y\_i = \\alpha + \\beta x\_i +
e\_i](https://latex.codecogs.com/png.latex?y_i%20%3D%20%5Calpha%20%2B%20%5Cbeta%20x_i%20%2B%20e_i
"y_i = \\alpha + \\beta x_i + e_i")  

But also recall that ![i](https://latex.codecogs.com/png.latex?i "i")
has no order. ![y](https://latex.codecogs.com/png.latex?y "y") and
![x](https://latex.codecogs.com/png.latex?x "x") are paired and indexed
by ![i](https://latex.codecogs.com/png.latex?i "i"), but there is no
sequence to the observations of
![y](https://latex.codecogs.com/png.latex?y "y") or
![x](https://latex.codecogs.com/png.latex?x "x").

If we think about this linear regression model in matrix notation, it
might look like:

  
![y\_i = \\begin{bmatrix} 1\\; x\_i\\\\ \\end{bmatrix} \\begin{bmatrix}
\\alpha\\\\ \\beta \\end{bmatrix} +
e\_i](https://latex.codecogs.com/png.latex?y_i%20%3D%20%5Cbegin%7Bbmatrix%7D%201%5C%3B%20x_i%5C%5C%20%5Cend%7Bbmatrix%7D%20%5Cbegin%7Bbmatrix%7D%20%5Calpha%5C%5C%20%5Cbeta%20%5Cend%7Bbmatrix%7D%20%2B%20e_i
"y_i = \\begin{bmatrix} 1\\; x_i\\\\ \\end{bmatrix} \\begin{bmatrix} \\alpha\\\\ \\beta \\end{bmatrix} + e_i")  

*Aside: If you took my Data Analysis in R class, I know you are familiar
with model matrices. Recall `model.matrix()`? What we are discussing
here is the same thing\! I told you that stuff would be useful.*

We can then think about the ![\\begin{bmatrix} 1\\; x\_i\\\\
\\end{bmatrix}](https://latex.codecogs.com/png.latex?%5Cbegin%7Bbmatrix%7D%201%5C%3B%20x_i%5C%5C%20%5Cend%7Bbmatrix%7D
"\\begin{bmatrix} 1\\; x_i\\\\ \\end{bmatrix}") matrix as
![\\mathbf{X}^T\_i](https://latex.codecogs.com/png.latex?%5Cmathbf%7BX%7D%5ET_i
"\\mathbf{X}^T_i") and the ![\\begin{bmatrix} \\alpha\\\\ \\beta
\\end{bmatrix}](https://latex.codecogs.com/png.latex?%5Cbegin%7Bbmatrix%7D%20%5Calpha%5C%5C%20%5Cbeta%20%5Cend%7Bbmatrix%7D
"\\begin{bmatrix} \\alpha\\\\ \\beta \\end{bmatrix}") matrix as
![\\mathbf{\\theta}](https://latex.codecogs.com/png.latex?%5Cmathbf%7B%5Ctheta%7D
"\\mathbf{\\theta}").

This produces a re-written equation

  
![y\_i = \\mathbf{X}^T\_i \\mathbf{\\theta} +
e\_i](https://latex.codecogs.com/png.latex?y_i%20%3D%20%5Cmathbf%7BX%7D%5ET_i%20%5Cmathbf%7B%5Ctheta%7D%20%2B%20e_i
"y_i = \\mathbf{X}^T_i \\mathbf{\\theta} + e_i")  

DLM is based on a linear model, but adds structures to allow the
parameters to change. So while a linear model estimates some number of
fixes parameters (e.g., an intercept and a slope), a DLM estimates
several parameters that are a function of (local) time.

Sticking with the matrix notation (which is common and necessary in
DLMs), we can modify the linear model to turn it into a DLM.

  
![y\_t = \\mathbf{X}^T\_t \\mathbf{\\theta}\_t +
e\_t](https://latex.codecogs.com/png.latex?y_t%20%3D%20%5Cmathbf%7BX%7D%5ET_t%20%5Cmathbf%7B%5Ctheta%7D_t%20%2B%20e_t
"y_t = \\mathbf{X}^T_t \\mathbf{\\theta}_t + e_t")  

Basically, we change the ![i](https://latex.codecogs.com/png.latex?i
"i") notation to ![t](https://latex.codecogs.com/png.latex?t "t"), which
indicates that the observations (data) have an inherent order that
creates non-independence and needs to be accounted for.

But a problem exists with the model form above. That is, we don’t want
to estimate parameters based only on the interval
![t](https://latex.codecogs.com/png.latex?t "t"). That is very little
data going into the esimtation of up to several parameters. Imagine
estimating a linear regression and each observation
(![x](https://latex.codecogs.com/png.latex?x "x") and
![y](https://latex.codecogs.com/png.latex?y "y")) got its own
parameters? For example, how would you estimate a slope with only one
point?

The solution is that we add observations (information) from previous
time steps. This lagging effect can be expressed as

  
![\\mathbf{\\theta}\_t = \\mathbf{G} \\mathbf{\\theta}\_{t-1} +
\\mathbf{w}\_t](https://latex.codecogs.com/png.latex?%5Cmathbf%7B%5Ctheta%7D_t%20%3D%20%5Cmathbf%7BG%7D%20%5Cmathbf%7B%5Ctheta%7D_%7Bt-1%7D%20%2B%20%5Cmathbf%7Bw%7D_t
"\\mathbf{\\theta}_t = \\mathbf{G} \\mathbf{\\theta}_{t-1} + \\mathbf{w}_t")  
where

  
![\\mathbf{w}\_t \\sim
MVN(0,Q)](https://latex.codecogs.com/png.latex?%5Cmathbf%7Bw%7D_t%20%5Csim%20MVN%280%2CQ%29
"\\mathbf{w}_t \\sim MVN(0,Q)")  

![\\mathbf{G}](https://latex.codecogs.com/png.latex?%5Cmathbf%7BG%7D
"\\mathbf{G}") is an ![m \\times
m](https://latex.codecogs.com/png.latex?m%20%5Ctimes%20m "m \\times m")
identity matrix.

The state-space form of the equation can be written as

  
![y\_t = \\mathbf{X}^T\_t \\mathbf{\\theta}\_t +
e\_t](https://latex.codecogs.com/png.latex?y_t%20%3D%20%5Cmathbf%7BX%7D%5ET_t%20%5Cmathbf%7B%5Ctheta%7D_t%20%2B%20e_t
"y_t = \\mathbf{X}^T_t \\mathbf{\\theta}_t + e_t")  
  
![\\mathbf{\\theta}\_t = \\mathbf{G} \\mathbf{\\theta}\_{t-1} +
\\mathbf{w}\_t](https://latex.codecogs.com/png.latex?%5Cmathbf%7B%5Ctheta%7D_t%20%3D%20%5Cmathbf%7BG%7D%20%5Cmathbf%7B%5Ctheta%7D_%7Bt-1%7D%20%2B%20%5Cmathbf%7Bw%7D_t
"\\mathbf{\\theta}_t = \\mathbf{G} \\mathbf{\\theta}_{t-1} + \\mathbf{w}_t")  

### DLM Example Equations

Let’s recall two ideas that were mentioned in ARIMA but are built
more-explicitly on here.

#### White Noise

White noise is essentially a random sample from a normal distribution
that is serially uncorrelated but considered sequential (i.e., a time
series). (For example, if you did a random draw of 100 samples you might
plot it with a histogram because you are trained to do that and it is
not a time series; however, that same sample is white noise when it is a
time series.) White noise is also important in time series because we
often want our errors to be thought of as white noise.

``` r
set.seed(20)
# Generate white noise
wn <- rnorm(100,10,3)
plot.ts(wn, las=1)
```

![](DLM_Basics_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

#### Random Walk

A random walk is one of the simplest time series model and another
useful modeling construct as we develop more advanced models. Random
walks are simply non-stationary time series, or white noise plus the
correlation to the previous observation.

``` r
set.seed(20)
## length of time series
t <- 100
## initialize {x_t} and {w_t}
x <- w <- rnorm(n = t, mean = 0, sd = 1)
## compute values 2 thru t
for (i in 2:t) {
    x[i] <- x[i - 1] + w[i]
}
# plot
plot.ts(x, ylab = expression(italic(x[t])))
```

![](DLM_Basics_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

Now let’s look at some simple DLMs not to focus on the equations, but to
see how the equations build in more features.

#### Expanding the Stochastic Intercept Model

Think of this as an intercept-only model, or a model in which we only
want to model a mean level at each time interval. This is conceptually a
random walk.

  
![y\_t = \\alpha\_t +
e\_t](https://latex.codecogs.com/png.latex?y_t%20%3D%20%5Calpha_t%20%2B%20e_t
"y_t = \\alpha_t + e_t")  
  
![\\alpha\_t = \\alpha\_{t-1} +
w\_t](https://latex.codecogs.com/png.latex?%5Calpha_t%20%3D%20%5Calpha_%7Bt-1%7D%20%2B%20w_t
"\\alpha_t = \\alpha_{t-1} + w_t")  

We can also express this model as an observation model

  
![y\_t = x\_t +
\\nu\_t](https://latex.codecogs.com/png.latex?y_t%20%3D%20x_t%20%2B%20%5Cnu_t
"y_t = x_t + \\nu_t")  
  
![x\_t = x\_{t-1} +
w\_t](https://latex.codecogs.com/png.latex?x_t%20%3D%20x_%7Bt-1%7D%20%2B%20w_t
"x_t = x_{t-1} + w_t")  

To this stochastic intercept model we can add *deterministic growth*, or
a *bias*. The additional term is called such because it is a fixed
factor and not something that is estimated or assumed to be stochastic.

We start with the same model as above   
![y\_t = \\alpha\_t +
e\_t](https://latex.codecogs.com/png.latex?y_t%20%3D%20%5Calpha_t%20%2B%20e_t
"y_t = \\alpha_t + e_t")  

And then simply add the parameter
![\\eta](https://latex.codecogs.com/png.latex?%5Ceta "\\eta") for the
*bias*. Note that ![\\eta](https://latex.codecogs.com/png.latex?%5Ceta
"\\eta") is not indexed, which is further evidence that is it
deterministic (or as I would call it, *global*).

  
![\\alpha\_t = \\alpha\_{t-1} + \\eta +
w\_t](https://latex.codecogs.com/png.latex?%5Calpha_t%20%3D%20%5Calpha_%7Bt-1%7D%20%2B%20%5Ceta%20%2B%20w_t
"\\alpha_t = \\alpha_{t-1} + \\eta + w_t")  

If we want that growth parameter to be stochastic because, perhaps, we
think it changes throughout the time series, we simply let
![\\eta](https://latex.codecogs.com/png.latex?%5Ceta "\\eta") be indexed
and then add an equation for estimating
![\\ets](https://latex.codecogs.com/png.latex?%5Cets "\\ets").

We start with the same model\!

  
![y\_t = \\alpha\_t +
e\_t](https://latex.codecogs.com/png.latex?y_t%20%3D%20%5Calpha_t%20%2B%20e_t
"y_t = \\alpha_t + e_t")  

And add time-varying
![\\eta](https://latex.codecogs.com/png.latex?%5Ceta "\\eta") to our
intercept estimate

  
![\\alpha\_t = \\alpha\_{t-1} + \\eta\_{t-1} +
w\_{\\alpha,t}](https://latex.codecogs.com/png.latex?%5Calpha_t%20%3D%20%5Calpha_%7Bt-1%7D%20%2B%20%5Ceta_%7Bt-1%7D%20%2B%20w_%7B%5Calpha%2Ct%7D
"\\alpha_t = \\alpha_{t-1} + \\eta_{t-1} + w_{\\alpha,t}")  

And then give ![\\eta](https://latex.codecogs.com/png.latex?%5Ceta
"\\eta") its own model   
![\\eta\_t = \\eta\_{t-1} +
w\_{\\eta,t}](https://latex.codecogs.com/png.latex?%5Ceta_t%20%3D%20%5Ceta_%7Bt-1%7D%20%2B%20w_%7B%5Ceta%2Ct%7D
"\\eta_t = \\eta_{t-1} + w_{\\eta,t}")  

The above equations are not meant to be memorized and furthermore, they
can be expressed in many different ways. Rather, they are shown to
demystify some of the complexity around the equations and notation for
DLMs.

## Other DLM Considerations

### Advantages

  - Non-stationary data(\!)
  - Good for modeling trends, seasonality, covariates, thresholds,
    breaks, and other dynamical aspects of the system
  - Useful for time series with missing data (not true of other time
    series models)
  - Can be applied to non-normal data

### Disadvantages

  - Can be very data hungry (may contain a lot of parameters)
  - Can have problems if data are zero-inflated
  - Terminology and machinery can get complicated and very matrix-heavy
  - Benefit from longer time series (though true for nearly all time
    series models)

## Resources and References

  - Holmes, E.E., Ward, E.J. and Wills, K., 2012. MARSS: Multivariate
    Autoregressive State-space Models for Analyzing Time-series Data. R
    Journal, 4(1).
    [Link](https://s3.amazonaws.com/academia.edu.documents/30588864/rjournal_2012-1_holmes_et_al.pdf?response-content-disposition=inline%3B%20filename%3DMARSS_Multivariate_Autoregressive_State-.pdf&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWOWYYGZ2Y53UL3A%2F20200210%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20200210T005027Z&X-Amz-Expires=3600&X-Amz-SignedHeaders=host&X-Amz-Signature=e1a935e13e4aab3623ad0bbcae2b3e9d21e7307961512680e10e0ac9188604f5)

  - Auger-Methe, M. et al. 2020. an Introduction to State-Space
    modeoling of Ecological Time Series. ArXiv.
    [Link](https://arxiv.org/pdf/2002.02001.pdf)

  - Kéry, M. and Schaub, M., 2011. Bayesian population analysis using
    WinBUGS: a hierarchical perspective. Academic Press.

  - Holmes, E.E., M. D. Scheuerell, and E. J. Ward. Applied Time Series
    Analysis for Fisheries and Environmental Sciences
    [Link](https://nwfsc-timeseries.github.io/atsa-labs/)
