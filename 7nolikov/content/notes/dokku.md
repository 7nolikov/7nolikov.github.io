---
title: Dokku - a small PaaS on your own box
date: 2026-08-21
categories: [ops]
---

`git push` to your own server and it builds, containerises and runs your app behind a reverse proxy with a TLS certificate. Heroku's workflow on a VPS you control.

It handles the parts that are tedious to assemble by hand: buildpack or Dockerfile detection, environment variables, zero-downtime restarts, database plugins, Let's Encrypt.

One thing to know before you commit: Dokku officially supports Debian and Ubuntu LTS. Pairing it with an immutable container OS looks appealing and does not work - Dokku installs via apt and owns nginx and the container lifecycle, which is exactly what an immutable OS refuses to let it do.

[dokku.com](https://dokku.com/)

See also:

- {{< backlink "caddy" >}}
- {{< backlink "uncloud" >}}
- {{< backlink "distroless" >}}
