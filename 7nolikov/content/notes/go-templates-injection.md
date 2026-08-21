---
title: Go Templates injection solved
date: 2025-05-09
categories: [golang]
---

I personally encountered a Go templates injection problem in a project I was working on.

Short version of the fix: use `html/template`, never `text/template`, for anything that reaches a browser. They have identical APIs, which is exactly the trap - the code compiles either way. `html/template` escapes values based on where they land, so a value inside an attribute, inside a script tag and inside plain text all get escaped differently.

The guide below covers the rest of the basics - layouts, data binding, reusable blocks - with working code.

[Learn Go Templates](https://blog.logrocket.com/learn-go-templates-a-practical-guide-to-layouts-data-binding-and-rendering/) · [code example](https://github.com/nilotpaul/tutorials/blob/main/golang/mastering-golang-templates-a-practical-guide-to-layouts-data-binding-and-rendering/main.go)
