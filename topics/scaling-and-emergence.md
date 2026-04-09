---
title: "Scaling and Emergence"
tags: [scaling, inverse-scaling, emergence, incoherence, test-time-compute]
papers:
  - "[[hot-mess-of-ai]]"
  - "[[inverse-scaling-test-time-compute]]"
  - "[[thought-anchors]]"
  - "[[thought-branches]]"
  - "[[emotion-concepts-llm]]"
last_updated: 2026-04-07
---

# Scaling and Emergence

> **Thread summary:** Scaling (model size, training compute, test-time compute) doesn't uniformly improve AI — it can introduce new failure modes, shift errors from systematic to incoherent, and actively degrade performance on certain tasks.

## Overview

The conventional scaling narrative ("bigger models = better performance") has significant caveats. Two lines of evidence challenge it:

1. **Inverse scaling with model size:** As models get larger, their remaining errors become more random and less predictable (Hot Mess). The incoherence ratio increases with scale.

2. **Inverse scaling with test-time compute:** More reasoning steps can *reduce* accuracy (Gema et al.). Models get distracted, latch onto spurious patterns, or lose track of constraints during extended reasoning.

Both findings are safety-relevant: we need AI systems to be *more* reliable as they handle harder tasks, but these results suggest the opposite can happen.

## Key Papers

1. **[[hot-mess-of-ai]]** — Bias-variance decomposition of model errors. Incoherence ratio increases with scale. Errors become "hot mess" rather than systematically wrong.
2. **[[inverse-scaling-test-time-compute]]** — Extended reasoning degrades performance. Failure modes: distraction, spurious correlation, constraint loss, framing sensitivity.
3. **[[thought-anchors]]** — Causal attribution within reasoning traces. Tool for identifying *which* tokens drive answers.
4. **[[thought-branches]]** — Branch point detection in reasoning. Tool for finding *where* reasoning could have diverged.
5. **[[emotion-concepts-llm]]** — Emotion-like representations in LLMs. Exploratory lens for whether emotional dynamics correlate with reasoning failures.

## Emerging Themes

- **"More is not always better"** applies to both model scale and reasoning scale
- The shift from bias to variance (systematic → incoherent errors) is a fundamental property of scaling, not a quirk
- Inter-trace analysis (Hot Mess) and intra-trace analysis (our planned research) are complementary views of the same phenomenon
- Existing mechanistic interpretability tools (Thought Anchors/Branches) can be repurposed for safety-relevant analysis

## Our Take

The *why* behind inverse scaling is the open question. We're pursuing intra-trace causal analysis as the entry point — identifying where in reasoning traces the model commits to answers, where it goes wrong, and what's happening mechanistically at those turning points. See [[inverse-scaling-incoherence]] for the full research plan.

## Open Questions

- Is there a unified mechanism behind inverse scaling at model scale and reasoning scale, or are they distinct phenomena?
- Can incoherence be predicted from task features? (Would enable targeted mitigation)
- Does RLHF/fine-tuning help or hurt? Does alignment training make failure modes more or less coherent?
- What's the relationship between inverse scaling and sycophancy? Both increase with scale.
- Can ensemble methods exploit incoherent errors? (If errors are random, majority voting should help)
