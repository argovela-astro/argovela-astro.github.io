---
title: 'Introduction to Clustering Algorithms: DBSCAN, OPTICS, and HDBSCAN'
date: 2023-05-08
permalink: /posts/2023-05-08-blog-dbscan
tags:
  - Unsupervised Machine Learning
  - Clustering Algorithms
  - Density-based Clustering
---

HDBSCAN - Hierarchical Density-Based Spatial Clustering of Applications with Noise.

Compared to the DBSCAN algorithm HDBSCAN has made several optimizations as follows:

1. Defines a way to measure the mutual reachability distance between two points from each other with the following formula:
$$
d_{\text {mreach }-k}(a, b)=\max \left\{\text { core }_k(a), \text { core }_k(b), \text { distance }(1, b)\right\}
$$
According to the [original article](https://ui.adsabs.harvard.edu/abs/2015arXiv150606422E/abstract), it has been shown that using this distance metric can effectively satisfy the Hartigan consistency condition

Use the minimum spanning tree to build a hierarchical number model between points, introducing the idea of hierarchical clustering, and at the same time the minimum spanning tree pruning of the smallest child tree to do restrictions, mainly to control the generation of class clusters not too small

3. defines a splitting measure called stability

Personally, I understand that the biggest advantage of HDBSCAN over DBSCAN is that there is no need to choose the domain radius R and MinPts manually, most of the time only the size of the minimum generating class cluster can be chosen, and the algorithm can automatically recommend the optimal cluster class result. It also defines a new distance measure that can better reflect the density of points.



Headings are cool
======

You can have many headings
======

Aren't headings cool?
------