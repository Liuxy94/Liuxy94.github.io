---
title: "Labeling Distribution Learning"
excerpt: 'Label distribution learning (LDL) is a general learning framework, which assigns to an instance a distribution over a set of labels rather than a single label or multiple labels.'
toc: true
toc_sticky: true
categories: 
    - paper
    - data science
tags:
    - label
    - distribution learning
    - machine learning
    - multi-label Learning 
    - classification
---

## Label distribution learning

 Label distribution learning is a learning framework to deal with problems of label ambiguity. Unlike single-label learning (SLL) and multi-label learning (MLL), which assume an instance is assigned to a single label or multiple labels, LDL aims at learning the relative importance of each label involved in the description of an instance, i.e., a distribution over the set of labels. Such a learning strategy is suitable for many real-world problems, which have label ambiguity. An example is facial age estimation. Even humans cannot predict the precise age from a single facial image. They may say that the person is probably in one age group and less likely to be in another. Hence it is more natural to assign a distribution of age labels to each facial image instead of using a single age label. Another example is movie rating prediction. Many famous movie review web sites, such as Netflix, IMDb and Douban, provide a crowd opinion for each movie specified by the distribution of ratings collected from their users. If a system could precisely predict such a rating distribution for every movie before it is released, movie producers can reduce their investment risk and the audience can better choose which movies to watch.
 
Many LDL methods assume the label distribution can be represented by a maximum entropy model and learn it by optimizing an energy function based on the model. But, the exponential part of this model restricts the generality of the distribution form, e.g., it has difficulty in representing mixture distributions. Some other LDL methods extend the existing learning algorithms, e.g, by boosting and support vector regression, to deal with label distributions, which avoid making this assumption, but have limitations in representation learning, e.g., they do not learn deep features in an end-to-end manner.


## Label Enhancement

Label distribution is more general than both single-label annotation and multi-label annotation. It covers a certain number of labels, representing the degree to which each label describes the instance. The learning process on the instances labeled by label distributions is called label distribution learning (LDL). Unfortunately, many training sets only contain simple logical labels rather than label distributions due to the difficulty of obtaining the label distributions directly. To solve the problem, one way is to recover the label distributions from the logical labels in the training set via leveraging the topological information of the feature space and the correlation among the labels. Such process of recovering label distributions from logical labels is defined as label enhancement (LE), which reinforces the supervision information in the training sets.

### Notation and Concept

Let the dataset be ${x_i, y_i}$ where $x_{i}$ is the $i$-th instance and $y_i$ is the according label value. The logical label vector of $x_i$ is denoted by $l_i = (l_{x_i}^{y_1}, \ldots, l_{x_i}^{y_c})^{T}$, where $c$ is the number of possible labels. The description degree of $y$ to $x$ is denoted by $ d_{x}^{y} $ , and the label distribution of $x_i$ is denoted by $d_i = (d_{x_i}^{y_1}, \ldots, d_{x_i}^{y_c})^{T}$. Let $\chi = R^{q}$ denote the $q$-dimensional feature space. Then, the process of label enhancement can be defined as follows:

Given a training set $S = (x_i, l_i)$, where $x_i\in \chi$ and $l_i \in (0,1)^{c}$, label enhancement recovers the label distribution $d_i$ of $x_i$ from the logical label vector $l_i$, and thus transforms $S$ into a LDL training set $\varepsilon = (x_i, d_i)$.

### Multi-Label Manifold Learning
We assume the topological structure of the feature space can be represented by a graph $G = <V,\varepsilon, W>$, where $V$ is the vertex set, $\varepsilon$ is the edge set in which each edge $e_i^j$ represents the relationship between the data $x_i$ and $x_j$, and $W$ is the weight matrix whose element $w_{ij}$ represents the weight of relationship between $x_i$ and $x_j$ (the weight of the edge $e_i^j$).


With the **smoothness assumption** that the points close to each other are more likely to share a label, the local topological structure can be shared between the feature manifold and the label manifold. According to this assumption, we need to use the local neighborhood information of each point to construct $G$ to keep the locality. For computational convenience, we use the **linearity assumption** that each data point can be optimally reconstructed using a linear combination of its neighbors. Then, we can introduce following minimization to approximate of the feature manifold

$$L_{\text{feature}}(W) = \sum\limits_{i=1}^{n} \|x_i - \sum\limits_{j\neq i}W_{i}^{j}x_j\|^2,$$

where $W_i^j = 0$ unless $x_j$ is one of $x_i$'s $K$-nearest neighbors. Without loss of generality, we assume that $\|W_i\|_{1} = \sum\limits_{k=1}^{n} W_i^k = 1$. Then, the approximation can be solved as a quadrtic function with constraint that

$$ \arg\min\limits_{W_i} W_i^T G_iW_i \quad \text{s.t. } \|W_i\|_1 = 1,$$

where $G_i$ is the local Gram matrix at point $x_i$ with $G_i^{jk} = (x_i - x_j)^T(x_i - x_k)$.

With the transferred topological structure, the reconstruction of the label manifold can infer to the minimization of 

$$L_{\text{label}}(d) = \sum\limits_{i=1}^{n}\|d_i - \sum\limits_{j\neq i} w_{ij} d_j\|^2 \quad \text{s.t. } d_{x_i}^{y_l} l_{x_i}^{y_l} > \lambda, \forall 1\leq i \leq n, 1 \leq l \leq c,$$

where $\lambda >0$. 

**Remark**: There are three advantages for the above constraint: 
1. It is convenient to judge whether a label is relevant or irrelevant to the example by the sign of it; 
2. It guarantees that the relevant numerical labels are larger than the irrelevant ones; 
3. The minimum of the relevant numerical labels will be equal to $\lambda$ or the maximum of the irrelevant numerical labels will be equal to $-\lambda$. This makes the scale of the reconstructed numerical labels on the control.
#### Implementation
The reconstructed numerical labels are real and the prob- lem can not be treated as a classification but rather a regres- sion problem. In the multi-label case, it is actually a multi- output regression problem. There have been some efficient algorithms proposed such as multi-output support vector regression (MSVR) and we propose the optimizor based on the MSVR.
Similar to the MSVR, we generalize the 1-D SVR to solve the multi-dimensional case. In addition, our regressor not only concerns the distance between the predicted and the real values, but also the sign consistency of them. It leads to the minimization of

$$L(\Theta, b) = \frac{1}{2}\sum\limits_{j=1}^{c}\|\theta^j\|^2 + C_1\sum\limits_{i=1}^{n}L_1(r_i) + C_2 \sum\limits_{i=1}^{n}\sum\limits_{j=1}^{c}L_2(t_i^j),$$

## Manifold Learning

In order to learn anything from data, we need to assume that the data has some inherent structure. In some machine learning methods, this assumption is implicit. By contrast, the field of manifold learning is defined by the fact that it makes this assumption explicit: it assumes that the observed data lie on a low-dimensional manifold embedded in a higher-dimensional space. Intuitively, this assumption, which is known as the manifold assumption or sometimes the manifold hypothesis, states that the shape of our data is relatively simple.

It should be emphasized that manifold learning is not a type of learning in the sense of supervised, semi-supervised, and unsupervised learning. Whereas these types of learning characterize the learning task (i.e. how much labeled data is available), manifold learning refers to a set of methods based on the manifold assumption. Manifold learning methods are used most often in the semi-supervised and unsupervised settings,5 but they may be used in the supervised setting as well.

### Isomap

One of the earliest approaches to manifold learning is the Isomap algorithm, short for Isometric Mapping. Isomap can be viewed as an extension of Multi-dimensional Scaling (MDS) or Kernel PCA. Isomap seeks a lower-dimensional embedding which maintains geodesic distances between all points.

### Locally Linear Embedding

Locally linear embedding (LLE) seeks a lower-dimensional projection of the data which preserves distances within local neighborhoods. It can be thought of as a series of local Principal Component Analyses which are globally compared to find the best non-linear embedding.

### Modified Locally Linear Embedding

One well-known issue with LLE is the regularization problem. When the number of neighbors is greater than the number of input dimensions, the matrix defining each local neighborhood is rank-deficient. To address this, standard LLE applies an arbitrary regularization parameter $r$, which is chosen relative to the trace of the local weight matrix. Though it can be shown formally that as $r\rightarrow 0$, the solution converges to the desired embedding, there is no guarantee that the optimal solution will be found for $r>0$. This problem manifests itself in embeddings which distort the underlying geometry of the manifold.

One method to address the regularization problem is to use multiple weight vectors in each neighborhood. This is the essence of modified locally linear embedding (MLLE).

### Spectral Embedding

Spectral Embedding is an approach to calculating a non-linear embedding. One example is Laplacian Eigenmaps, which finds a low dimensional representation of the data using a spectral decomposition of the graph Laplacian. The graph generated can be considered as a discrete approximation of the low dimensional manifold in the high dimensional space. Minimization of a cost function based on the graph ensures that points close to each other on the manifold are mapped close to each other in the low dimensional space, preserving local distances

### Local Tangent Space Alignment

Though not technically a variant of LLE, Local tangent space alignment (LTSA) is algorithmically similar enough to LLE that it can be put in this category. Rather than focusing on preserving neighborhood distances as in LLE, LTSA seeks to characterize the local geometry at each neighborhood via its tangent space, and performs a global optimization to align these local tangent spaces to learn the embedding. 







[1]: http://palm.seu.edu.cn/xgeng/LDL/index.htm


