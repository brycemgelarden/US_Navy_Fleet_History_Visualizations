# NHHC U.S. Navy Ship Force Levels, 1886–2016: tidy extract

**Source:** Naval History and Heritage Command, "US Ship Force Levels, 1886–present,"
https://www.history.navy.mil/research/histories/ship-histories/us-ship-force-levels.html
Saved from a browser on 2026-11-12 (`US Ship Force Levels.html`). Scripts can't fetch the live site because it uses DoD-issued TLS certificates, and it also returns empty pages to automated requests.

## Files
- `nhhc_ship_force_levels_long.csv`: one row per **year × category** (tidy/long format), 1,718 rows.
- `nhhc_ship_force_levels_notes.csv`: the NOTES and EVENTS boxes that sit under each source table (29 entries). Useful for annotations.
- `US Ship Force Levels.html`: the raw saved page (keep it for reproducibility).

## Columns (long file)
| column | meaning |
|---|---|
| `year` | Year of the snapshot (one snapshot per year, 131 years) |
| `snapshot_date` | Actual date of the count. **Not consistent across eras:** Dec 31 (1886–1916), wartime dates (e.g. 1917-04-06, 1918-11-11, 1941-12-07, 1945-08-14), July 1 (1919–1937), June 30 (1938–1977), Sept 30 (1978–2016) |
| `counting_method` | `total_active` (1886–2010) or `battle_force` (2011–2016). **These two methods aren't directly comparable.** |
| `category` | Harmonized ship category (the source labels change by era; see `raw_labels`) |
| `row_type` | `ship_type`, `subtotal` (e.g. Surface warships, Steel/Old navy), or `total` |
| `count` | Active ships. `-` and blank cells are treated as 0. Footnote symbols are stripped. |
| `reduced_commission` | Ships in reduced commission ("rc", 1921–1934). The source **excludes** these from active totals, and so does `count`. |
| `footnote_marks` | Footnote symbols removed from the cell (`*`, `^`, `@`, …). Look them up in the notes file. |
| `raw_labels`, `raw_values` | Original row label(s) and cell text, for auditing. When several source rows were merged (e.g. SSBN + SSGN), they're separated by `|`. |

## Decisions to discuss in Methodology (Parse / Filter)
- When several source rows map to one category, their counts are **summed**. For example, 2011–2016 detail rows are rolled up into Destroyers, Amphibious, and Auxiliary & support. SSBN, SSGN, and SSG are combined into "Missile submarines."
- "Submarines (attack/general)" means **all** submarines before 1951. From 1951 on, missile submarines are counted separately.
- Don't add `subtotal` and `total` rows to `ship_type` rows, or you'll double-count. Filter on `row_type == "ship_type"` to get the parts.

## Validation
For each year, the sum of `ship_type` rows equals the published total in 129 of 131 years. The two exceptions are 1922 (379 published vs 378 summed) and 1923 (365 vs 364). Both are off by one in the source table itself.
