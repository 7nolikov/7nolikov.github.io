---
title: Free hosting cost me a kernel panic
date: 2026-07-15
categories: [cloud]
---

I run a blog, a relocation guide and four sandbox sites. All of them are free, and they stay
free. I am careful about it.

I got careful the hard way.

<!--more-->

## The expensive way to save money

In January I had a side project on a small VPS. I didn't want to pay for a managed database,
so I self-hosted Supabase on the same box - gotrue and Postgres in containers, next to the
application. Then a second Postgres arrived from another compose file. The node had 3.7 GB
of RAM and no swap at all.

It died. OOM killer, then a kernel panic. The whole host, not just the container.

The database and the app were on the same machine, so there was nothing left to serve
anything. Hosting was free. The evening was not.

By that point I had twenty Ansible playbooks for that server. Some of the names:

```
fix-firewall.yml
diagnose-supabase.yml
check-supabase-state.yml
verify-supabase-deployment.yml
```

Half of my automation existed to debug the other half. That is the real signal. When your
ops tooling starts generating its own workload, the problem is the architecture, not the
tooling.

Three rules came out of that night and I still follow them:

- The database does not live on the app box.
- Swap is not optional.
- The cheapest high availability is not having state on the machine.

**Free that you operate yourself is not free.** You pay in evenings. I paid a whole one, and
I still had to move the database off the box afterwards - which is what I should have done
at the start.

## The only question for managed free tiers

So now I use managed things. And for those there is exactly one question I care about:
what happens when I cross the limit?

There are two answers.

**Suspend.** The service stops. My site is down until the next cycle or until I upgrade.
I lose availability. I pay nothing.

**Overage.** The service keeps running and bills me. I keep availability. I find out the
price later.

For a blog, a landing page, a sandbox - suspend is correct. Nobody dies because my demo is
down for a day. An unexpected invoice is a real problem, and avoiding it was the entire
point.

I don't compare free tiers by their limits anymore. I compare them by what they do when I
hit the limit.

## What I run now

**GitHub Pages** for anything static - this blog, the relocation guide, the sandboxes. There
is no billing relationship at all, so there is no configuration where it can start charging
me. Static only, no backend. For most of what I build that is not a limitation.

**Cloudflare** when I need compute. One of my landing pages runs there. The free tier is
large, but that is not why I keep going back - it's egress. Every byte leaving AWS or GCP
costs money, and egress is the line that grows quietly while you don't watch it. Cloudflare
doesn't charge for it. For a small product that one difference decides the math.

**Neon** for Postgres. Scale-to-zero, and a branch per experiment, so a new idea costs
roughly nothing to stand up. Cold start is a few hundred milliseconds, which doesn't matter
behind a connection pool. Most importantly it is not on my app box.

**PostHog** for analytics, chosen for exactly one property: a permanent free allowance and a
hard $0 billing cap. Not "generous limits" - a cap. I set it once and stopped thinking.

The pattern is not "who gives the most". It is "who cannot bill me by accident, and who I
don't have to operate".

## Things that get people

**Inactivity.** Many free backends sleep without traffic. The first request after that takes
seconds. If that request is a demo you sent to somebody, it's the first impression.

**Egress.** Almost never in the headline numbers. Always in the invoice.

**One site takes down the account.** On some platforms a single project exceeding its limit
suspends everything else on the same account.

**Free databases with an expiry date.** Some free Postgres offerings are deleted after a
fixed number of days if you don't upgrade. Free storage and durable storage are different
products.

## One honest warning

I'm not putting a table of current limits here. Providers change limits, rename plans and
move features between tiers constantly - any such table is wrong within months. The failure
mode is the durable part. Check that yourself, on the billing page, before you deploy
something you care about.

And a warning about the other direction. Neon, PostHog and the rest send very good
newsletters, and reading them makes adopting tools feel like progress. It isn't. The filter
I use now: does this change something that is already in front of users? If no, it's reading
material, not a task.

How do you keep side projects from producing a surprise bill? I don't trust alerts on their
own. I'd rather the thing just stops.

See also:

- {{< backlink "posthog" >}}
- {{< backlink "uncloud" >}}
- {{< backlink "dokku" >}}
- {{< backlink "caddy" >}}
