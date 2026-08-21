---
title: 100 Go Mistakes
date: 2026-08-21
categories: [golang]
---

A catalogue of the mistakes Go developers actually make, with the reasoning behind each one. Free to read online, and also a book.

The value is not the individual gotchas. It's that most of them cluster around a few themes: slices sharing backing arrays, loop variable capture, goroutine lifetimes nobody owns, and interfaces holding a nil pointer that is not `nil`.

Read it once early and you will recognise the shapes later, which is the whole point.

[100go.co](https://100go.co/)

See also:

- {{< backlink "learn-go-with-tests" >}}
- {{< backlink "go-synctest" >}}
