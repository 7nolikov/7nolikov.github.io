---
title: Conventional commits
date: 2024-12-01
categories: [processes]
---

Thousands of times in my professional career I have figured out the reason for changes in the history of the Git. And here is the solution to the problem!

```
<type>[(optional <scope>)]: <description>

[optional <body>]

[optional <footer(s)>]
```

`feat`, `fix`, `docs`, `refactor`, `chore`, and `BREAKING CHANGE` in the footer.

The convention pays off twice. A human scanning `git log` sees what kind of change each commit was without opening it. And tooling can read the same thing - changelogs generate themselves, and the version bump falls out of the types: a `fix` is a patch, a `feat` is a minor, a breaking change is a major.

It costs nothing to start. You just write commits differently from tomorrow.

[conventionalcommits.org](https://www.conventionalcommits.org) · [tooling](https://www.conventionalcommits.org/en/about/#tooling-for-conventional-commits)
