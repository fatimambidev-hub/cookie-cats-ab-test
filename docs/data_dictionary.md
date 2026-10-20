# Data Dictionary: Cookie Cats A/B Test
File: `data/raw/cookie_cats.csv` (not committed to Git)
Source: Kaggle "Cookie Cats" dataset. Column meanings come from the source description; data types and counts were observed in our own file (Phase 2).
Shape (observed): 90,189 rows x 5 columns. One row per player.
| Column | Observed dtype | Meaning (per source) | Role in analysis |
|---|---|---|---|
| `userid` | int64 | Unique number that identifies each player | Identifier. Used for duplicate checks. Not an analysis variable. |
| `version` | text (str) | Experiment group: `gate_30` = control (gate at level 30), `gate_40` = treatment (gate at level 40) | Group label (the experimental factor) |
| `sum_gamerounds` | int64 | Number of game rounds played during the first 14 days after install | Exploratory / supporting engagement metric. Activity volume, not quality. |
| `retention_1` | bool | Did the player come back and play 1 day after install? | Supporting metric (short-term retention) |
| `retention_7` | bool | Did the player come back and play 7 days after install? | Candidate primary metric (final choice made in Phase 4, before testing) |
## Group sizes (observed in our file)
| `version` | Players | Share |
|---|---:|---:|
| `gate_30` (control) | 44,700 | 49.56% |
| `gate_40` (treatment) | 45,489 | 50.44% |
| Total | 90,189 | 100% |
The counts match the figures published in public notebooks for this dataset. The two groups add up to the total number of rows, so every row has one of the two expected `version` values.
## Notes and open points
- Missing values: all five columns have a non-nullable dtype (`int64` or `bool`) after loading. This suggests no missing values in the numeric and boolean columns, but the formal check is done in Phase 3.
- Duplicates: uniqueness of `userid` has not been checked yet (Phase 3).
- Range of `sum_gamerounds`: minimum, maximum and outliers have not been inspected yet (Phase 3).
- Group balance: the formal Sample Ratio Mismatch test, with its threshold set beforehand, is done in Phase 3.
- `retention_7` definition: the source does not say whether it means "returned exactly on day 7" or "returned within 7 days". We use the source wording and do not interpret it further.
- Windows differ: `sum_gamerounds` covers 14 days; retention flags refer to day 1 and day 7 after install.
- Not in the dataset: revenue, purchases, install date, platform, country, or any player attribute (see `experiment_brief.md`, section 7).