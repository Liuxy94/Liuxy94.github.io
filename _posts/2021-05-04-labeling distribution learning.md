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

## Label distribution learning (LDL)

 Label distribution learning is a learning framework to deal with problems of label ambiguity. Unlike single-label learning (SLL) and multi-label learning (MLL), which assume an instance is assigned to a single label or multiple labels, LDL aims at learning the relative importance of each label involved in the description of an instance, i.e., a distribution over the set of labels. Such a learning strategy is suitable for many real-world problems, which have label ambiguity. An example is facial age estimation. Even humans cannot predict the precise age from a single facial image. They may say that the person is probably in one age group and less likely to be in another. Hence it is more natural to assign a distribution of age labels to each facial image instead of using a single age label. Another example is movie rating prediction. Many famous movie review web sites, such as Netflix, IMDb and Douban, provide a crowd opinion for each movie specified by the distribution of ratings collected from their users. If a system could precisely predict such a rating distribution for every movie before it is released, movie producers can reduce their investment risk and the audience can better choose which movies to watch.
 
Many LDL methods assume the label distribution can be represented by a maximum entropy model and learn it by optimizing an energy function based on the model. But, the exponential part of this model restricts the generality of the distribution form, e.g., it has difficulty in representing mixture distributions. Some other LDL methods extend the existing learning algorithms, e.g, by boosting and support vector regression, to deal with label distributions, which avoid making this assumption, but have limitations in representation learning, e.g., they do not learn deep features in an end-to-end manner.


## Label Enhancement

Label distribution is more general than both single-label annotation and multi-label annotation. It covers a certain number of labels, representing the degree to which each label describes the instance. The learning process on the instances labeled by label distributions is called label distribution learning (LDL). Unfortunately, many training sets only contain simple logical labels rather than label distributions due to the difficulty of obtaining the label distributions directly. To solve the problem, one way is to recover the label distributions from the logical labels in the training set via leveraging the topological information of the feature space and the correlation among the labels. Such process of recovering label distributions from logical labels is defined as label enhancement (LE), which reinforces the supervision information in the training sets.

## Manifold Learning

In order to learn anything from data, we need to assume that the data has some inherent structure. In some machine learning methods, this assumption is implicit. By contrast, the field of manifold learning is defined by the fact that it makes this assumption explicit: it assumes that the observed data lie on a low-dimensional manifold embedded in a higher-dimensional space. Intuitively, this assumption, which is known as the manifold assumption or sometimes the manifold hypothesis, states that the shape of our data is relatively simple.

It should be emphasized that manifold learning is not a type of learning in the sense of supervised, semi-supervised, and unsupervised learning. Whereas these types of learning characterize the learning task (i.e. how much labeled data is available), manifold learning refers to a set of methods based on the manifold assumption. Manifold learning methods are used most often in the semi-supervised and unsupervised settings,5 but they may be used in the supervised setting as well.
