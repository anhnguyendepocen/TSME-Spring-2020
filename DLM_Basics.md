DLM Basics
================
Steve Midway
Spring 2020

## State-Space Models

State-space models (SSMs) are a class of hierarchical models that
involve two, coupled models.

## Dynamic Linear Models

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

  
![y\_i = \\begin{bmatrix} 1 x\_i\\\\ \\end{bmatrix} \\begin{bmatrix}
\\alpha\\\\ \\beta \\end{bmatrix} +
e\_i](https://latex.codecogs.com/png.latex?y_i%20%3D%20%5Cbegin%7Bbmatrix%7D%201%20x_i%5C%5C%20%5Cend%7Bbmatrix%7D%20%5Cbegin%7Bbmatrix%7D%20%5Calpha%5C%5C%20%5Cbeta%20%5Cend%7Bbmatrix%7D%20%2B%20e_i
"y_i = \\begin{bmatrix} 1 x_i\\\\ \\end{bmatrix} \\begin{bmatrix} \\alpha\\\\ \\beta \\end{bmatrix} + e_i")  

We can then think about the ![\\begin{bmatrix} 1 x\_i\\\\
\\end{bmatrix}](https://latex.codecogs.com/png.latex?%5Cbegin%7Bbmatrix%7D%201%20x_i%5C%5C%20%5Cend%7Bbmatrix%7D
"\\begin{bmatrix} 1 x_i\\\\ \\end{bmatrix}") matrix as
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

*Aside: If you took my Data Analysis in R class, I know you are familiar
with model matrices. Recall `model.matrix()`?*

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

But a problem exists with the model form above. That is,

## Resources and References
