---
title: JSON API - RESTful API Specification
date: 2025-01-04
categories: [architecture]
---

A specification for what a JSON API response should look like, so you stop re-deciding it on every project.

It settles the boring arguments - where errors go, how pagination and filtering are expressed, how related resources are included. Sparse fieldsets and compound documents are the two features that earn it: the client asks for exactly the fields it needs, and pulls related objects in one round trip instead of five.

Heavier than most APIs need. But if you are about to invent your own envelope format, read this first.

[jsonapi.org](https://jsonapi.org/)

See also:

- {{< backlink "hal" >}}
- {{< backlink "rest-api-tutorial" >}}
