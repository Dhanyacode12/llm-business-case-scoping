# Presentation Talking Points (6–8 minutes total)

**Opening (15 seconds):** "I scoped two vague 'use AI' requests as a Product Manager: one where ML fits, and one where it mostly doesn't."

---

## Scenario A3: Support Ticket Overload (3–4 minutes)

**The problem (30 sec)**
- Support volume is up. Tickets are free text and routed by hand.
- Goal: help staff route and prioritise tickets faster, so response time drops.
- The system supports people. It does not replace agents.

**First step, no ML (20 sec)**
- Check where the time is really lost: waiting for triage, the right team, or resolution.
- If routing isn't the bottleneck, a routing model won't help.

**Problem type (30 sec)**
- There are no clean labels and no agreed categories.
- So: unsupervised first, to discover the categories. Then classification, once labels exist.

**Data and risks (30 sec)**
- Ticket text, timestamps, routing history, reply times.
- Risks: inconsistent routing, label noise, personal data, rare but critical tickets.

**Approach (45 sec)**
- Group similar tickets, then have support leads name the groups.
- Use an LLM to suggest labels on a sample. Humans review and correct.
- Show agents similar past tickets.

**What I would not do (20 sec)**
- No LLM auto-replies to customers before triage is proven.
- Don't trust the routing history as labels until I've tested it.

**LLM reflection (40 sec)**
- I used Claude to understand the use case and to discuss a draft.
- It clarified my thinking: I assumed the routing history was a good training set. The test is to have two agents route the same 100 tickets and compare. That turned an opinion into a check.
- I challenged it: an LLM may force tickets into wrong categories. I kept human review, then we found reviewers tend to accept suggestions. So I added an "other" option and blind human labelling.

**Unknown method (30 sec)**
- BERTopic with HDBSCAN: embeddings turn text into numbers, HDBSCAN groups similar tickets and leaves odd ones out, BERTopic names the groups.
- My wrong assumption: I thought clustering and classification were the same. Clustering finds categories. Classification sorts into categories that already exist.

---

## Scenario B5: Executive Dashboard Automation (3–4 minutes)

**The problem (30 sec)**
- Executives want AI to explain weekly KPI changes.
- "Explain" can mean describe, flag as unusual, or prove why. The first two are feasible. The third is not from a dashboard alone.

**Problem type (30 sec)**
- Mostly not ML: splitting a change into its parts, plus checks for unusual movement.
- The LLM only writes up numbers that were calculated.

**Data and risks (30 sec)**
- KPI time series, breakdowns by region and segment, metric definitions.
- Risks: inconsistent metric definitions, noise read as signal, invented explanations, over-trust.

**Approach (45 sec)**
- Split each change by region and segment.
- Compare against normal seasonal variation.
- LLM turns verified numbers into plain text.

**What I would not do (20 sec)**
- Don't ask an LLM to explain raw data.
- Don't claim a cause without an experiment.

**LLM reflection (40 sec)**
- I did not use an LLM for the first draft. I used it afterwards to discuss it.
- It clarified my thinking: I thought a dashboard should tell executives the cause. It can show what changed, not prove why.
- I refined its output. Compare "Sales fell because the campaign ended" with a version that avoids "because". The final wording: "Sales fell 8%. The campaign ended the same week. We haven't tested whether it caused the drop."

**Unknown method (30 sec)**
- Rate/mix decomposition: a number can fall because people behave differently in each group (rate), or because the mix of customers changed (mix).
- My wrong assumption: I thought a drop means customers changed their behaviour. It can be only a change in mix.

---

**Closing (15 seconds):** "In both cases the main work was framing: deciding what is really being asked, and when ML is not the answer."
