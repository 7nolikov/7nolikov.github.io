---
title: Delta Lake vs Apache Iceberg
date: 2024-12-20
categories: [databases]
---

Two table formats that sit on top of files in object storage and give you ACID transactions, schema evolution and time travel - the things a pile of Parquet files does not have on its own.

**Delta Lake** came out of Databricks and is deepest there and in Spark.

**Iceberg** is engine-agnostic by design and runs under Spark, Flink, Trino and more. Its hidden partitioning is the feature to know: you partition by a column, and readers don't have to know the partition scheme to hit it. With traditional partitioning, a query that doesn't mention the partition column scans everything.

Rough rule: heavy Databricks or Spark shop, Delta Lake. Multiple engines, Iceberg.

Both have moved a lot since this was written - check the current state before betting on it.

[Delta Lake vs Apache Iceberg. The Lake House Squabble](https://dataengineeringcentral.substack.com/p/delta-lake-vs-apache-iceberg-the)
