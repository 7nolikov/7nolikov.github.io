---
title: HAL - Hypertext Application Language
date: 2025-01-03
categories: [rest]
---

HAL (Hypertext Application Language) is a simple format for representing linked resources in REST APIs. It helps APIs provide structured responses with links, making navigation between resources easier.

## Key Features

- Uses JSON or XML to format API responses.
- Includes hyperlinks (_links) to connect related resources.
- Helps clients discover API functionality without hardcoding URLs.


## Example HAL Response

```json
{
  "_links": {
    "self": { "href": "/orders/123" },
    "customer": { "href": "/customers/456" }
  },
  "id": 123,
  "status": "shipped"
}
```

This response provides details about an order while linking to the related customer.

## Why Use HAL?

1. Standardized structure for APIs.
2. Easier navigation between resources.
3. Reduces hardcoded URLs, making APIs more maintainable.

Read the full HAL specification: : [https://stateless.co/hal_specification.html](https://stateless.co/hal_specification.html)
