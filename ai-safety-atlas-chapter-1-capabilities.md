---
title: "AI Safety Atlas — Chapter 1: Capabilities"
authors: []
year: 
tags: []
status: reading
draft: true
date_read: 2026-09-03
connections: []
---

# AI Safety Atlas — Chapter 1: Capabilities

> **One-line takeaway:** 

## Summary

<!-- 3-5 sentences: what problem, what approach, what result -->

## Key Contributions

<!-- Bulleted list of what's new or important -->

## Key Highlights

> "The Overhang Argument. There might be situations where there are substantial advancements or availability in one aspect of the AI system, such as hardware or data, but the corresponding software or algorithms to fully utilize these resources haven't been developed yet. The term 'overhang' is used because these situations imply a kind of 'stored’ or ‘latent’ potential." (p.0) [#fbbf24]

## Notes

General notes and thoughts:
- AI started narrow -> with LLMs AI becomes more "general". However, even this does not feel true since language is narrow. Current AI demos and use cases showcase how LLMs operate in the digital world by using language to write and run code. So this is not a general intelligence, but gets us quite far (imho).
- capability and generality for defining intelligence

Questions:
- Are LLMs a viable path toward AGI? How about JEPA models: https://www.youtube.com/watch?v=kYkIdXwW2AE
- why is it so hard to land on a shared definition of AGI?

Questions during the course:
- Talking about compute: I am hearing fundamentally different opinions ranging from bubble and overbuilding to compute (and memory shortage) for decades?
- What is your take on orbital compute?
- Today, I listened to a podcast and the claim was that if somebody reaches RSI and the recipe is "known" you are in a very bad place if you regulate RSI in your country (you could run it unregulated in space)?
- Open-weight model debate --> decelerating force for frontier models and hence a good idea, BUT, others are saying open-weight models are a huge problem to safety (model poisoning, etc.
- What overhang might we currently have (i.e., data, compute, algorithms)?

Interesting quotes:
- "AGI can also be defined through a combination of time and scale - can AI match ‘n’ experts working together for time ‘t’.


**AI Insight (p.1):**
**Foundation models** are large models trained on broad data that can be adapted to many tasks — the term emphasizes architecture and training approach (pre-training on massive datasets, then fine-tuning). GPT-4, Claude, Gemini are all foundation models.

**Frontier models** are the most capable models at a given point in time — the term emphasizes competitive position. It's a moving label: today's frontier model is tomorrow's baseline.

The overlap is large. Most frontier models are foundation models, but not all foundation models are frontier (e.g., an older GPT-3 is still a foundation model but no longer frontier). The chapter uses "foundation models" because it's describing the architectural shift from narrow to general-purpose systems — which is what matters for the capability story here.


**AI Insight (p.1):**
The chapter previews its own answer in the intro: it rejects binary AGI definitions in favor of two continuous axes — **capability** (how well) and **generality** (how many domains). The goal is statements like "performs at the 85th percentile across 30% of cognitive domains" — something measurable rather than a threshold you cross.

The text gestures at two historical frameworks before landing there:

- **Turing Test** — can the system fool a human in conversation? Largely obsolete as a definition; current LLMs pass it trivially, yet no one thinks that settles the question.
- **Psychometric frameworks** — AGI as matching human performance across the range of cognitive tests we use to measure human intelligence (IQ-style breadth). More tractable but still anthropocentric.

The practical reason there's no consensus: "AGI" conflates at least three distinct questions that different people care about:

1. **Task coverage** — what fraction of economically/cognitively meaningful tasks can it do?
2. **Reliability/robustness** — does it generalize to genuinely novel situations, or does it pattern-match within its training distribution?
3. **Autonomy** — can it pursue goals over long horizons without human scaffolding?

Current frontier models score high on (1), mixed on (2), and weak on (3). That's why you see disagreement: someone focused on benchmark breadth might say GPT-4 crossed the line; someone focused on robust out-of-distribution generalization or agentic autonomy would say we're far from it.

For your safety work, the continuous-axes framing is the more useful one — it lets you ask "at what capability level does risk X become relevant?" rather than waiting for a binary AGI declaration that may never arrive.

## Discussion Notes

<!-- Reading-chat exchanges left out of this quick capture (7 substantive of 7 available) — turn on "include chat" in the Reader, or run Full Capture for a privacy-reviewed note -->

## Connections

<!-- How this paper relates to others we've read -->
<!-- Use wikilinks: [[other-paper]] -->

## Open Questions

<!-- Things we want to investigate further -->
