# Who Subscribes? A Pre-Call Look at Bank Term-Deposit Marketing

**Avaloka Engineering Intern Hackathon, 14-15 Nov 2026** | Poorvika N P

## Question

Which customers should a bank call first, using only information it has **before** the call?

## Data

UCI Bank Marketing (`bank-additional-full.csv`, Moro et al. 2014): 41,188 customers, 21 columns, from a Portuguese bank's phone campaigns.
Target `y` = subscribed to a term deposit. **11.3% yes (4,640) / 88.7% no (36,548).** A model that always says "no" is already 88.7% accurate, so accuracy alone is misleading here.

## How I used Avaloka

1. Converted the semicolon-separated file to a comma CSV and uploaded it to avaloka.ai.
2. Read Auto Insights (readiness 82/100, three warnings: class imbalance, pdays uniformity, duration leakage risk).
3. Skipped the upload-time auto-clean, because the platform did not expose its cleaning rules. Figures therefore match the original file.
4. Asked one grouped question at a time ("group by X, give the number of rows and the share where y is yes"), then trained a model and read Avaloka's own checks.

## Findings

**1. `duration` is a leakage trap, so I excluded it.** Auto Insights flagged it: calls average 553 s for "yes" vs 221 s for "no" (2.5x). But call length is only known after the call, so it cannot guide who to call. No analysis below uses it, and Avaloka confirmed the 19 model features do not include it.

**2. Prior campaign history is the strongest pre-call signal.**
- 96.3% of rows have `pdays = 999`, a code for "never contacted" (the mean of 962 is meaningless).
- The ~1,515 previously contacted customers (`pdays < 999`) subscribe at **63.83%** (95% CI 61.38-66.21) vs **9.26%** (CI 8.98-9.55) for never contacted.
- By prior outcome: success **65.1%** (1,373 rows), failure 14.2% (4,252), nonexistent 8.8% (35,563).
- A cross-tab shows these are largely one signal: 1,373 of the 1,515 `pdays < 999` customers (about 91%) are prior successes.
- Data quirk: 4,110 of 4,252 prior-failure customers (96.7%) have `pdays = 999`, so `pdays` under-records prior contact.

**3. Cellular beats landline.** 14.7% vs 5.2% (about 3x).

**4. Job.** Student 31.4% (875 rows) and retired 25.2% (1,720 rows) convert best; blue-collar is lowest at 6.9% (9,254 rows). The largest group, admin. (10,422 rows), is near average at 12.97%. The high-rate jobs are small groups.

**5. Month.** May carried 13,769 of 41,188 calls (about 33%) at the lowest yes rate (about 6%). March, September, October and December convert at roughly 44-51% but on small volumes (e.g. December has 182 rows). Month is also entangled with 2008-2010 economic conditions, so I do not treat it as a scheduling rule.

**6. A model did not help.** I trained Avaloka's logistic-regression classifier on the 19 pre-call features (all columns except `duration` and `y`) with a deterministic 80/20 row-hash split over 41,188 rows. Validation accuracy was **0.840 vs 0.887** for the majority-class baseline. Avaloka's evidence layer reported **"Beats Baseline: No"** and advised against using the model for decisions, so I do not use it. Yes-class precision, recall and AUC were not reported by the platform, so I make no claim about them. A second run requested as a gradient-boosting classifier with class weights returned byte-for-byte identical metrics (same losses, accuracy, F1 and 17 epochs; only the run ID differed), so I treat it as the same underlying model, not independent evidence, and I make no claim that boosting or class weights were actually applied. My recommendations rest on the segment analysis.

## The verifier caught mistakes in Avaloka's own answers

Avaloka's Claim Verifier checks each written answer against the computed tables.
- pdays answer: **2 of 4 claims flagged.** A "54.6% gap" was not in the computed results, and "significant" was used with no test. Re-asking for only the rates and intervals passed cleanly.
- Job answer: **1 of 6 flagged.** It called 875 rows the smallest job; the smallest has 330.
- Month and prior-outcome answers: 2 of 5 flagged each, mostly "highest/smallest" claims that contradicted the table ordering or cited a derived percentage.

I use only figures from the computed tables, not the flagged prose.

## Recommendations

1. **Re-contact customers with a prior success first.** About 65% convert, but the group is only about 1,400 people, so treat it as a priority list, not a volume plan.
2. **Call on cellular, not landline.**
3. **Question the heaviest calling in the weakest period.** May took a third of all calls at the lowest yield.
4. **Pilot student and retiree outreach** before scaling, since both are small groups.

## Limitations

- These are associations, not causes. Channel, month and prior contact may be confounded by how the campaign was run.
- One Portuguese bank, 2008-2010 (the financial crisis).
- The 80/20 split is hash-based, not chronological. `euribor3m`, `nr.employed` and `emp.var.rate` act as time proxies, so performance may not carry to later periods.
- Auto Insights charts were drawn from an 11,000-row sample, with figures computed on all 41,188 rows (as labelled by Avaloka).
- Some answers were served by a fallback model when the primary was rate-limited, and the hosted backend was intermittently unavailable. I relied on the computed tables, not the prose.
- Small segments (student, retired, Dec, Sep, Oct) have wide uncertainty.

## Reproducibility: prompts used (in order)

1. Group by job. For each job give the number of rows and the share of rows where y is yes.
2. Calculate the subscription rate by contact channel.
3. Compare pdays = 999 and pdays < 999. State only the two subscription rates and their confidence intervals. Do not add derived numbers or the word significant.
4. Group by month. For each month give the number of rows and the share of rows where y is yes.
5. Group by poutcome. For each poutcome value give the number of rows and the share of rows where y is yes.
6. Count rows for each combination of poutcome and whether pdays equals 999.
7. Train a scikit-learn LogisticRegression classifier for y. Do not use duration. / List the 19 selected features and confirm duration is not one of them. / Start training with this plan.
8. Train a gradient boosting classifier for y. Do not use duration. Use class weights so the yes class is not ignored. / Start training with this plan.

## Evidence

Screenshots: [add folder or images: Auto Insights overview, Analysis results tables, verifier warnings, Training Run Report]

## Data source

Moro, S., Rita, P., Cortez, P. (2014). Bank Marketing. UCI Machine Learning Repository. https://doi.org/10.24432/C5K306
