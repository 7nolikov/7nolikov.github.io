---
title: B-Trees and database indexes
date: 2024-12-21
categories: [databases]
---

PlanetScale on why database indexes are B-trees and not something simpler.

The part worth keeping: a B-tree keeps keys sorted and balanced, so a lookup costs a handful of disk reads instead of a full scan. Range queries - everything between two values - come almost free, because the keys already sit in order next to each other.

That last property is why `WHERE id BETWEEN 100 AND 200` is cheap on an indexed column, and why a hash index would not help you there at all.

[B-trees and database indexes](https://planetscale.com/blog/btrees-and-database-indexes)

See also:

- {{< backlink "cmu-advanced-database-systems" >}}
- {{< backlink "postgresql-exercises" >}}
