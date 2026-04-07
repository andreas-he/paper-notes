---
title: "Inverse Scaling in Test-Time Compute"
authors: [Gema, Hägele, Chen, Arditi, Goldman-Wetzler, Fraser-Taliente, Sleight, Petrini, Michael, Alex, Minervini, Chen, Benton, Perez]
year: 2025
arxiv: "2507.14417"
url: "https://arxiv.org/abs/2507.14417"
tags: [inverse-scaling, test-time-compute, reasoning, evaluation]
status: reading
date_read: 2026-04-07
date_discussed: 2026-04-07
connections:
  - "[[the-hot-mess-of-ai]]"
  - "[[thought-anchors]]"
  - "[[thought-branches]]"
code: ""
---

# Inverse Scaling in Test-Time Compute

> **One-line takeaway:** More test-time reasoning doesn't always help — it can actively degrade model performance through distraction, spurious correlation, and constraint loss.

## Summary

Investigates when giving reasoning models more test-time compute actually degrades performance. Identifies failure patterns: increased distraction by irrelevant info, overfitting to problem formats, shifts toward spurious correlations, and reinforcement of flawed reasoning. Extended reasoning can amplify flawed reasoning patterns rather than correcting them.

## Key Contributions

- Empirical demonstration that test-time compute scaling can be *inverse* — more thinking = worse answers
- Taxonomy of failure modes in extended reasoning (distraction, spurious correlation, constraint loss, framing sensitivity)
- Identification of specific task types where inverse scaling is most pronounced
- Challenges the narrative that scaling test-time compute is a reliable path to better performance

## Method

<!-- To be filled after deep reading -->

## Results

<!-- To be filled after deep reading -->

## Discussion Notes

- Key insight: extended reasoning can *amplify* flawed reasoning patterns, not just fail to correct them
- Complements Hot Mess: that paper shows errors become incoherent with model scale; this shows errors increase with reasoning scale
- Together they paint a picture where "more" (bigger model, longer reasoning) isn't reliably "better"
- The failure modes described (distraction, constraint loss) are candidate mechanisms for *why* inverse scaling happens — our research project aims to test these mechanistically

## Connections

- [[the-hot-mess-of-ai]] — complementary finding: Hot Mess = errors become incoherent with model scale; this = errors increase with reasoning scale
- [[thought-anchors]] — tool for identifying which tokens in a reasoning trace drive the answer (could reveal *where* distraction/constraint loss happens)
- [[thought-branches]] — tool for finding where reasoning diverges (could reveal *when* the model goes off-track)

## Open Questions

- Which failure modes are most common? Is there a dominant mechanism or is it task-dependent?
- Do failure modes interact? (e.g., does distraction lead to constraint loss?)
- Can the effect be predicted from task features before running the model?
- What is the relationship between inverse scaling in test-time compute and the incoherence ratio from Hot Mess?

## Code & Experiments

<!-- Check for open-source implementation on GitHub -->
