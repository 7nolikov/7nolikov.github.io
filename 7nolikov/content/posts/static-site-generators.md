---
title: Choosing a static site generator is the easy part
date: 2026-08-12
categories: [websites]
---

I run three static sites - this blog, a relocation guide, and a sandbox. All Hugo. A few
smaller things sit on Next.js and SolidJS.

The generator turned out to be the least important decision in all of it.

<!--more-->

## They all do the same job

Text files in, templates applied, HTML out. Jekyll, Hugo, 11ty, Astro - the core is
identical. None of them will be the reason your site is good or bad.

They differ on build speed, language and plugin count. That matters at the margin. Hugo is
fast and it's one binary. That's why I use it. I personally don't like Jekyll - Ruby is not a
dependency I want for a blog. That is about as deep as this decision needs to go.

The theme is where the work actually is. It's also where things break quietly.

## The CSS that was correct and did nothing

My theme ships a prebuilt CSS bundle. Tailwind, compiled once by the theme author, shipped as
a finished file. Tailwind is not installed in my project at all.

I didn't think about what that means for months.

Here is what it means. Tailwind only emits the classes it finds in the templates it scans.
The author scanned the theme's templates. Not mine. So a class I use in my own override, that
the theme itself never uses, is simply not in the bundle.

My footer had this:

```
<nav class="flex flex-row flex-wrap items-center gap-x-3 gap-y-1">
```

`gap-x-3` and `gap-y-1` are not in the compiled CSS. Zero occurrences. The markup is fine.
The names are spelled right. The browser applies them and they resolve to nothing.

My social links rendered as `GitHubLinkedInBlueskyXReddit`. One word, no spaces. Live, for as
long as that footer existed.

## The fallback that never fired

I found a second one the same day. I like this one more.

Every icon in the theme sizes itself like this:

```
class="lucide lucide-shapes {{ (and (reflect.IsMap .) .class) | default "h-4 w-4" }}"
```

Read it the way it is obviously meant. If a class was passed in, use it. Otherwise fall back
to `h-4 w-4`.

That is not what happens. Call the partial with no argument and `reflect.IsMap` returns false.
`and` short-circuits to the boolean `false`. And Hugo's `default` does not treat `false` as
empty - it's a value. So the fallback never fires and you get:

```
class="lucide lucide-shapes false"
```

`false` is not a CSS class. Icons without a fallback width attribute rendered at their natural
SVG size, which is enormous. Three shapes the size of a paragraph, sitting in the middle of a
post.

I found it by opening my own site on a phone.

## Both of these are the same bug

This is the part worth keeping.

Neither was a typo. Both were written by someone who knew what they wanted. Both look correct
on review. Both produce silence instead of an error.

A class name missing from the stylesheet is not a failure. A fallback that never fires is not
a failure. Nothing logs. The build is green.

A static site has no runtime, so nothing catches it later either. The build succeeds and the
wrong thing ships.

The only thing that finds this is looking at the output. Not the template - the rendered page,
on a real device.

## The 2026 part the guides don't mention

There is a new job that didn't exist when most SSG tutorials were written. Make the site
readable by language models, not just Google.

Three files:

- `robots.txt` naming AI crawlers explicitly - GPTBot, ClaudeBot, PerplexityBot and the rest.
  Named groups matter, some bots only honour their own name.
- `llms.txt` - a plain text summary of who you are and which pages matter. Cheap to write.
- JSON-LD in the head, so the structured facts are machine readable.

I added all three. Then left them alone for two months. Which brings the punchline: my
`llms.txt` said I was based in a country I moved out of in June.

A file that exists to tell machines accurate things about me, quietly wrong, being read by
every crawler that asked. Same shape as the CSS. Written once, correct at the time, never
checked again.

I fixed it while writing this post.

## So which one

Hugo, if you want my answer. One binary, no runtime dependency, builds a hundred pages faster
than you can switch windows. 11ty if you already live in Node. Astro if some pages need
interactive components and the rest don't.

But this choice is not where your site succeeds or fails. Pick one. Learn its template
language properly - that is the part that will bite you. Then open the result on a device you
didn't build it on.

When did you last do that?
