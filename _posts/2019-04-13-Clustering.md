---
title: "Clustering: how are jobs distributed in PA in 2019?"
date: 2021-12-20
published: true
tags: [clustering, k-means, DBScan, hvplot, WAC]
excerpt: "Clustering visualization of jobs distribution in Philly"

hv-loader:
  2_wac_2019_kmeans_type: ["charts/2_wac_2019_kmeans_type.html","1000"]
  2_wac_2019_kmeans_all: ["charts/2_wac_2019_kmeans_all.html","800"]
toc: true
toc_sticky: true
---

## Introduction

We use clustering analysis to find out the patterns of jobs distribution in different block groups. 

The dataset we used is WAC(Workplace Area Characteristic data, jobs are totaled by work Census Block), a subset of LEHD dataset. In the dataset, every row is the numbers of employees in a block group in a year, and the employees are classified in different dimension.

![2_ WAC-data-description-1]({{ site.url }}{{ site.baseurl }}/assets/images/2_ WAC-data-description-1.png)
![2_ WAC-data-description-2]({{ site.url }}{{ site.baseurl }}/assets/images/2_ WAC-data-description-2.png)


## HvPlot Example

We use elbow method and determine 4 clusters in the dataset. 
![2_ elbow]({{ site.url }}{{ site.baseurl }}/assets/images/2_elbow_2019.png)

Below are the overall interactive charts. You can select the factor you are interested in to see how different group differentiate. Note that barplot of the type of job is seperated from the main chart because of space limitation.

Overall factors of 4 clusters:
<div id="2_wac_2019_kmeans_all"></div>

Job types of 4 clusters:
<div id="2_wac_2019_kmeans_type"></div>

![2_Edu&Earning]({{ site.url }}{{ site.baseurl }}/assets/images/2_edu&earning.png)

![2_Ethinity&Race]({{ site.url }}{{ site.baseurl }}/assets/images/2_Ethinity&Race.png)

### Why k-means?
We also tried DBSCAN to do clustering, but compared to K-means, the result of DBSCAN is more complicated. It often defines excessive clusters and doesn't show difference among them. For example, below is a barplot of jobs type in different clusters, which provides little information. 

![2_db]({{ site.url }}{{ site.baseurl }}/assets/images/2_DBScan_2019.png)

## Notes

In 
