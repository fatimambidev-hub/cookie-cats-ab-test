# Experiment Brief: Cookie Cats Gate Placement
Status: Draft v2 (Phase 1). Written before looking at the data.
Data source: Kaggle "Cookie Cats" dataset (about 90,189 players). Facts about the dataset below come from its public description; they are verified against our own file in Phase 2.
---
## 1. Product problem
Cookie Cats is a mobile puzzle game. Players eventually reach a *gate* that forces them to wait or make an in-app purchase to continue. The first gate was originally at level 30. The experiment moved it to level 40.
Decision to make: Should the first gate stay at level 30 or move to level 40?
The question is neutral: moving the gate may improve, worsen, or not change player outcomes. We do not assume it helps.
## 2. Variants
| Group | `version` value | What the player experiences |
|---|---|---|
| Control | `gate_30` | First gate at level 30 (current experience) |
| Treatment | `gate_40` | First gate at level 40 |
## 3. Unit of randomization
The player (`userid`). According to the dataset description, each player was randomly assigned to `gate_30` or `gate_40` at the moment of installing the game. Outcomes are measured per player, so the unit of randomization and the unit of analysis are the same.
## 4. Why randomization matters
Random assignment makes the two groups comparable *on average* before the treatment is applied, so a later difference can be attributed to the gate and not to who ended up in which group.
Counter-example: if existing players were put in `gate_30` and new players in `gate_40`, a retention difference could come from the gate or from the fact that existing and new players behave differently. That is a confounding problem.
Randomization does not guarantee perfectly balanced groups; it makes imbalance random. We cannot observe how assignment was implemented, so in Phase 3 we check the group sizes (Sample Ratio Mismatch check) as a sanity test.
## 5. Outcome variables
| Variable | Meaning (per source) | Notes |
|---|---|---|
| `retention_1` | Did the player come back and play 1 day after install? | Short-term retention |
| `retention_7` | Did the player come back and play 7 days after install? | Longer-term retention |
| `sum_gamerounds` | Game rounds played in the first 14 days after install | Activity signal, not a quality signal |
Assumptions and cautions:
- The source does not say whether `retention_7` means "returned on day 7" or "returned within 7 days". We treat it as written ("1 / 7 days after install") and do not over-interpret it.
- `sum_gamerounds` covers 14 days while retention covers day 1 and day 7. The windows differ.
- More rounds is not automatically better. A gate is designed to create a break, so fewer rounds in one group is not necessarily bad. Retention is the more direct signal.
## 6. Product hypothesis (plain language)
Moving the first gate from level 30 to level 40 may change player retention, because players play longer before they hit the first forced break. We do not claim in which direction, or that Gate 40 is better. That is what the experiment tests.
The formal statistical hypotheses (H0, H1), the single primary metric, and alpha are not fixed here. They are written in `docs/analysis_plan.md` (Phase 4), before running any test.
## 7. Data limitations
The five columns in the dataset do not include:
- revenue, in-app purchases, ARPU, or paying-conversion;
- advertising revenue and player acquisition cost;
- install date or timestamps (so no time trends, novelty effects or day-of-week checks);
- platform, country or other player attributes (so no segment analysis);
- player satisfaction or detailed in-game events;
- how the randomization was technically implemented.
This matters for this specific decision because gates exist partly to drive in-app purchases and to give players an enforced break. A retention gain from Gate 40 would be useful evidence, but this dataset cannot show whether Gate 40 increases or decreases revenue.
## 8. Decision framework
Not: "p < 0.05, therefore ship Gate 40."
Evidence is combined in this order:
`Metric difference -> Statistical evidence -> Effect size and confidence interval -> Product relevance -> Decision`
Three possible outcomes (the exact thresholds are fixed in the analysis plan in Phase 4, before the analysis):
| Outcome | When |
|---|---|
| Move to Gate 40 (conditional) | Clear evidence of improvement on the primary metric, effect large enough to matter, and no harm on supporting metrics. Because revenue is unknown, the recommendation is conditional on a monetization check. |
| Keep Gate 30 | Gate 40 is worse on the primary metric, or shows harm on supporting metrics. |
| Inconclusive | Evidence is uncertain or the effect is practically negligible. The current experience stays by default and more evidence is gathered. "Inconclusive" does not mean "Gate 30 is better". |
Default when in doubt: keep the current experience (Gate 30).