---
title: 'Introduction to Clustering Algorithms: DBSCAN, OPTICS, and HDBSCAN'
date: 2023-05-08
permalink: /posts/2023-05-08-blog-dbscan
tags:
  - Unsupervised Machine Learning
  - Clustering Algorithms
  - Density-based Clustering
---

## DBSCAN Pros and Cons Summary
Advantages:

Compared to K-Means, DBSCAN does not require a pre-declared number of clusters.
It can cluster dense datasets of arbitrary shape, as opposed to clustering algorithms like K-Means which are generally only applicable to convex datasets.
It is possible to find outliers while clustering and is insensitive to outliers in the dataset.
The clustering results are not biased, in contrast, the initial values of clustering algorithms such as K-Means have a great influence on the clustering results.
Disadvantages:

The quality of clustering is poor when the density of spatial clustering is not uniform and the clustering spacing difference is very different, because the parameters MinPts and Eps are difficult to select in this case.
The clustering convergence time is long if the sample set is large, and then it can be improved by imposing a size limit on the KD tree or sphere tree built when searching the nearest neighbor.
The point at the junction edge of two clusters will decide which cluster to join depending on its order in the database; fortunately, this is not common and has little impact on the overall clustering result (DBSCAN* variant algorithm, which treats the junction point as noise, achieves a fully deterministic result.)
Tuning is slightly more complicated than traditional clustering algorithms like K-Means, mainly requiring joint tuning of distance threshold eps, neighborhood sample number threshold MinPts, and different combinations of parameters have a greater impact on the final clustering result.

## OPTICS:
OPTICS is an extended algorithm for DBSCAN. This algorithm allows the algorithm to be less sensitive to radius Eps. A slight change in radius Eps does not affect the clustering results as long as the value of minPts is determined.

OPTICS does not generate the resulting class clusters as displayed, but rather generates an extended cluster ordering for the clustering analysis (e.g., a coordinate plot with reachable distance as the vertical axis and sample point output order as the horizontal axis), and this ordering represents the density-based clustering structure of each sample point. It contains information equivalent to the density-based clustering obtained from a wide range of parameter settings, in other words, the clustering results of the DBSCAN algorithm based on any parameter Eps and minPts can be obtained from this ranking.

OPTICS (Ordering Points To Identify the Clustering Structure) is another unsupervised machine learning method that is used for clustering. It's similar to DBSCAN in the way it works, but it addresses one of DBSCAN's major weaknesses: the need to choose an appropriate value for the radius of the neighborhood around each point. Instead of a global parameter, OPTICS generates a reachability plot which gives a visualization of the data's structure and allows cluster extraction at different levels of granularity.

Application of OPTICS to find galaxy clusters:

Similar to HDBSCAN, before starting with OPTICS, you need to have a structured dataset which typically includes parameters like right ascension (RA), declination (Dec), redshift (z), etc., of the galaxies. The process of finding galaxy clusters can be described as follows:

Data Preparation: Clean your data and ensure it's in the right format. Remove any null or irrelevant values. The dimensions used for clustering could be the coordinates of the galaxies (e.g., right ascension and declination) or some transformed coordinates (e.g., redshift space).

Feature Scaling: This step might be necessary if the features have different scales. The common methods of feature scaling include normalization and standardization.

Apply OPTICS: Apply the OPTICS algorithm to the dataset. OPTICS has two main parameters, max_eps and min_samples. The max_eps is the maximum radius of the neighborhood around a data point, and min_samples is the number of points to be included in the cluster's core. The choice of these parameters may affect the number and size of clusters that you find, and they may need to be tuned depending on the specifics of your dataset.

Interpret the results: The output of OPTICS is an ordering of the data points as a reachability plot, rather than explicit clusters. However, clusters can be extracted from this plot at different scales, which can be useful if you're not sure ahead of time what the appropriate scale is.

Limitations of OPTICS:

Complexity: OPTICS is more complex and computationally expensive than some other clustering algorithms. Its time complexity is O(n^2) in the worst case, although in practice it's usually closer to O(n log n).

Noisy Data: OPTICS can be sensitive to noise in the data, which can distort the reachability plot and make it harder to identify clusters.

Parameter Selection: Although OPTICS does not require choosing a global radius parameter like DBSCAN, it still requires choosing values for max_eps and min_samples, which can influence the results and may need to be tuned based on the dataset.

Interpretability: Interpreting the reachability plot and extracting clusters from it can be less straightforward than with algorithms that directly output clusters.

Remember, the quality of clustering always depends on the nature of the data, the feature representation, and the specific task at hand. Also, always consider the computational resources you have at your disposal when choosing a clustering algorithm.


## what's the difference between them?

Both HDBSCAN and OPTICS are density-based clustering algorithms, and they're both extensions of the original DBSCAN algorithm. Here are some key differences between them:

1. Handling of Variable Density:

HDBSCAN, compared to OPTICS, is better at handling clusters of varying density. HDBSCAN operates by transforming the space according to the density/sparsity of the data points, which effectively makes it able to find clusters of different densities.

OPTICS, on the other hand, provides a reachability plot which presents the data's structure at different density levels, but doesn't inherently handle varying densities as well as HDBSCAN.

2. Hierarchical Clustering:

HDBSCAN, as the "H" in its name suggests, offers hierarchical clustering. It creates a hierarchy of clusters at different scales and uses this to determine the "best" clustering at a single scale. This can be particularly useful for data where clusters are nested within other clusters.

OPTICS doesn't provide an inherent hierarchy of clusters. However, the reachability plot it generates can be analyzed at different scales to extract clusters at those scales.

3. Interpretability of Results:

HDBSCAN provides explicit cluster assignments, which can be easier to interpret and use for some applications. Each point is assigned to a cluster, or designated as noise.

OPTICS, in contrast, provides an ordered list of data points and their reachability distances, which can be less intuitive to interpret. You have to examine the reachability plot and decide where to cut it off to define clusters.

4. Parameter Selection:

Both algorithms require parameter tuning, but the parameters have different interpretations. In HDBSCAN, the key parameters are min_cluster_size and min_samples, which control the minimum size of clusters and the density considered for clustering.

In OPTICS, the parameters are min_samples (similar to HDBSCAN's parameter) and max_eps, which is the maximum reachability distance to consider. If max_eps is set too low, denser clusters may be missed; if it's set too high, clusters may be merged together.

In summary, the best algorithm to use depends on the specifics of your dataset and the problem you're trying to solve. Both HDBSCAN and OPTICS have their strengths and weaknesses, and it might be beneficial to try both and see which one gives better results for your specific use case.










## HDBSCAN - Hierarchical Density-Based Spatial Clustering of Applications with Noise. 

Performs DBSCAN over varying epsilon values and integrates the result to find a clustering that gives the best stability over epsilon. This allows HDBSCAN to find clusters of varying densities (unlike DBSCAN), and be more robust to parameter selection.


Compared to the DBSCAN algorithm HDBSCAN has made several optimizations as follows:

1. Defines a way to measure the mutual reachability distance between two points from each other with the following formula:
$$
d_{\text {mreach }-k}(a, b)=\max \left\{\text { core }_k(a), \text { core }_k(b), \text { distance }(1, b)\right\}
$$
According to the [original article](https://ui.adsabs.harvard.edu/abs/2015arXiv150606422E/abstract), it has been shown that using this distance metric can effectively satisfy the Hartigan consistency condition

Use the minimum spanning tree to build a hierarchical number model between points, introducing the idea of hierarchical clustering, and at the same time the minimum spanning tree pruning of the smallest child tree to do restrictions, mainly to control the generation of class clusters not too small

3. defines a splitting measure called stability

Personally, I understand that the biggest advantage of HDBSCAN over DBSCAN is that there is no need to choose the domain radius R and MinPts manually, most of the time only the size of the minimum generating class cluster can be chosen, and the algorithm can automatically recommend the optimal cluster class result. It also defines a new distance measure that can better reflect the density of points.

HDBSCAN (Hierarchical Density-Based Spatial Clustering of Applications with Noise) is an unsupervised machine learning method that is used for clustering. It can be used to identify galaxy clusters from a catalog of astronomical data. The method is based on density, which means it groups together data points that are closely packed together (dense), and separates data points that are far apart.

Application of HDBSCAN to find galaxy clusters:

Before starting with HDBSCAN, you need to have a structured dataset which typically includes parameters like right ascension (RA), declination (Dec), redshift (z), etc., of the galaxies. The process of finding galaxy clusters can be described as follows:

Data Preparation: Ensure your data is clean and in the right format. Remove any null or irrelevant values. The dimensions used for clustering in this case could be the coordinates of the galaxies (e.g., right ascension and declination) or some transformed coordinates (e.g., redshift space).

Feature Scaling: This step might be necessary if the features have different scales. The common methods of feature scaling include normalization and standardization.

Apply HDBSCAN: Apply the HDBSCAN algorithm to the dataset. HDBSCAN has two main parameters, min_cluster_size and min_samples. The min_cluster_size is the minimum number of points required to form a cluster, and min_samples is the number of points to be included in the cluster's core. The choice of these parameters may affect the number and size of clusters that you find, and they may need to be tuned depending on the specifics of your dataset.

Interpret the results: The output of HDBSCAN is a set of labels that indicate the cluster to which each data point is assigned. Noise points that are not assigned to any cluster are typically given a label of -1. You can visualize the results using a scatter plot or similar, with different clusters shown in different colors.

Limitations of HDBSCAN:

Choosing parameters: The biggest challenge with HDBSCAN is the selection of the min_cluster_size and min_samples parameters. These parameters can greatly influence the results, but there is no universally "correct" choice, it depends on the specific dataset and the specific scientific question you are trying to answer.

Handling of noise: HDBSCAN can be sensitive to noise, and it treats less dense regions as noise. This can be a problem if your dataset has a lot of noise, or if it has clusters of varying density.

Interpretability: The algorithm can sometimes produce complex hierarchical clusters that are hard to interpret, especially in higher dimensional spaces.

Performance: HDBSCAN has a time complexity of O(N log N) which makes it efficient for small to medium-sized datasets but can become a bottleneck when dealing with large datasets.

Assumption of density: HDBSCAN assumes that clusters are formed by regions of high density separated by regions of low density. This might not always be the case, especially in astronomical data where galaxy clusters could be shaped by other factors like gravitational forces, dark matter, etc.

Remember, the quality of clustering always depends on the nature of the data, the feature representation, and the specific task at hand.


<!-- Headings are cool
======

You can have many headings
======

Aren't headings cool?
------ -->