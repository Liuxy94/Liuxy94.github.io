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
update $x$ based on GD, ADAM, LSGD
* `SAV(x0, lr, Parameter)` <br>
update $x$ based on SAV approach
* `test_cases(test_name)` <br>
check the inverse operator and disspation law
* `compare


