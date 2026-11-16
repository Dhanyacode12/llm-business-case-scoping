# Presentation Script (about 8 minutes)

How to use this: read it aloud a few times until you know the ideas, then say it in your own words. Short pauses are fine. Each part is marked with the slide it belongs to. Where it says *(pause)*, stop for a second and breathe.

---

## Slide 1: Cover (about 20 seconds)

Hello everyone. Today I will show you two scenarios from the exercise on applied LLMs for business cases. In both, someone in a company says, "Let's use AI," but nobody has said exactly what problem they want to solve. My job, as a Product Manager, was not to build a model. It was to frame the problem correctly before any modelling starts. I will show you one scenario where machine learning fits well, and one where it mostly doesn't.

---

## Slide 2: A3, the problem (about 1 minute)

Let me start with scenario A3, support ticket overload.

The situation is this. Customer support volume has gone up a lot. The tickets are free text, and people route them by hand. Leadership wants AI to reduce the response time.

My first point is that the system should help the support staff, not replace them. The goal is to route and prioritise tickets faster, so that customers get an answer sooner.

My second point is that the first step is not machine learning at all. Before building anything, I would check where the time is really lost. Is it waiting for someone to sort the ticket? Is it waiting for the right team? Or is it the resolution itself being slow? If routing is not the bottleneck, a routing model will not make responses faster. *(pause)*

My third point is the problem type. We do not have clean labels, and we do not have an agreed list of categories. So I would start with unsupervised learning, to discover the categories from the data. Only after that would I move to classification, when we have categories and labels.

---

## Slide 3: A3, data and risks (about 1 minute)

Now, the data. The company already has ticket text, timestamps, customer details, the routing history, the agents' replies, and the response and resolution times. So there is a lot of data.

The problem is the labels. The routing history tells us where tickets went, but people routed them by hand, and not always in the same way. So I do not trust it as a source of correct labels.

Let me mention three risks.

The first is label noise. If routing was inconsistent, a model trained on it will copy that inconsistency.

The second is leakage. The agent's reply and any tags added after triage must not be used as input, because they are not available when a new ticket arrives.

The third is rare but critical tickets, such as outages, legal issues, or security problems. There are few of them, but missing one is very costly.

There are other risks too, such as changes over time, personal data in tickets, and messy text. I can talk about those in the questions.

---

## Slide 4: A3, approach (about 1 minute)

For the approach, I propose three steps.

Step one: group similar tickets together, and let the support leads give each group a name. This way the categories come from the real data, not from our guesses.

Step two: use an LLM to suggest labels on a sample of tickets, and have humans review and correct them. I add two safeguards. There is an "other" option, for tickets that do not fit any category. And some tickets are labelled by humans without seeing the LLM's suggestion, so we can compare.

Step three: show agents similar past tickets, so they can answer faster.

And there are two things I would not do. I would not let an LLM answer customers automatically before the triage is proven to work. And I would not trust the routing history as labels until I have tested it. *(pause)*

---

## Slide 5: A3, what I learned (about 1 minute 15 seconds)

Now, what I learned in this scenario.

I used Claude as a thinking partner. First to understand the use case, and then to discuss a draft of my scoping document.

Here is the first thing. I assumed that the routing history, the old record of where each ticket was sent, would make a good set of correct answers for a model, because it is large and the support staff are experienced. Claude pointed out that people sorted those tickets by hand, so the records may not be consistent. So I changed my plan: before using them, I would test them. Two experienced support staff, who are humans, each sort the same hundred tickets alone, and we compare how often they agree. If they agree most of the time, the history is usable. If not, a model would just learn their disagreement. So an opinion became a test.

Here is the second thing. Claude suggested that an LLM could label a sample of tickets using a fixed list of categories. I found that shaky, because the LLM might force tickets into categories that do not describe the real problem. So I changed the plan again. We added an "other" option for tickets that fit nowhere. And we added blind labelling: some tickets are labelled by a person who has not seen the LLM's suggestion, because people tend to just accept a suggestion when they see one.

The new method I learned is called BERTopic with HDBSCAN. In simple words: first, embeddings turn each ticket into numbers, so that tickets with similar meaning are close together. Then HDBSCAN groups the close tickets, and it leaves out the ones that do not fit anywhere. Then BERTopic names each group with its most common words, and a person decides the final name.

My wrong assumption was that clustering and classification are the same thing. They are not. Classification sorts tickets into categories that already exist. Clustering finds the categories first. *(pause)*

---

## Slide 6: B5, the problem (about 1 minute)

Now to scenario B5, the executive dashboard.

Executives want an AI-powered dashboard that automatically explains the weekly changes in their key numbers.

The important point is that "explain" can mean three different things. It can mean describe: what changed, and where. It can mean flag: is this change unusual, or just normal variation? Or it can mean prove why. The first two are possible. The third is not possible from a dashboard alone, because proving a cause needs an experiment or a careful causal design.

So I called this mostly not a machine learning problem. The work is to split a change into its parts and to check whether it is unusual. The LLM has only one job: to write up numbers that were already calculated.

---

## Slide 7: B5, data and risks (about 1 minute)

The data is KPI numbers over time, broken down by region, segment, channel, and product. We also have the metric definitions, and calendars of releases, campaigns, and holidays. But there is no label for "the reason" a number changed.

The danger here is a confident, wrong explanation. Let me give four risks.

First, unclear definitions. If two teams define "active users" differently, automating the report only spreads the confusion faster.

Second, noise read as signal. Weekly numbers go up and down all the time, and most changes mean nothing.

Third, mix shift. A number can move because the mix of customers changed, not because anyone behaved differently.

Fourth, the LLM can write fluent reasons that the data does not support. A polished explanation looks more certain than it is.

---

## Slide 8: B5, approach (about 45 seconds)

My approach has three steps. First, split each change by region and segment. Second, compare it with normal seasonal variation. Third, the LLM turns the verified numbers into plain text.

What I would not do: ask an LLM to explain raw data, or claim a cause without an experiment. *(pause)*

---

## Slide 9: B5, what I learned (about 1 minute 15 seconds)

Now, what I learned here. I want to be honest: I did not use an LLM for the first draft of this document. I used Claude afterwards to discuss it.

I started with an assumption: a dashboard that explains a change should tell executives the cause. Through the discussion I understood that a dashboard can show what changed, and mention what happened at the same time, but it cannot prove why. So I changed how the dashboard should speak.

That led us to look at the wording. One version said, "Sales fell because the campaign ended." The word "because" claims a cause. So I chose a version without it. I shortened it, and Claude pointed out that my short version lost the timing. The final sentence is on the slide: "Sales fell 8%. The campaign ended the same week. We haven't tested whether it caused the drop."

The new method I learned is rate and mix decomposition. A number can fall for two reasons. The rate: people buy less in each group. Or the mix: people behave the same, but fewer customers are in the group that usually buys more. The technique works out how much of the change comes from each.

My wrong assumption was that when a number drops, customers must have changed their behaviour. That is only one possible reason. The other is just a change in the mix. *(pause)*

---

## Slide 10: Closing (about 30 seconds)

To finish, the lesson was the same in both scenarios. The main work was framing. In A3, machine learning fits, but we start by discovering the categories, not by predicting them. In B5, it is mostly not machine learning, and the LLM only writes up numbers that we calculated.

In both cases, the most useful step was to decide what is really being asked, before building anything. Thank you. I am happy to answer questions.

---

## If you get a question

**"Why not just train a classifier on the routing history?"** Because the routing was done by hand and was not always consistent, so a model would learn the same inconsistency. I would first test it: two agents route the same hundred tickets, and we compare. If they agree most of the time, a classifier is a good next step.

**"Why isn't B5 a machine learning problem?"** Because there is no label for the reason of a change, and a prediction model does not explain a cause. Splitting a change into parts and checking if it is unusual is more reliable, and the results can be checked. The LLM is useful for writing, not for finding causes.

**If you don't know an answer:** say, "I haven't looked at that yet, but my first step would be..." and name a simple check. That is an honest and good answer.
