---
title: Conventional commits
date: 2024-12-01
categories: [Awesome, Git]
---

## Reasoning

Thousands of times in my professional career I have figured out the reason for changes in the history of the Git. 
And here is the solution to the problem!

## Standardizing Commit Messages

The concept behind Conventional Commits is to provide a rich commit history  
that can be read and understood by both humans and automated tools.

Conventional Commits have the following format:

```code
<type>[(optional <scope>)]: <description>

[optional <body>]

[optional <footer(s)>]
```

**Type**: fix, feat or BREAKING CHANGE, etc.

Here you can find more information about Conventional Commits:

[conventionalcommits.org](https://www.conventionalcommits.org)

Also, tooling available for enforcing this specification, like vscode extensions:

[conventionalcommits.org/en/about/#tooling-for-conventional-commits](https://www.conventionalcommits.org/en/about/#tooling-for-conventional-commits)