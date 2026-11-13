# Scoping Document: B5 – Executive Dashboard Automation

> Scenario: *Executives want an AI-powered dashboard that automatically explains weekly KPI changes.*

## 1. Problem Clarification

**Problem statement.** Executives want to know, each week, why key metrics moved, without waiting for an analyst. The real need is a trustworthy account of which parts of the business drove a change, and whether the change is meaningful or just noise. The dashboard informs where leadership asks questions or takes action; it must not present guesses as causes.

- **Decision-maker:** executive team (and the analytics/BI lead who owns the numbers).
- **Decision informed:** where to investigate, escalate, or act each week.
- **Key reframing:** "explain" can mean three different things: *describe* what changed (decomposition), *flag* whether it is unusual (anomaly detection), or *prove why* (causal). The first two are feasible; the third is not from a dashboard alone.

## 2. Problem Type Classification

**Primary framing: mostly non-ML analytics, with a small time-series component and an LLM used for narration only.**

- **Variance decomposition** (deterministic): split a KPI change across regions, segments, channels, products.
- **Time-series anomaly / change-point detection:** is this week's movement outside normal seasonal variation?
- **Natural-language generation:** an LLM turns verified numbers into readable text.
- **Causal analysis is explicitly out of scope.** Observational dashboards show association; "X caused Y" needs experiments or a causal design.

**Why this framing:** supervised learning has no target here (there is no label for "the reason"), and a predictive model that ranks features by importance would be misread as an explanation.

## 3. Data Science Scope

- **Available data:** KPI time series (daily/weekly), dimensional breakdowns (region, segment, channel, product, cohort), metric definitions, release/campaign/holiday calendars, past commentary written by analysts.
- **Modality:** tabular and time series; text for the narrative layer.
- **Granularity:** weekly KPI level, broken down by dimension; underlying event/transaction data.
- **Labels:** none for "reason for change". Past analyst commentary can serve as style examples and as an informal check, not ground truth.
- **Key risks:**
  - *Inconsistent metric definitions* across teams (see scenario B2): automating on an unagreed definition just spreads confusion faster.
  - *Noise read as signal:* weekly numbers fluctuate; most changes are not meaningful.
  - *Simpson's paradox / mix shift:* a KPI can move because segment mix changed, not behaviour.
  - *Spurious explanations:* testing many dimensions guarantees some look significant by chance.
  - *Seasonality, holidays, outages, and data latency or backfills.*
  - *LLM hallucination:* fluent but unsupported causes.
  - *Over-trust:* a polished explanation looks authoritative.

## 4. Modeling Approach (High-Level)

1. **Deterministic decomposition by dimension.** Compute each segment's contribution to the total change (rate vs. mix effects). Appropriate because it is exact, auditable, and needs no training data.
2. **Seasonal baseline + anomaly / change-point detection.** Compare each KPI and segment against an expected range, so the dashboard says "this is within normal variation" when it is. Appropriate because it controls false alarms and handles seasonality.
3. **LLM narration with grounding.** Feed the LLM only the computed facts (via structured output), have it write the summary, attach the numbers, and flag uncertainty. Appropriate because it adds readability without letting the model invent causes. Calendar events (releases, campaigns) can be listed as *candidate context*, labelled as correlation.

**Not appropriate:**
- Asking an LLM to "explain" raw data or charts directly (hallucination risk, unverifiable).
- A predictive model with feature importances presented as the cause of the change.
- Claiming causality (e.g. "the campaign drove the drop") without an experiment or a causal design.
- Building the dashboard before metric definitions are agreed.

**Success measures:** analyst agreement that the decomposition is correct, false-alarm rate of flagged anomalies, executive follow-up questions per report, share of narrative statements traceable to a computed number (target 100%).

## 5. Use of LLMs (Reflection)

> *To be written from my real working session with an LLM. Do not fill with invented content.*

- **How I used an LLM in this task:** TODO
- **One example where it clarified my thinking:** TODO
- **One example where I challenged, corrected, or refined its output:** TODO

## 6. Unknown Methods Discovery

> *To be written from what I actually learn. Candidates: change-point detection, rate/mix (Kitagawa–Oaxaca-style) decomposition, Shapley-value attribution.*

- **Technique I did not previously know:** TODO
- **Why it is relevant here:** TODO
- **Explained in my own words:** TODO
- **An incorrect assumption I had, and how it was corrected:** TODO
