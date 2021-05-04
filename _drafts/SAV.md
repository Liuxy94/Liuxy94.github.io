---
title: "Efficient SAV Approcach For Discrete Gradient Systems "
categories:
- Research
tags:
- SAV
- optimization
- deep learning
- neural network
- machine learning
toc: true
---
# Numerical Examples for SAV

## Quadratic function
Let

$$ f(x) = \sum\limits_{i} x_{i}^2 + \sum\limits_{j} \frac{x_{j}^2}{10^2}, $$

where $x_i$ is the odd element and $x_j$ is the even element. And

$$ \nabla_{\epsilon} f(x) := \nabla f(x) + \epsilon \mathcal{N}(\mathbf{0}, \mathbf{I}),$$

where $\epsilon$ denote the noise level, $\mathcal{N}(\mathbf{0}, \mathbf{I})$ is the Gaussian noise.

### M file list
*  `f(x)`,  `df(x)` <br>
return the value of $f(x)$ and $\nabla_{\epsilon} f(x)$
*  `L(x)`, `dL(x)`  <br>
return the value and gradient of linear part
*  `N(x)`, `dN(x)`  <br>
return the value and gradient of non-linear part
*  `inverseA(x)` <br>
compute the inverse of operator $A:= I + \sigma \Delta$
*  `optim(x0, lr, Parameter)` <br>
update $x$ based on GD, ADAM, LSGD and SAV
* `test_cases(test_name)` <br>
check the inverse operator and disspation law
*`pipeline(version)`<br>
framework to optimize the loss function
* `main`<br>
compare the operator

### Test in LSGD
$\sigma = 10.0$

## Phase retrieval
### M file list
*  `f(x_v, Problem)`,  `df(x_v, Problem)` <br>
return the value of $f(x)$ and $\nabla_{\epsilon} f(x)$ with size $2n \times 1$
*  `L(x_m, Parameter)`, `dL(x_v, Parameter)`  <br>
return the value and gradient of linear part with size $2n \times 1$
*  `N(x_v, Parameter, Problem)`,  `dN(x_v, Parameter, Problem)`  <br>
return the value and gradient of non-linear part with size $2n \times 1$
* `x_v = m2v(x_m)`, `x_m = v2m(x_v, Problem)` <br>
reshape the vector to matrix and matrix to vector
*  `inverseA(x)` <br>
compute the inverse of operator $A:= I + \sigma \Delta$
*  `[x1, Parameter] = optim(x0, dt, Parameter, Problem)` <br>
update $x$ based on GD, ADAM, LSGD and SAV
* `test_cases(test_name)` <br>
check the inverse operator and disspation law
* `pipeline(version)`<br>
framework to optimize the loss function
* `record_update(x0, Parameter, Record, Problem)`<br>
* `Parameter = paramter_initialization(x0, Parameter, Problem)`<br>
* `Record = record_initialization(x0, Parameter, Problem)` <br>
* `Record = record_update(x0, Parameter, Record, Problem)`<br>
* `main`<br>
compare the operator
