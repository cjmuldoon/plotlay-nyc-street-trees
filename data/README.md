# The data

Made from NYC Open Data (no restrictions on use, see [`../NOTICE`](../NOTICE)); our tables and shapes are CC BY 4.0.

- `tables/trees_by_census.csv`: one row per neighbourhood and census: `nta`, `census`, `trees` (live street trees placed in it), `ntaname`, `boroname`, `area_km2`, `trees_per_km2`.
- `tables/change_1995_2015.csv`: one row per neighbourhood: trees in each census, `change` and `change_pct` 1995 to 2015.
- `tables/species.csv`: the species most common in 2015 and the Norway maple: trees and share in each census.
- `tables/citywide.csv`: per census: records, live trees, trees with no position, trees placed by the area the census named, trees in no neighbourhood, trees in parks, cemeteries and airports.
- `trees/east_new_york_trees.csv`: every live tree with a position in BK82, BK83 and BK85, each census: `id` (the census's own), `census`, `lat`, `lng`, `species`, `dbh_in` (trunk diameter, inches), `nta`.
- `geometry/neighbourhoods.*`: the 188 neighbourhoods, keyed by `nta` (`key` in the files).
