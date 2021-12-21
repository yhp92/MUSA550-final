---
title: "Introduction"
date: 2021-12-119T15:34:30-04:00
excerpt: ""
categories:
  - Introduction
tags:
  - Employment
  - Data analysis
---
![assets/images/chesterbrook.jpg](Aerial image of Chesterbrook business park CBD, Philadelphia)

# An analysis of homes and jobs in the Philadelphia metro region

Over the past few years, an increasing number of researchers are exploring the spatial structure of employment centers in American cities. Even as cities generally have experienced an urban revitalization movement to some degree or another, only a select few cities proper have been able to truly capture new offices, labs, and warehouses in the building boom. The rest must contend with an employment base that is staunchly intertial and content to stay largely in the same vicinity — and for most of the country over the past half-century, that means sticking to some degree of suburban decentralzation.

The general question that must be asked at this moment is: Across any metro area, how can employment be characterized? Is there something quantifiable we can say that differentiates Silicon Valley from Chicago's Loop, and everything in between? And how much harder has it been for workers to travel to commute to their workplace (and how much time can they save with a post-pandemic work-from-home option)?

The main data source used comes from the Census Bureau's Longitudinal Employer-Household Dynamics (LEHD) program, managed by the Bureau's Center for Economic Studies. Among the datasets published is Origin-Destination Employment Statistics (LODES). This relatively unknown dataset is high-resolution, comprehensive across nearly the entire US, and comprises over a decade of historical data. It is compiled every year since 2002, and contains the point data for a resident's home and workplace at the Census block group level, and accompanying characteristics. This makes it not only very extensible through joining with typical Census demographic data, but also very powerful in its own right in subsequent O-D analysis (which the Census Bureau also conducts through OnTheMap web application).

Using LODES data, we will be able to extract broad insights about the changing patterns in American cities. First, as stated above, we can both characterize and classify the diverse set of business districts, and analyze the increasing spatial mismatch between where people live and where they work. Researcher Robert Manduca has done [pioneering research](https://journals.sagepub.com/doi/pdf/10.1177/2399808320934821) into the spatial structure and classification of business districts in urban areas

Regarding the home-workplace spatial mismatch (otherwise known as job sprawl), research has been rapidly growing as well, [The Brookings Institution](https://gspp.berkeley.edu/assets/uploads/research/pdf/p60.pdf) for example notes that the poor are more suburbanized in areas with higher rates of job sprawl. Job sprawl is associated with increased levels of commuting by car and overall longer commute times, resulting in more congestion. This results in the total number of jobs available for people within a given radius also decreasing, leading to more urban sprawl in general as people self-segregate to be near their employment pool and industry of choice. Thus, understanding how much Philadelphia has sprawled over the past decade is important to understanding how much of a factor it plays in hindering its overall economic success.

The report is outlined as follows:

1. Exploratory data analysis of the LODES data
2. Clustering analysis of workplace/business districts
3. Origin-destination visualizations and network flow graph
4. Regression analysis on job density (This will be completed at a later date)
