---
title: "Recommendation Systems"
excerpt: 'Federated Learning (FL) is an approach to apply machine learning to situations in which data cannot be centralized for a training process.'
toc: true
toc_sticky: true
categories: 
    - paper
    - data science
tags:
    - recommendation systems
    - machine learning
    - sparse matrix
    - Amazon product data
---
# Recommender Systems

## Collaborative Filtering

### Matrix Factorization

Matrix factorization is a simple embedding model. Given the feedback matrix $R\in R^{m\times n}$, where $m$ is the number of users (or queries) and $n$ is the number of items, the model learns:
* A user embedding matrix $U\in \mathcal{R}^{m\times d}$, where row i is the embedding for user i.
* An item embedding matrix $V\in \mathcal{R}^{n\times d}$, where row j is the embedding for item j.

The embeddings are learned such that the product $UV^T$ is a good approximation of the feedback matrix $R$. Observe that the $(i,j)$ entry of $UV^T$ is simply the dot product $<U_i, V_j>$ of the embeddings of user $i$ and item $j$, which you want to be close to $R_{i,j}$.

#### Objective Function

Intuitively, we may choose the squared distance as the objective function:

$$\arg\min\limits_{U,V}\sum\limits_{i,j} (R_{ij} - <U_i,V_j>)^2$$

It's common practice to add regularization terms to this loss function to help prevent overfitting. Adding L2 regularization terms for both row and column factors gives the following:

$$\arg\min\limits_{U,V}\sum\limits_{i,j} (R_{ij} - <U_i,V_j>)^2 + \lambda_U \sum\limits_{i}\|U_i\|^2 + \lambda_V \sum\limits_{j}\|V_j\|^2$$

Then, we have the gradient

$$\frac{\partial{L}}{\partial U_i} = -2\sum\limits_{j} (R_{ij} - <U_i,V_j>) V_j + 2\lambda_U U_i $$

$$\frac{\partial{L}}{\partial V_j} = -2\sum\limits_{i} U_i (R_{ij} - <U_i,V_j>) + 2\lambda_V V_j $$

###  Probabilistic Matrix Factorization

Inspired by Bayesian learning for parameter estimation, our aim is to find a posterior distribution of the model parameters by resorting to Bayes rule:

$$ P(\theta | X, \alpha) = \frac{P(X|\theta, \alpha) P(\theta | \alpha)}{P(X|\alpha)}$$

Here, $X$ is our dataset, $\theta$ is the parameter or parametr set of the distribution. $\alpha$ is the hyperparameter of the distribution.  

$$P(\theta |X, \alpha)$$ 
is the posterior distribution, also known as $\alpha$ - posteriori. 
$$P(X| \theta, \alpha)$$ 
is the likehood, and 
$$P(\theta| \alpha)$$ 
is the prior. 
The whole idea of the training process is that as we get more information about the data distribution, we will adjust the model parameter θ to fit the data. Technically speaking, the parameters of the posterior distribution will be plugged into the prior distribution for the next iteration of the training process. That is, the posterior distribution of a given training step will eventually become the prior of the next step. This procedure will be repeated until there is little variation in the posterior distribution 
$P(\theta | X, \alpha)$ 
between steps.

The model parameters will be $U$ and $V$, whereas $R$ will be our dataset. Once trained, we will end up with a revised $R^*$ matrix that will also contain ratings for user-item cells that were originally empty in $R$. We will use this revised ratings matrix to make predictions. Accordingly, $\theta$ = {$U, V$}, $X = R$, $\alpha = \sigma^2$, where $\sigma$ is the standard deviation of a zero-mean spherical Gaussian distribution. Then, by replacing these expressions in previous equation we will get:

$$ P(U,V | R, \sigma^2) = P(R|U,V, \sigma^2) P(U,V |\sigma^2_U, \sigma^2_V)$$

Since the $U$ and $V$ matrices are independent of each other (users and items occur independently), then this expression can also be written like this:

$$ P(U,V | R, \sigma^2) = P(R|U,V, \sigma^2) P(U |\sigma^2_U)P(V |\sigma^2_V)$$

First, the likelihood function is given by:

$$P(R|U,V, \sigma^2) = \prod\limits_{i=1}^{n}\prod\limits_{j=1}^{m} [\mathcal{N}(R_{ij}|U_{i}^TV_{j}, \sigma^2]^{\delta_{ij}}$$

Here, $\delta_{ij}$ is the indicator function. As we can see, this distribution is a spherical Gaussian with mean $U^T_iV_j$ and variance $\sigma^2$.

In turn, the prior distributions for U and V are given by:

$$P(U |\sigma^2_U) = \prod\limits_{i=1}^{n}\mathcal{N}(U_i|0, \sigma^2_U)$$

$$P(V |\sigma^2_V) = \prod\limits_{j=1}^{m}\mathcal{N}(V_j|0, \sigma^2_V)$$

which are two zero-mean spherical Gaussians. Then, we obtain

$$ P(U,V | R, \sigma^2) = \prod\limits_{i=1}^{n}\prod\limits_{j=1}^{m} [\mathcal{N}(R_{ij}|U_{i}^TV_{j}, \sigma^2]^{\delta_{ij}} \prod\limits_{i=1}^{n}\mathcal{N}(U_i|0, \sigma^2_U) \prod\limits_{j=1}^{m}\mathcal{N}(V_j|0, \sigma^2_V)$$

In order to train our model, we will seek to maximize this function with respect to the parameters $U$ and $V$ by equating their derivatives to zero. However, doing so will be very difficult because of the exp functions in the Gaussians. To overcome this issue, we should instead apply logarithms to both sides of the previous equations and then apply the required derivatives. Therefore, by applying logarithms to both sides of the previous equation, we will get

$$\ln P(U,V | R, \sigma^2) = \sum\limits_{i=1}^{n}\sum\limits_{j=1}^{m} \delta_{ij} \ln[\mathcal{N}(R_{ij}|U_{i}^TV_{j}, \sigma^2])] + \sum\limits_{i=1}^{n}\ln\mathcal{N}(U_i|0, \sigma^2_U) + \sum\limits_{j=1}^{m}\ln\mathcal{N}(V_j|0, \sigma^2_V)$$

By the definition of the Gaussian PDF that 
$\mathcal{N}(X|\mu, \sigma^2) = \frac{1}{\sigma\sqrt{2\pi}}\exp(-\frac{(X- \mu)^2}{2\sigma^2})$,
the log-posterior will look like this (For simplicity, we have gotten rid of the constants):

$$ \ln P(U,V | R, \sigma^2) = -\frac{1}{2\sigma^2}\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{m} \delta_{ij} (R_{ij} - U_i^TV_j)^2 - \frac{1}{2\sigma^2_U} \sum\limits_{i=1}^{n}\|U_i\|^2_F - \frac{1}{2\sigma^2_V} \sum\limits_{j=1}^{m}\|V_j\|^2_F $$

Finally, by introducing some additional notation to identify the hyperparameters of the model, we will get:

$$ L = -\frac{1}{2}(\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{m}) \delta_{ij} (R_{ij} - U_i^TV_j)^2 + \lambda_U \sum\limits_{i=1}^{n}\|U_i\|^2_F + \lambda_V \sum\limits_{j=1}^{m}\|V_j\|^2_F $$


[1]: https://developers.google.com/machine-learning/recommendation/collaborative/matrix
[2]: https://cloud.google.com/architecture/recommendation-system-tensorflow-overview
[3]: https://towardsdatascience.com/pmf-for-recommender-systems-cbaf20f102f0
