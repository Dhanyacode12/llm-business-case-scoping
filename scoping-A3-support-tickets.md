# Scoping Document: A3 – Support Ticket Overload

> Scenario: *Customer support volume has increased significantly. Tickets are free-text, routed manually, and vary widely in topic and urgency. Leadership wants AI to help reduce response time.*

## 1. Problem Clarification

**Problem statement.** Support tickets arrive as unstructured text and are routed by hand, so urgent or misrouted tickets wait too long before the right person sees them. We want to help support staff route and prioritise incoming tickets faster and more consistently, so time-to-first-response and time-to-resolution drop. The system supports human routing decisions; it does not replace agents or answer customers on its own.

- **Decision-maker:** Head of Support / support operations lead.
- **End users:** support agents and team leads who triage and handle tickets.
- **Decision informed:** which queue or team a ticket goes to, and how urgent it is.
- **First thing to check (non-ML):** where does the time actually go? Waiting for triage, waiting for the right team, or slow resolution? If routing is not the bottleneck, a routing model will not move response time.

## 2. Problem Type Classification

**Primary framing: unsupervised learning (topic discovery), followed by weakly/semi-supervised classification.**

- There is no agreed ticket taxonomy and no clean labels, so we first have to *discover* what categories exist (clustering / topic modelling on text embeddings).
- Once a taxonomy is agreed with support leads, we bootstrap labels (LLM-assisted plus human review) and train a lightweight classifier. That is semi-supervised, not plain supervised.
- Urgency is a second, separate target. It is probably ordinal and ambiguous, so it needs its own definition before it is modelled.
- Representation learning (embeddings) underlies both steps.

**Why not supervised from day one:** the only "labels" are manual routing decisions, which are inconsistent and encode today's process, including its mistakes.

## 3. Data Science Scope

- **Available data:** ticket text (subject, body), timestamps, channel, customer/account metadata, routing history (which queue it landed in, reassignments), agent replies, response and resolution times.
- **Modality:** text (primary), tabular metadata, time series of volume.
- **Granularity:** ticket-level; threads/conversations may need grouping.
- **Labels:** none clean. Possible weak signals: final queue after reassignments (better than first queue), tags agents already add, resolution time as an urgency proxy. Realistic to create: a few hundred to a thousand human-labelled tickets with LLM pre-labelling.
- **Key risks:**
  - *Label noise and bias:* the manual routing is inconsistent; training on it copies it.
  - *Leakage:* agent replies, resolution info, or post-triage tags must not be input features.
  - *Imbalance and sparsity:* rare but critical categories (outages, legal, security).
  - *Drift:* new products and incidents change the topic mix; volume spikes are seasonal.
  - *Privacy:* tickets contain PII; processing may need to stay local or be scrubbed.
  - *Multilingual / noisy text:* typos, mixed languages, pasted logs.

## 4. Modeling Approach (High-Level)

1. **Embeddings + clustering (BERTopic / HDBSCAN-style).** Turn tickets into vectors, cluster them, and have support leads name and merge clusters. Appropriate because it discovers structure without labels and gives a taxonomy grounded in real data.
2. **LLM-assisted labelling, then a lightweight classifier.** Use an LLM with the agreed taxonomy to pre-label a sample, have humans correct it, then train a small classifier on embeddings for fast, cheap, auditable routing. Appropriate because it scales labelling while keeping humans in control, and it runs locally if privacy requires.
3. **Similar-ticket retrieval.** Surface past resolved tickets and their routing/answers for the agent. Appropriate because it needs no labels and helps response speed directly.

**Not appropriate:**
- A supervised model trained directly on raw manual routing (learns the existing inconsistency).
- LLM auto-replies to customers before triage quality is proven (risk of wrong answers, tone and compliance problems).
- Predicting urgency from resolution time alone (confuses "hard" with "urgent").
- Doing nothing about process: if the bottleneck is staffing or unclear queue ownership, ML will not fix it.

**Success measures:** routing agreement with a human-reviewed gold set, reassignment rate, time-to-first-response, escalation misses on critical tickets (recall matters most there).

## 5. Use of LLMs (Reflection)

> *To be written from my real working session with an LLM. Do not fill with invented content.*

- **How I used an LLM in this task:** TODO
- **One example where it clarified my thinking:** TODO
- **One example where I challenged, corrected, or refined its output:** TODO

## 6. Unknown Methods Discovery

> *To be written from what I actually learn. Candidates: BERTopic / HDBSCAN, weak supervision with labelling functions.*

- **Technique I did not previously know:** TODO
- **Why it is relevant here:** TODO
- **Explained in my own words:** TODO
- **An incorrect assumption I had, and how it was corrected:** TODO
