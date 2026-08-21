---
title: Hurl - HTTP requests as plain text
date: 2026-08-21
categories: [testing]
---

Write HTTP requests in a plain text file, run them, assert on the responses. Built on curl.

```hurl
GET https://example.org/api/health
HTTP 200
[Asserts]
jsonpath "$.status" == "ok"
```

Because it is text, the tests diff and review like code, and they run identically on a laptop and in CI. No GUI, no exported collection that drifts from what is committed.

The gap it fills: too much for `curl` in a shell script, far less machinery than a full API client.

[hurl.dev](https://hurl.dev/)
