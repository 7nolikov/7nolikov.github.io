---
title: HAL - Hypertext Application Language
date: 2025-01-03
categories: [architecture]
---

A tiny convention for putting links inside JSON API responses, so clients navigate instead of building URLs by hand.

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

That's essentially the whole spec. The client follows `_links.customer` instead of knowing that customers live at `/customers/{id}` - so the server can move things without breaking anyone.

In practice most APIs skip this and hardcode URL patterns on the client. Worth knowing what the alternative looks like before deciding that.

[HAL specification](https://stateless.co/hal_specification.html)
