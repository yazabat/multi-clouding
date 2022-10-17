+++
author = "ADS"
comments = false
date = "2022-01-01"
draft = false
image = ""
menu = ""
share = false
slug = "sample2"
title= "Business Intelligence"
tags = ["data","blog"]
+++

![Hugo_themes](/blog/images/vis.png)

As reference #1 let’s use a traditional BI-oriented data stack which looks approximately like this:

Google Big Query or any other DWH if we take a different organization is in the middle of the stack. There are multiple data sources and ETL or ELT software that collects data from these data sources and puts it into the DWH. 
Afterward, as soon as data are in the data warehouse, Looker is connected to it for business intelligence and since it is a part of Google Cloud it integrates perfectly with Google Big Query.
dbt is most probably used for in-warehouse data transformation and this process is orchestrated by Apache Airflow.

## So, what can we say about this data stack while looking at it?

First of all, it is pretty much BI and Data Science oriented. Meaning, most probably, the main consumers of data produced and stored by it are data scientists, business analysts, business developers, or managing directors
At the same time, we don’t see anything responsible for data quality, such as, for example, Great Expectations, Monte Carlo Data, or any other similar software. It doesn’t mean the company does not use it, but it is not mentioned in the data stack at least
And there is also a lot of space for real-time operational analytics. ETL, data transformations, and modeling take time and typically operational departments cannot wait for it. Especially if we talk about payment or risk operations from FinTech.


![Hugo_themes](/blog/images/vis_bigquery.png)