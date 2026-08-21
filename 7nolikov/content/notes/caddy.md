---
title: Caddy
date: 2026-08-21
categories: [ops]
---

A web server that gets HTTPS right by default. Point it at a domain and it obtains and renews the certificate itself, with no cron job and no certbot.

A whole reverse proxy config is two lines:

```
example.com {
	reverse_proxy localhost:8080
}
```

That is the entire file. The equivalent nginx config is a block of boilerplate plus a separate certificate renewal setup that you will forget about until it expires on a weekend.

Written in Go, ships as a single binary.

[caddyserver.com](https://caddyserver.com/)

See also:

- {{< backlink "dokku" >}}
