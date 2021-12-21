---
title: "Clustering: how are jobs distributed in PA in 2019?"
date: 2021-12-20
published: true
tags: [clustering, k-means, DBScan, hvplot, WAC]
excerpt: "Clustering visualization of jobs distribution in Philly"
altair-loader:
  altair-chart-1: "charts/measlesAltair.json"
hv-loader:
  hv-chart-1: ["charts/measlesHvplot.html", "500"] # second argument is the height
  hv-chart-2: ["charts/phl_bg_map.html", "500"]
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


## Altair Example

Below is a chart of the incidence of measles since 1928 for the 50 US states.

<div id="altair-chart-1"></div>

This was produced using Altair and embedded in this static web page. Note that you can also display Python code on this page:

```python
import altair as alt
alt.renderers.enable('notebook')
```

## HvPlot Example

Lastly, the measles incidence produced using the HvPlot package:

<div id="hv-chart-1"></div>

u1s1

<div id="hv-chart-2"></div>

Heyhey check this out!

<div id="2_wac_2019_kmeans_type"></div>

These are other types:
<div id="2_wac_2019_kmeans_all"></div>

## Notes

- See the [raw source code](https://raw.githubusercontent.com/MUSA-550-Fall-2020/github-pages-starter/master/_posts/2019-04-13-measles-charts.md) of this post for details on how these charts were embedded.
- See the [lecture 13A slides](https://github.com/MUSA-550-Fall-2020/week-13/blob/master/lecture-13A.ipynb) for the code that produced these plots.

**Important: When embedding charts, you will likely need to adjust the width/height of the charts before embedding them in the page so they fit nicely when embedded.**