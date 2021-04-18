---
title: "Federated Learning"
excerpt: 'Federated Learning (FL) is an approach to apply machine learning to situations in which data cannot be centralized for a training process.'
toc: true
toc_sticky: true
categories: 
    - paper
    - data science
tags:
    - federated learning
    - machine learning
    - decision tree
    - neural networks
---
## Federated Learning
Federated learning (FL) is an approach to train machine learning models that do not require sharing datasets with a central entity. In federated learning, a model is trained collaboratively among multiple parties, which keep their training dataset to themselves out participate in a shared federated learning process. FL stands in contrast to established distributed training systems, where all data is transmitted to a central data center and is subsequently distributed among cluster nodes to train in parallel. This clustered, non-federated approach benifits from understanding the characteristics of the entire training data set and the computational capabilities of the nodes in the cluster, as well as its ability to partition the dataset into convenient chunks among nodes in a cluster. There assumptions do no hold in a federated setting.

## Challenges of FL
* data heterogeneity
* robustness of the federation process 
* selection of unbiased fusion operators
* securit and privacy inference prevention
* operational and effective deployment in enterprise and multi-cloud settings

## Concepts and Terminology

FL trains a model $\mathcal{M}$ over data $\mathcal{D}$. $\mathcal{M}$ can be a neural network or any non-neural model. In contrast to centrlized machine learning, $\mathcal{D}$ is split over $n$ parties, where each party $P_{i}$ has its own private training dataset $D_{i}$. An FL process involves an _aggregator_ $A$ and those $n$parties $P_1$, $P_2$, $\ldots$, $P_n$ in a way that no party has lnowleadge of any other dataset than its own. $A$ has no knowledge of any dataset.

The FL process is shown in Figure 1. To train a global machine learning model $\mathcal{M_G}$ the aggregator and the parties participate in a _federated learning algorithm_ that is excuted bt the aggregator and the partied by sending messages. The overall process runs as follows:
1. To train $\mathcal{M_G}$, the aggregator uses a function $\mathcal{Q}$ that takes as input the current model or state of the training $\mathcal{M_t}$ at round $t$, and generates a next query $q_{t+1}$.
2. One such query, $q_t$ requests information about a local model or aggregated information about each party's dataset. Example quries include requests for gradients or model weights of a neural network, or counts for decision trees.
3. The local training process applies a function $\mathcal{L}$ that takes query $q_t$ and the local dataset $D_i$ and outputs a _model update_ $r_{i,t}$. Usually the query, $q_t$, contains information that the party can use to initialize the local training process, for example, model weights to start with local training, or candidate feature values and/or class labels to compute counts for.
4. $r_{i,t}$ is sent back from party $P_i$ to the aggregator $A$, which collects all the $r_{i,t}$ from parties $P_i$.
5. When parties model updates $r_{1,t}$, $r_{2,t}$, $\ldots$, $r_{n,t}$, where $r_{i,t}$ refers to the model update of party $i$ at round $t$, are recieved by the aggregator forming set $R_t = \{r_{i,t}\}$, they are aggregated by applying fusion function $\mathcal{F}$ that takes as input $R_t$ and returns $\mathcal{M}_t$.

## [IBM Federated Learning][1]
Here, we take the IBM federated learning as the framework to run on MNIST dataset.

### Install Package

1. Setup an environment locally
```python
conda create -n IBM_FL python=3.6 tensorflow==1.15
```
```python
conda activate IBM_FL
```
```python
pip install <IBM_federated_learning_whl_file>
```
### Fusion Methods

| Method | Short description |
| ------------- |:-------------:| 
| Iterative Average | Simplest aggregation that is used as a baseline where all parties’ model updates are equally weighted. |
| Weighted average fusion |  Weights the average of updates based on the number of each party sample. Use with training data sets of widely differing sizes. |
| [Coordinate-Median Aggregation Fusion] [2] | Avoids the case that some computing units may behave abnormally, or even exhibit Byzantine failures—arbitrary and potentially adversarial behavior. | 
| [Federated Averaging Fusion] [3] |  Leaves the training data distributed on the mobile devices, and learns a shared model by aggregating locally-computed updates.  |
| [Krum Fusion][4] | An aggregation rule that satisfies a resilience property of the aggregation rule capturing the basic requirements to guarantee convergence despite f Byzantine workers.  | 
| [PFNM Aggregation Fusion] [5] |  |
| [SPAHM Aggregation Fusion][6] | |
| [Zeno Fusion][7] | tolerant to an arbitrary number of faulty workers.


[1]: https://github.com/IBM/federated-learning-lib
[2]: https://arxiv.org/pdf/1803.01498.pdf
[3]: https://arxiv.org/pdf/1602.05629.pdf
[4]: https://proceedings.neurips.cc/paper/2017/file/f4b9ec30ad9f68f89b29639786cb62ef-Paper.pdf
[5]: https://arxiv.org/pdf/1905.12022.pdf
[6]: https://arxiv.org/pdf/1911.00218.pdf
[7]: http://proceedings.mlr.press/v97/xie19b/xie19b.pdf

