---
layout: post
title: Data Engineering Week 3 - Data Warehouse 
date: 2024-01-01 08:57:00-0400
description:  Data Warehouse and BigQuery
tags: data-engineering
categories: data
giscus_comments: false
related_posts: false
toc:
  beginning: true
---

### **OLAP vs OLTP**

`OLAP (Online Analytical Processing)`:

Purpose: OLAP is designed for performing multi-dimensional analysis on large datasets quickly and interactively.

Data Source: Typically utilizes data from data warehouses, data marts, or centralized data stores.

Usage: Ideal for data mining, business intelligence, complex analytical calculations, and business reporting (e.g., financial analysis, budgeting, sales forecasting).

Data Model: Based on a multidimensional data model, organizing data into dimensions (e.g., time, geography, product) and measures (e.g., sales revenue, profit margin).

Capabilities: Enables users to explore data from different perspectives, perform complex calculations, and analyze data at various levels of granularity.

Volume of Data: OLAP has large storage requirements. Think terabytes (TB) and petabytes (PB).

Example Applications: OLAP is good for analyzing trends, predicting customer behavior, and identifying profitability.


`OLTP (Online Transaction Processing)`:

Purpose: OLTP is designed for managing and processing large volumes of transaction-oriented data in real-time, supporting day-to-day operational tasks.

Data Source: OLTP uses real-time and transactional data from a single source.

Usage: Critical for operational systems and applications where high availability and fast response times are essential.

Data Model: Based on a transactional data model, optimized for recording and processing individual transactions. Data is typically organized into tables.

Volume of Data: OLTP has comparatively smaller storage requirements. Think gigabytes (GB).

Example Applications:OLTP is good for processing payments, customer data management, and order processing.

In summary, OLAP and OLTP serve different purposes in the data management landscape, with OLAP focusing on analytical tasks and decision-making, while OLTP is geared towards supporting real-time transaction processing for day-to-day business operations.

### **Key differences**

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/blog/data/week3/difference.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
   OLAP vs OLTP
</div>


### **Partitioning and Clustering in BigQuery**

Partition is a huge advantage so that Big Query doesn’t need to run over the whole table. For example, when most of the queries are based on date and use it as a filter. You can partition a table based on Integer column, or time-unit column, or ingestion time.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/blog/data/week3/partition.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
   Partitioning
</div>

Clustering tables means sort by clustered columns. In this way it can also improve query performance and reduce query costs.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/blog/data/week3/clustering.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
   Clustering
</div>

```sql
-- Create a partitioned table from external table
CREATE OR REPLACE TABLE `dez-de-404011.dezoomcamp.rides_partitioned`
PARTITION BY
 DATE(pickup_datetime) AS
SELECT * FROM `dez-de-404011.dezoomcamp.external_yellow_tripdata`;


-- Creating a partition and cluster table
CREATE OR REPLACE TABLE `dez-de-404011.dezoomcamp.rides_partitoned_clustered`
PARTITION BY DATE(pickup_datetime)
CLUSTER BY affiliated_base_number AS
SELECT * FROM `dez-de-404011.dezoomcamp.external_yellow_tripdata`;
```

### **BigQuery Best Practices**

Cost reduction

- Avoid SELECT *
- Price your queries before running them
- Use clustered or partitioned tables
- Use streaming inserts with caution
- Materialize query results in stages

Query performance
- Filter on partitioned columns
- Denormalizing data
- Use nested or repeated columns
- Use external data sources appropriately
- Reduce data before using a JOIN
- Do not treat WITH clauses as prepared statements
- Avoid oversharding tables
- Avoid JavaScript user-defined functions
- Use approximate aggregation functions (HyperLogLog++)
- Order Last, for query operations to maximize performance
- Optimize your join patterns
- As a best practice, place the table with the largest number of rows first, followed by the table with the fewest rows, and then place the remaining tables by decreasing size.