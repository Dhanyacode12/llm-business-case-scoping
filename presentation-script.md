# Presentation Script (about 8 minutes, 14 slides)

How to use this: read it aloud a few times until you know the ideas, then say it in your own words. Each part is marked with the slide it belongs to. *(pause)* means stop for a second and breathe. Slide text is short on purpose, so you tell the story and the slide only supports it.

---

## Slide 1: Cover (about 20 seconds)

Hello everyone. Today I will show you two scenarios from the exercise on applied LLMs for business cases. In both, someone in a company says, "Let's use AI," but nobody has said exactly what problem they want to solve. My job, as a Product Manager, was not to build a model. It was to frame the problem correctly before any modelling starts. One scenario is a case where machine learning fits well, and the other is a case where it mostly doesn't.

---

# Scenario A3: Support ticket overload

## Slide 2: The scenario (about 40 seconds)

Let me start with scenario A3. Customers write support tickets. The tickets are free text, so they can be about anything and can be urgent or not. Today, a person sorts each ticket by hand and sends it to a team, such as billing, account access, or shipping. This is slow, and it is not always consistent, which is why that box is marked in red.

The decision-maker is the Head of Support. The decision is which team should get each ticket, and how urgent it is. The goal is faster answers for customers. The AI should help the support staff, not replace them. *(pause)*

## Slide 3: Where is the time lost? (about 35 seconds)

Before any AI, I would ask one question: where is the time really lost? A ticket's journey has three stages: waiting to be sorted, waiting for the right team, and being solved. This picture is only an illustration, because we don't know the real lengths yet. We would have to measure them.

Why does this matter? A sorting model only helps if sorting is the slow part. If the real delay is in the third stage, a sorting model will not make answers faster. And this first step needs no machine learning at all.

## Slide 4: Can we trust the old sorting records? (about 45 seconds)

The company has a record of where past tickets were sent. It is tempting to use these records to train a model, as if they were correct answers. But people sorted them by hand, and different people may have sorted similar tickets differently. So I would test the records first.

The test works like this. We take one hundred past tickets. Two experienced support staff, who are humans, sort them separately, without seeing the old record. Then we compare, and we see how often they agree.

If they agree most of the time, the old records are usable as labels. If they often disagree, a model trained on them would only learn that confusion, so we should not train on them. *(pause)*

## Slide 5: The plan (about 50 seconds)

Here is the plan, in four steps.

Step one: turn the ticket text into numbers, so that tickets with similar meaning get similar numbers. Step two: group similar tickets together, so the categories come from the data. Step three: humans check the groups, and we add an "other" option for tickets that fit nowhere. Step four: train a router that suggests a team for new tickets.

Why in this order? Because we have no trusted labels, we find the categories before we predict them.

And here is what I would not do. I would not let an AI answer customers before the sorting is proven to work. And I would not trust the old records without testing them.

## Slide 6: How grouping finds categories (about 45 seconds)

This is the method I learned, called BERTopic with HDBSCAN. This picture is a sketch, not real data. Each dot is one ticket.

First, similar tickets land close together. Second, the crowded areas become groups, and a person gives each group a name, like account access, billing, or shipping. Third, the odd tickets, the grey dots, are left out. They are not forced into a group. That matters to me, because forcing every ticket into a category can hide the real problems. *(pause)*

## Slide 7: What I learned in A3 (about 60 seconds)

I used Claude as a thinking partner, first to understand the use case, and then to discuss a draft of my scoping document. Here is what changed in my thinking.

First, I assumed the old routing records were correct answers. Claude pointed out that people sorted them by hand, so I plan to test them: two support staff sort the same hundred tickets, and we compare.

Second, Claude suggested that an LLM could label tickets using a fixed list of categories. I worried that it would force tickets into the wrong categories. So I added the "other" option, and some tickets are labelled by a person who has not seen the LLM's suggestion, because people tend to accept a suggestion when they see one.

Third, I had a wrong assumption: I thought clustering and classification were the same. They are not. Clustering finds categories. Classification sorts tickets into categories that already exist. *(pause)*

---

# Scenario B5: Executive dashboard

## Slide 8: The scenario (about 45 seconds)

Now scenario B5. Executives want an AI dashboard that explains why their key numbers moved each week. The word "explain" can mean three different things.

It can mean describe: what changed, and where. That is possible. It can mean flag: is the change unusual, or just normal variation? That is also possible. Or it can mean prove why. That is not possible from a dashboard alone, because proving a cause needs an experiment.

The decision-maker is the executive team, and the decision is where to look and act each week.

## Slide 9: The plan (about 50 seconds)

Here is the plan, again in four steps. Step one: collect the KPI numbers, with one agreed definition for each metric. Step two: split the change by region, segment, and channel. Step three: check whether the change is outside the normal range. Step four: the LLM writes the sentence, only from the verified numbers.

Why this way? Because a cause can't be proven from a dashboard, we calculate the facts, and the LLM only puts them into words. That is why I call this mostly not a machine learning problem.

What I would not do: ask an LLM to explain raw data, or claim a cause without an experiment.

## Slide 10: Is this change unusual? (about 35 seconds)

This is step three. The chart is a sketch, not real data. The line is a weekly number. The blue band is the normal range. For eleven weeks the line stays inside it. In the last week it drops below. That is a change worth looking at.

If the line had stayed inside the band, the dashboard should say that it is normal variation. Most week-to-week changes are noise, and a good dashboard should say so. *(pause)*

## Slide 11: Where did the change come from? (about 55 seconds)

This is step two, and it is the second method I learned: rate and mix. The numbers here are a made-up toy example. Sales fell from one hundred to ninety-two. The chart splits that drop into two reasons.

The first is rate, or behaviour: people buy less in each group. Here that explains five. The second is mix: people behave the same, but fewer customers are in the group that usually buys more. Here that explains three.

The two reasons call for different actions. If it is behaviour, you look at the product or the price. If it is only the mix, nobody changed their mind. So the dashboard should show both.

## Slide 12: How the dashboard should word it (about 40 seconds)

Now step four, the wording. One version says, "Sales fell because the campaign ended." The word "because" claims a cause, and we have not tested that. So I would avoid it.

The version I would use says, "Sales fell 8%. The campaign ended the same week. We haven't tested whether it caused the drop." It states the facts, it mentions what happened at the same time, and it says what has not been tested. The LLM writes only what the numbers support. *(pause)*

## Slide 13: What I learned in B5 (about 50 seconds)

I want to be honest here: I did not use an LLM for the first draft of this document. I used Claude afterwards, to discuss it and to test my assumptions.

I assumed a dashboard should tell executives the cause. Now I know it can show what changed, but it cannot prove why. That is why we avoid the word "because."

I also had a wrong assumption: when a number drops, customers changed their behaviour. Now I know the customer mix can change instead, and that is the rate and mix idea.

## Slide 14: Closing (about 25 seconds)

To finish, the lesson was the same in both scenarios. The main work was framing. In A3, machine learning fits, but we start by discovering the categories, not by predicting them. In B5, it is mostly not machine learning, and the LLM only writes up numbers that we calculated.

In both cases, the most useful step was to decide what is really being asked, before building anything. Thank you.

---

## If you get a question

**"Why not just train a classifier on the routing history?"** Because the routing was done by hand and was not always consistent, so a model would learn the same inconsistency. I would first test it: two support staff sort the same hundred tickets, and we compare. If they agree most of the time, a classifier is a good next step.

**"Why isn't B5 a machine learning problem?"** Because there is no label for the reason of a change, and a prediction model does not explain a cause. Splitting a change into parts and checking if it is unusual is more reliable, and the results can be checked. The LLM is useful for writing, not for finding causes.

**"Are the charts real data?"** No. I have no company data. They are sketches that show the idea, and each one is marked as an illustration or a toy example.

**If you don't know an answer:** say, "I haven't looked at that yet, but my first step would be..." and name a simple check. That is an honest and good answer.
