---
title: I use Make in every project and never for building
date: 2026-06-24
categories: [build]
---

I don't use Meson. I have never used Meson.

I have a Makefile in every project I own. Not one of them builds anything.

<!--more-->

## What Make was actually for

Make solves a specific problem. Source files produce object files. Object files link into a
binary. Some sources changed, most didn't. Recompiling everything takes an hour you don't
have.

So you declare a dependency graph. Make compares timestamps. Only the stale parts rebuild.
That is the whole idea, and in 1976 it was worth a lot.

Now look at the first line of my Makefile:

```make
.PHONY: up dev down demo demo-error demo-chaos demo-replay logs push test lint build clean verify verify-infra verify-demo verify-all
```

Sixteen targets. All phony. None of them produces a file - so there are no timestamps to
compare and no graph to walk.

The mechanism Make exists for is switched off. In every project I have.

## Something else does that job now

`go build` tracks its own dependencies and caches compiled packages. So does Cargo. So does
Maven, npm, every toolchain written in the last twenty years. They know their language's
import graph. Make never did - it knows files and timestamps, and you had to describe the
graph by hand.

That job is gone. It didn't move to Meson. It moved into the language toolchains.

## What I use it for instead

A menu.

```
make up       # start the stack
make test     # unit gates
make demo     # push 5 records through
make lint
```

That's it. Named commands, discoverable, tab-completable.

The part I actually value is that they're identical across projects in different languages.
`make test` works whether the project is Go, Java or a static site. I don't have to remember
which runner each one uses. A small thing that saves a small amount of thought, many times a
day.

## Where it stops being fine

Make is a bad programming language. Tabs are syntax. Every line runs in its own shell, so
variables don't survive between them. `$` means one thing to Make and another to the shell,
so half your escaping is `$$`. Conditionals hurt.

I hit that wall. One of my Makefiles is 125 lines and does nothing but dispatch. The actual
logic - test tiers, output summarising, failure excerpts - lives in a 256-line shell script
next to it.

That split is the rule. **The moment a target needs an `if`, move it to a script and leave
Make as the dispatcher.** A Makefile with real logic in it gets worse quietly.

While writing this I noticed something else. My Makefile has sixteen `## target: description`
comments, written for a `help` target I never wrote. Sixteen tidy descriptions that nothing
prints.

## So, Meson

Meson is a real build system. It competes with CMake and Autotools, generates Ninja files,
and it is genuinely better than a hand-written Makefile at compiling C and C++. Git moved its
build to Meson in 2.48, and Git is a large C project - exactly the case where the comparison
means something.

If you compile C or C++, take it seriously. Meson is fast, the syntax is readable, and the
error messages beat the alternatives.

If you write Go, Rust, TypeScript or Python - you don't have the problem Meson solves. You
have a task-runner problem. Much smaller. Make is fine for it. `just` and `task` are nicer if
you want real arguments and no tab syntax. Make's advantage is that it is already installed,
and everyone knows what `make test` means.

Meson versus Make is a real question. It's just not the one most of us are asking.

What does `make test` do in your repo - and does it still work?

See also:

- {{< backlink "modern-make" >}}
- {{< backlink "cli-guidelines" >}}
