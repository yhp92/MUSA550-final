---
title: "Clustering: how are jobs distributed in PA in 2019?"
date: 2021-12-20
published: true
tags: [clustering, k-means, DBScan, hvplot, WAC]
excerpt: "Clustering visualization of jobs distribution in Philly"

hv-loader:
  2_wac_2019_kmeans_type: ["charts/2_wac_2019_kmeans_type.html","1000"]
  2_wac_2019_kmeans_all: ["charts/2_wac_2019_kmeans_all.html","1000"]
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
<div id="2_wac_2019_kmeans_type"></div>

Job types of 4 clusters:
<div id="2_wac_2019_kmeans_all"></div>



## Notes

- See the [raw source code](https://raw.githubusercontent.com/MUSA-550-Fall-2020/github-pages-starter/master/_posts/2019-04-13-measles-charts.md) of this post for details on how these charts were embedded.
- See the [lecture 13A slides](https://github.com/MUSA-550-Fall-2020/week-13/blob/master/lecture-13A.ipynb) for the code that produced these plots.

**Important: When embedding charts, you will likely need to adjust the width/height of the charts before embedding them in the page so they fit nicely when embedded.**
