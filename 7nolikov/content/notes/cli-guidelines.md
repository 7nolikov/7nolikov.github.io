---
title: Command Line Interface Guidelines
date: 2026-08-21
categories: [tools]
---

An opinionated guide to designing command line tools that people can actually use.

Concrete and immediately applicable: respect `--help`, exit non-zero on failure, write errors to stderr and results to stdout, don't emit colour when piped, confirm before doing something destructive, and make the tool work in a script as well as in a terminal.

The section on output is the one I'd hand to anybody writing an internal tool. Most CLI pain is a program that prints a friendly progress animation into a log file.

[clig.dev](https://clig.dev/)
