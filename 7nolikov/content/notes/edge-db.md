---
title: EdgeDB - Turbocharged Postgres
date: 2024-12-09
categories: [databases]
---

> Updated: EdgeDB was renamed to **Gel** in February 2025, and it now also speaks plain Postgres SQL. This note predates that - the link goes to the new site.

Postgres underneath, a different data model and query language on top.

You declare a schema with types and links between them, then query with EdgeQL, which returns nested objects instead of joined rows. Fetching an order with its customer and line items is one query and one shaped result, not a join you reassemble in application code.

Type-safe queries and generated client code are the selling point. The cost is a query language nobody on your team knows yet.

[geldata.com](https://www.geldata.com/)

See also:

- {{< backlink "postgresql-exercises" >}}
