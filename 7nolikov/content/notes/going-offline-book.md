---
title: Book - Going Offline
date: 2024-12-26
categories: [architecture]
---

Jeremy Keith's book on service workers and making a site work without a connection.

It is short and it is the clearest explanation of service workers I've come across. The core idea: a service worker is a script sitting between your page and the network, and once you accept that, caching strategies stop being magic - you decide per request whether to try the network first, the cache first, or race them.

Worth reading even if you never ship a PWA, because it explains what that layer actually does.

[goingoffline.adactio.com](https://goingoffline.adactio.com/)
