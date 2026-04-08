---
title: "Hot Mess of AI"
authors: []
year: 
tags: []
status: reading
date_read: 2026-04-08
connections: []
---

# Hot Mess of AI

> **One-line takeaway:** 

## Summary

<!-- 3-5 sentences: what problem, what approach, what result -->

## Key Contributions

<!-- Bulleted list of what's new or important -->

## Key Highlights

> "and how reasoning length is a stronger indicator of incoherence than model siz" (p.7) [#fbbf24]

> "nsembling is a particular form of model error correction, which is impractical for
action loops in the world, since state can typically not be reset." (p.9) [#fbbf24]

> "nsembling is a particular form of model error correction, which is impractical for
action loops in the world, since state can typically not be reset." (p.9) [#fbbf24]

> "nsembling is a particular form of model error correction, which is impractical for
action loops in the world, since state can typically not be reset." (p.9) [#fbbf24]

> "igure 7: Ensembling and larger reasoning budgets reduce incoherence. Other forms of
error correction may also reduce incoherence. (a) Instructing models to reason longer improves
performance (inference scaling laws, Fig. 17) and sometimes incoherence. This effect is smaller
than natural variation, where incoherence rises sharply (Fig. 3; direct comparison in Fig. 17). (b)
With O4-MINI on GPQA, we analyze the effect of the ensembling, i.e., using multiple samples
to average output probabilities over targets for the same question. The bias and variance are now
computed by comparing different ensembles of the same size. We find that, as expected from theory,
it reduces variance with a rate of 1/E, without affecting bias (left). As a consequence, incoherence
drops (right). Ensembling is a particular form of model error correction, which is impractical for
action loops in the world, since state can typically not be reset. However, we expect other error" (p.9) [#fbbf24]

> "Why expect more capable models to be more incoherent? In this paper, we do not experimen-
tally or theoretically explore the specific mechanisms for increasing incoherence with increasing
trajectory length and (sometimes) model size." (p.10) [#fbbf24]

> "ut considerable effort, nor should we expect this to be easier
than training other properties into their dynamics." (p.10) [#fbbf24]

> "Why expect more capable models to be more incoherent? In this paper, we do not" (p.10) [#fbbf24]

> "mechanisms" (p.10) [#fbbf24]

> "However" (p.10) [#fbbf24]

> "ems. When they generate text or take actions, they trace
trajectories in a high-dimensional state space. It is often very hard to constrain a generic dynamical
system to act as an optimizer. The set of dynamical systems that act as optimizers of a fixed loss
is measure zero in the space of all dynamical systems. As models scale and acquire broader capa-
bilities, their effective state and action space expands, exacerbating this difficulty. We should not
expect AIs to act as optimizers without considera" (p.10) [#fbbf24]

> "s models scale and acquire broader capa-" (p.10) [#fbbf24]

> "ilities, their effective state and action space expands, exacerbating this difficulty." (p.10) [#fbbf24]

> "This should inform judgements of the relative plausibility of different AI
risk scenarios and guide further research into understanding the mechanistic origins of incoherence." (p.10) [#fbbf24]

> "judgements" (p.10) [#fbbf24]

> "understanding" (p.10) [#fbbf24]

## Notes

## Notes:
- It seems that intelligence causally correlates with incoherence, not just in AI models, but in general.
- "Ensembling is a particular form of model error correction, which is impractical for
action loops in the world, since state can typically not be reset." -> how can ensembling be adapted to agentic workflows?

## Questions:
- If overall error goes down, but share of variance (incoherence) goes up, isn't that better still?
- Reasoning length is a stronger indicator than model size.
- How do you further decompose Bias into Bias(mesa) and Bias(spec)? How can this be done mathematically, or can it be done at all?

# Research ideas:
- measuring effectiveness of ensembles (i.e., size of ensemble, voting mechanism, etc.)
-- what effect does that have on safety and alignment?
- Will compute allow to ensemble more and improve alignment? 


**AI Insight (p.7):**
This is the core safety-relevant question of the paper, framed through a bias-variance lens.

When a capable model fails at a task, there are two possible failure modes:

1. **Wrong goal, effective optimizer** (high bias, low variance) — the model consistently pursues the wrong objective. It's coherent but misaligned. This is the classic mesa-optimization worry from Hubinger et al. 2019.

2. **Right goal, ineffective optimizer** (low bias, high variance) — the model has learned the correct objective on average, but can't reliably execute it. It's aligned but incoherent.

The paper's synthetic experiment (training transformers to literally mimic an optimizer descending a quadratic) lets them cleanly measure this. The finding: **larger models reduce bias much faster than variance** (Fig. 6 right, bias α=2.165 vs variance α=0.878). They learn *what* to optimize before they learn to do it *consistently*.

The safety implication: current capability gains are buying you models that know the right answer in expectation but give you wildly different outputs each time. That's arguably less dangerous than the alternative (a model that coherently pursues the wrong goal), but it means you can't trust any single output from a capable model on hard tasks — even when the model "knows" the right approach.

## Discussion Notes

<!-- Captured from 10 reading session(s) -->

### Session: 2026-04-07

**Q (p.5):** What does this mean?

"SWE-BENCH"

**A:** SWE-Bench is a benchmark for evaluating AI coding agents on real-world software engineering tasks. Each task is a actual GitHub issue from popular open-source Python repos (like Django, scikit-learn, sympy). The agent has to read the issue, navigate the codebase, and produce a patch — then success is measured by whether the repo's existing unit tests pass.

In this paper, they use it as their "agentic coding" task because it involves **multi-step tool use** (browsing files, editing code, running tests) rather than just answering a question. That makes it a natural fit for studying incoherence over longer action sequences — the x-axis in Figure 2b is "number of actions" rather than reasoning tokens.

The key finding for SWE-Bench specifically: as agents take more actions to solve an issue, their errors become increasingly dominated by variance (incoherence) rather than bias. Sonnet 4 and o3-mini show clear power-law scaling (α ≈ 0.40–0.49, high R²), while o4-mini's fit is poor (R² = 0.27), suggesting its incoherence pattern is less predictable on this task.

### Session: 2026-04-07

**Q:** What does this mean?

"SWE-BENCH"

**A:** SWE-Bench is a benchmark for evaluating AI coding agents on real-world software engineering tasks. Each task is a actual GitHub issue from popular open-source Python repos (like Django, scikit-learn, sympy). The agent has to read the issue, navigate the codebase, and produce a patch — then success is measured by whether the repo's existing unit tests pass.

In this paper, they use it as their "agentic coding" task because it involves **multi-step tool use** (browsing files, editing code, running tests) rather than just answering a question. That makes it a natural fit for studying incoherence over longer action sequences — the x-axis in Figure 2b is "number of actions" rather than reasoning tokens.

The key finding for SWE-Bench specifically: as agents take more actions to solve an issue, their errors become increasingly dominated by variance (incoherence) rather than bias. Sonnet 4 and o3-mini show clear power-law scaling (α ≈ 0.40–0.49, high R²), while o4-mini's fit is poor (R² = 0.27), suggesting its incoherence pattern is less predictable on this task.

**Q (p.6):** What does this mean?

"notably, for QWEN3, incoherence levels and slopes are nearly identical across"

**A:** The full sentence is: "for QWEN3, incoherence levels and slopes are nearly identical across **all sizes**, even though larger models perform better."

They tested QWEN3 at 1.7B, 4B, 8B, 14B, and 32B parameters. The finding: **scaling up the model improves accuracy but does nothing to reduce incoherence.** You can see it in Figure 2(a) right panel — the α (slope) values are all clustered tightly around 0.34–0.43, and the curves basically overlap.

Why this matters: you might expect that a 32B model, being smarter, would also be more *consistent* — getting the same question right reliably rather than flip-flopping. But no. It gets more questions right on average (lower bias), while being just as noisy run-to-run (same variance). The incoherence power law w.r.t. reasoning length is essentially a property of the architecture/training recipe, not the scale.

This contrasts with the frontier models in the left panel, where Sonnet 4, o3-mini, and o4-mini each have noticeably different slopes (0.56, 0.40, 0.29) — suggesting that different training approaches (RLHF, distillation, etc.) *do* affect how quickly incoherence grows with reasoning length, even if raw scale within a family doesn't.

### Session: 2026-04-07

**Q:** What does this mean?

"SWE-BENCH"

**A:** SWE-Bench is a benchmark for evaluating AI coding agents on real-world software engineering tasks. Each task is a actual GitHub issue from popular open-source Python repos (like Django, scikit-learn, sympy). The agent has to read the issue, navigate the codebase, and produce a patch — then success is measured by whether the repo's existing unit tests pass.

In this paper, they use it as their "agentic coding" task because it involves **multi-step tool use** (browsing files, editing code, running tests) rather than just answering a question. That makes it a natural fit for studying incoherence over longer action sequences — the x-axis in Figure 2b is "number of actions" rather than reasoning tokens.

The key finding for SWE-Bench specifically: as agents take more actions to solve an issue, their errors become increasingly dominated by variance (incoherence) rather than bias. Sonnet 4 and o3-mini show clear power-law scaling (α ≈ 0.40–0.49, high R²), while o4-mini's fit is poor (R² = 0.27), suggesting its incoherence pattern is less predictable on this task.

**Q:** What does this mean?

"notably, for QWEN3, incoherence levels and slopes are nearly identical across"

**A:** The full sentence is: "for QWEN3, incoherence levels and slopes are nearly identical across **all sizes**, even though larger models perform better."

They tested QWEN3 at 1.7B, 4B, 8B, 14B, and 32B parameters. The finding: **scaling up the model improves accuracy but does nothing to reduce incoherence.** You can see it in Figure 2(a) right panel — the α (slope) values are all clustered tightly around 0.34–0.43, and the curves basically overlap.

Why this matters: you might expect that a 32B model, being smarter, would also be more *consistent* — getting the same question right reliably rather than flip-flopping. But no. It gets more questions right on average (lower bias), while being just as noisy run-to-run (same variance). The incoherence power law w.r.t. reasoning length is essentially a property of the architecture/training recipe, not the scale.

This contrasts with the frontier models in the left panel, where Sonnet 4, o3-mini, and o4-mini each have noticeably different slopes (0.56, 0.40, 0.29) — suggesting that different training approaches (RLHF, distillation, etc.) *do* affect how quickly incoherence grows with reasoning length, even if raw scale within a family doesn't.

**Q (p.6):** How did they measure incoherence again? Did they use logprobs or look at generated answers and compare with ground truth? I think it was a mix, right?

**A:** Let me check the earlier pages where they define the metric.Your notes actually cover this well. It's **not logprobs directly** — it's closer to your second guess but with a specific twist. Here's how it works:

**The setup:** Run the model many times (many samples) on the same question under different random seeds / sampling noise. Each run produces a probability distribution over answer choices (e.g., [0.7, 0.2, 0.1] for A/B/C).

**The decomposition splits total error into two KL divergence terms:**

1. **Bias** = KL(truth ‖ consensus prediction). Average all runs into a "consensus" distribution f̄, then measure how far that consensus is from ground truth. This is the systematic error that re-running can't fix.

2. **Variance** = E[KL(consensus ‖ individual run)]. How much does each individual run scatter from the model's own consensus. This is the inconsistency.

3. **Incoherence** = Variance / Total Error. What fraction of the model's mistakes are random scatter vs. systematic aim.

**It varies by task type though:**

- **MCQ (GPQA, MMLU):** They extract the model's probability distribution over choices (they mention asking for probability estimates of choice correctness). KL-based decomposition.
- **SWE-Bench:** They use test coverage as the metric — which tests pass/fail across runs. Incoherence = variance in coverage / total coverage error.
- **Open-ended MWE:** No ground truth distribution possible, so they embed the text responses and measure **embedding variance** across runs. No bias term here — just pure variance of the output representation.

So you're right that it's a mix — KL-based for discrete choice tasks, coverage-based for agentic coding, embedding-based for open-ended generation. The unifying idea is always: sample many times, measure how much the outputs scatter.

### Session: 2026-04-07

**Q:** What does this mean?

"SWE-BENCH"

**A:** SWE-Bench is a benchmark for evaluating AI coding agents on real-world software engineering tasks. Each task is a actual GitHub issue from popular open-source Python repos (like Django, scikit-learn, sympy). The agent has to read the issue, navigate the codebase, and produce a patch — then success is measured by whether the repo's existing unit tests pass.

In this paper, they use it as their "agentic coding" task because it involves **multi-step tool use** (browsing files, editing code, running tests) rather than just answering a question. That makes it a natural fit for studying incoherence over longer action sequences — the x-axis in Figure 2b is "number of actions" rather than reasoning tokens.

The key finding for SWE-Bench specifically: as agents take more actions to solve an issue, their errors become increasingly dominated by variance (incoherence) rather than bias. Sonnet 4 and o3-mini show clear power-law scaling (α ≈ 0.40–0.49, high R²), while o4-mini's fit is poor (R² = 0.27), suggesting its incoherence pattern is less predictable on this task.

**Q:** What does this mean?

"notably, for QWEN3, incoherence levels and slopes are nearly identical across"

**A:** The full sentence is: "for QWEN3, incoherence levels and slopes are nearly identical across **all sizes**, even though larger models perform better."

They tested QWEN3 at 1.7B, 4B, 8B, 14B, and 32B parameters. The finding: **scaling up the model improves accuracy but does nothing to reduce incoherence.** You can see it in Figure 2(a) right panel — the α (slope) values are all clustered tightly around 0.34–0.43, and the curves basically overlap.

Why this matters: you might expect that a 32B model, being smarter, would also be more *consistent* — getting the same question right reliably rather than flip-flopping. But no. It gets more questions right on average (lower bias), while being just as noisy run-to-run (same variance). The incoherence power law w.r.t. reasoning length is essentially a property of the architecture/training recipe, not the scale.

This contrasts with the frontier models in the left panel, where Sonnet 4, o3-mini, and o4-mini each have noticeably different slopes (0.56, 0.40, 0.29) — suggesting that different training approaches (RLHF, distillation, etc.) *do* affect how quickly incoherence grows with reasoning length, even if raw scale within a family doesn't.

**Q:** How did they measure incoherence again? Did they use logprobs or look at generated answers and compare with ground truth? I think it was a mix, right?

**A:** Let me check the earlier pages where they define the metric.Your notes actually cover this well. It's **not logprobs directly** — it's closer to your second guess but with a specific twist. Here's how it works:

**The setup:** Run the model many times (many samples) on the same question under different random seeds / sampling noise. Each run produces a probability distribution over answer choices (e.g., [0.7, 0.2, 0.1] for A/B/C).

**The decomposition splits total error into two KL divergence terms:**

1. **Bias** = KL(truth ‖ consensus prediction). Average all runs into a "consensus" distribution f̄, then measure how far that consensus is from ground truth. This is the systematic error that re-running can't fix.

2. **Variance** = E[KL(consensus ‖ individual run)]. How much does each individual run scatter from the model's own consensus. This is the inconsistency.

3. **Incoherence** = Variance / Total Error. What fraction of the model's mistakes are random scatter vs. systematic aim.

**It varies by task type though:**

- **MCQ (GPQA, MMLU):** They extract the model's probability distribution over choices (they mention asking for probability estimates of choice correctness). KL-based decomposition.
- **SWE-Bench:** They use test coverage as the metric — which tests pass/fail across runs. Incoherence = variance in coverage / total coverage error.
- **Open-ended MWE:** No ground truth distribution possible, so they embed the text responses and measure **embedding variance** across runs. No bias term here — just pure variance of the output representation.

So you're right that it's a mix — KL-based for discrete choice tasks, coverage-based for agentic coding, embedding-based for open-ended generation. The unifying idea is always: sample many times, measure how much the outputs scatter.

**Q (p.6):** The observation that “larger, more capable and intelligent systems are often more incoherent.” makes sense so me → I mean isn’t that because we have a larger search space? That is a smaller model has less options and is probably more certain of a wrong direction, while through the shier number of parameters, a larger model may go off to explore directions a smaller model does not have access to. What do you think of this observation?

**A:** Your intuition is solid and I think basically right for the hard-task regime. But the paper's finding is more nuanced than just "bigger = more incoherent" — and the nuance is where it gets interesting.

**Where your intuition fits perfectly:** On hard questions, the larger model has access to more reasoning strategies that a smaller model literally can't represent. When no single path clearly dominates, different samples explore different viable-but-uncertain directions → higher variance. The smaller model, with its constrained representational capacity, is "stuck" in a narrower corridor — more consistently wrong, but consistently so.

**Where it needs refinement:** On easy tasks, the same expanded search space actually *reduces* incoherence. More capacity means the model finds the correct answer more reliably across runs. The key variable isn't search space size alone — it's search space size *relative to task difficulty*:

- Easy task + big model → many paths, but they converge on the right answer → low incoherence
- Hard task + big model → many paths, none dominant → high incoherence
- Hard task + small model → few paths, consistently picks the wrong one → low incoherence, high bias

So it's less "bigger search space = more noise" and more "bigger search space = the model's errors shift from systematic (bias) to stochastic (variance)." The total error might actually decrease with scale, but the *character* of the remaining error changes — what's left is increasingly incoherent rather than predictably wrong.

This is arguably the more concerning finding for safety. A model that's consistently wrong in a predictable way is easier to monitor and correct than one that's usually right but occasionally goes off in unpredictable directions. The incoherence scaling means that as models get more capable, their failure modes get *less* predictable — which is exactly the wrong direction for reliable deployment.

The QWEN3 result from earlier adds another wrinkle to your framing: within the same architecture family, scaling up doesn't change the incoherence *slope* (how fast it grows with reasoning length). So the search space expansion from more parameters affects *which* questions trigger incoherence (harder ones), but not the fundamental rate at which longer reasoning chains degrade consistency. That suggests the slope is more about the training recipe / architecture than raw capacity.

### Session: 2026-04-08

**Q (p.7):** What does this mean?

"In this paper, we are trying to disentangle whether capable models will
more tend to act as effective optimizers of the wrong goal"

**A:** This is the core safety-relevant question of the paper, framed through a bias-variance lens.

When a capable model fails at a task, there are two possible failure modes:

1. **Wrong goal, effective optimizer** (high bias, low variance) — the model consistently pursues the wrong objective. It's coherent but misaligned. This is the classic mesa-optimization worry from Hubinger et al. 2019.

2. **Right goal, ineffective optimizer** (low bias, high variance) — the model has learned the correct objective on average, but can't reliably execute it. It's aligned but incoherent.

The paper's synthetic experiment (training transformers to literally mimic an optimizer descending a quadratic) lets them cleanly measure this. The finding: **larger models reduce bias much faster than variance** (Fig. 6 right, bias α=2.165 vs variance α=0.878). They learn *what* to optimize before they learn to do it *consistently*.

The safety implication: current capability gains are buying you models that know the right answer in expectation but give you wildly different outputs each time. That's arguably less dangerous than the alternative (a model that coherently pursues the wrong goal), but it means you can't trust any single output from a capable model on hard tasks — even when the model "knows" the right approach.

### Session: 2026-04-08

**Q:** What does this mean?

"In this paper, we are trying to disentangle whether capable models will
more tend to act as effective optimizers of the wrong goal"

**A:** This is the core safety-relevant question of the paper, framed through a bias-variance lens.

When a capable model fails at a task, there are two possible failure modes:

1. **Wrong goal, effective optimizer** (high bias, low variance) — the model consistently pursues the wrong objective. It's coherent but misaligned. This is the classic mesa-optimization worry from Hubinger et al. 2019.

2. **Right goal, ineffective optimizer** (low bias, high variance) — the model has learned the correct objective on average, but can't reliably execute it. It's aligned but incoherent.

The paper's synthetic experiment (training transformers to literally mimic an optimizer descending a quadratic) lets them cleanly measure this. The finding: **larger models reduce bias much faster than variance** (Fig. 6 right, bias α=2.165 vs variance α=0.878). They learn *what* to optimize before they learn to do it *consistently*.

The safety implication: current capability gains are buying you models that know the right answer in expectation but give you wildly different outputs each time. That's arguably less dangerous than the alternative (a model that coherently pursues the wrong goal), but it means you can't trust any single output from a capable model on hard tasks — even when the model "knows" the right approach.

**Q (p.9):** What is the brier incoherence or brier component again? e.g., figure 7?

**A:** **Brier score** is just a different scoring rule than KL divergence for measuring prediction quality. For a binary outcome y ∈ {0,1} and predicted probability p:

**Brier = (p − y)²**

It decomposes cleanly into:
- **Brier Bias** = (p̄ − ȳ)² — how far the *average* prediction is from truth
- **Brier Variance** = E[(p − p̄)²] — how much individual predictions scatter around that average

**Brier Incoherence** = Variance / (Bias + Variance) — same concept as KL-Incoherence, just computed with Brier components instead of KL.

They switch to Brier for the ensembling experiment (Fig 7b) because it has a clean theoretical property: **averaging E independent samples reduces variance by exactly 1/E**. That's why the variance slope is α = −1.00 — it matches the theoretical prediction perfectly. KL doesn't have this neat closed-form relationship with ensemble size, so Brier makes the demonstration crisper.

The takeaway is the same either way: ensembling kills variance without touching bias, so incoherence drops. They note in the text that KL-Incoherence plots (Fig. 11) show the same pattern.

### Session: 2026-04-08

**Q:** What does this mean?

"In this paper, we are trying to disentangle whether capable models will
more tend to act as effective optimizers of the wrong goal"

**A:** This is the core safety-relevant question of the paper, framed through a bias-variance lens.

When a capable model fails at a task, there are two possible failure modes:

1. **Wrong goal, effective optimizer** (high bias, low variance) — the model consistently pursues the wrong objective. It's coherent but misaligned. This is the classic mesa-optimization worry from Hubinger et al. 2019.

2. **Right goal, ineffective optimizer** (low bias, high variance) — the model has learned the correct objective on average, but can't reliably execute it. It's aligned but incoherent.

The paper's synthetic experiment (training transformers to literally mimic an optimizer descending a quadratic) lets them cleanly measure this. The finding: **larger models reduce bias much faster than variance** (Fig. 6 right, bias α=2.165 vs variance α=0.878). They learn *what* to optimize before they learn to do it *consistently*.

The safety implication: current capability gains are buying you models that know the right answer in expectation but give you wildly different outputs each time. That's arguably less dangerous than the alternative (a model that coherently pursues the wrong goal), but it means you can't trust any single output from a capable model on hard tasks — even when the model "knows" the right approach.

**Q:** What is the brier incoherence or brier component again? e.g., figure 7?

**A:** **Brier score** is just a different scoring rule than KL divergence for measuring prediction quality. For a binary outcome y ∈ {0,1} and predicted probability p:

**Brier = (p − y)²**

It decomposes cleanly into:
- **Brier Bias** = (p̄ − ȳ)² — how far the *average* prediction is from truth
- **Brier Variance** = E[(p − p̄)²] — how much individual predictions scatter around that average

**Brier Incoherence** = Variance / (Bias + Variance) — same concept as KL-Incoherence, just computed with Brier components instead of KL.

They switch to Brier for the ensembling experiment (Fig 7b) because it has a clean theoretical property: **averaging E independent samples reduces variance by exactly 1/E**. That's why the variance slope is α = −1.00 — it matches the theoretical prediction perfectly. KL doesn't have this neat closed-form relationship with ensemble size, so Brier makes the demonstration crisper.

The takeaway is the same either way: ensembling kills variance without touching bias, so incoherence drops. They note in the text that KL-Incoherence plots (Fig. 11) show the same pattern.

**Q (p.9):** And how does it differ to MSE?

**A:** It doesn't. Brier score **is** MSE applied to probability predictions. (p − y)² = MSE when y ∈ {0,1} and p is a predicted probability.

The name "Brier score" is just the forecasting/calibration literature's convention for MSE in this specific context. They use the term because it signals "we're evaluating probabilistic predictions against binary outcomes" rather than generic regression — but the math is identical.

### Session: 2026-04-08

**Q (p.7):** How come the paper says this "Since the implementation details
of reasoning budgets for frontier models are not public, it is unclear how exactly it can improve
incoherence." AND this earlier: " reasoning length is a stronger indicator of incoherence than model size". Like they say both reasoning length is the cause for higher incoherence but also reduces incoherence. What am I missing?

**A:** Good catch — it does look contradictory on the surface, but they're talking about two different things:

**Natural reasoning length** is an *observed symptom*. When a model naturally produces a longer chain of thought on a given question, that's a signal it's struggling — exploring multiple paths, second-guessing itself. This correlates with incoherence because the model is in uncertain territory. It's not that the length *causes* incoherence; both are downstream of task difficulty/model confusion.

**Reasoning budgets** are an *externally imposed constraint* (the API parameter like OpenAI's `reasoning_effort`). Setting a higher budget gives the model more structured compute to work through the problem systematically. This is deliberate allocation, not emergent floundering.

The analogy: if a student spends 3 hours on a problem that usually takes 30 minutes, that's a bad sign (natural length → confusion → inconsistent answers). But if you *schedule* 3 hours of structured study time for an exam, that helps (budget → more resources → more consistent answers).

The paper is actually internally consistent here — they even say the budget effect is much weaker than the natural variation effect (Section 3.3.1 header: "Reasoning budgets reduce incoherence, but natural variation has a much stronger effect"). The structured allocation helps a little, but the emergent struggle effect dominates.

The honest admission about not knowing *how* budgets help is separate — they can observe the effect without understanding the mechanism, since the implementation is proprietary.

### Session: 2026-04-08

**Q:** How come the paper says this "Since the implementation details
of reasoning budgets for frontier models are not public, it is unclear how exactly it can improve
incoherence." AND this earlier: " reasoning length is a stronger indicator of incoherence than model size". Like they say both reasoning length is the cause for higher incoherence but also reduces incoherence. What am I missing?

**A:** Good catch — it does look contradictory on the surface, but they're talking about two different things:

**Natural reasoning length** is an *observed symptom*. When a model naturally produces a longer chain of thought on a given question, that's a signal it's struggling — exploring multiple paths, second-guessing itself. This correlates with incoherence because the model is in uncertain territory. It's not that the length *causes* incoherence; both are downstream of task difficulty/model confusion.

**Reasoning budgets** are an *externally imposed constraint* (the API parameter like OpenAI's `reasoning_effort`). Setting a higher budget gives the model more structured compute to work through the problem systematically. This is deliberate allocation, not emergent floundering.

The analogy: if a student spends 3 hours on a problem that usually takes 30 minutes, that's a bad sign (natural length → confusion → inconsistent answers). But if you *schedule* 3 hours of structured study time for an exam, that helps (budget → more resources → more consistent answers).

The paper is actually internally consistent here — they even say the budget effect is much weaker than the natural variation effect (Section 3.3.1 header: "Reasoning budgets reduce incoherence, but natural variation has a much stronger effect"). The structured allocation helps a little, but the emergent struggle effect dominates.

The honest admission about not knowing *how* budgets help is separate — they can observe the effect without understanding the mechanism, since the implementation is proprietary.

**Q (p.10):** What exactly are inverse scaling trends/law and how does that relate to incoherence of this paper?

**A:** **Inverse scaling** is when making a model bigger or giving it more compute makes it *worse* at a task — the opposite of the normal expectation that scale improves performance.

The classic "scaling laws" say: more parameters, more data, more compute → better performance. Inverse scaling is when that relationship flips for specific tasks. The Inverse Scaling Prize (2023) collected concrete examples — things like models becoming more confidently wrong on trick questions, or repeating a user's false premise more readily as they get larger.

**How it connects to this paper:**

The related work section (page 10) cites recent studies finding that extended reasoning *degrades* performance — a form of inverse scaling where the "scale" axis is reasoning length rather than model size. More thinking → worse answers on some tasks.

This paper's contribution is more specific than just "inverse scaling exists." They decompose the error into **bias** and **variance**:

- **Bias** = systematic, consistent errors (the model reliably gets something wrong the same way)
- **Variance/incoherence** = inconsistent errors (the model gives different answers each time)

Previous inverse scaling work showed "performance gets worse" but didn't distinguish *how* it gets worse. This paper says: the degradation from longer reasoning is primarily **variance** (incoherence), not bias. The model isn't learning a consistent wrong answer — it's becoming *flaky*.

That distinction matters for safety: a biased model pursues a stable (if wrong) goal — you can detect and correct that. An incoherent model fails unpredictably — the "hot mess" scenario. Their argument is that as we deploy more capable models on longer tasks, the dominant failure mode won't be scheming AIs with coherent misaligned goals, but unreliable systems that fail in ways that don't correspond to *any* stable objective.

### Session: 2026-04-08

**Q:** How come the paper says this "Since the implementation details
of reasoning budgets for frontier models are not public, it is unclear how exactly it can improve
incoherence." AND this earlier: " reasoning length is a stronger indicator of incoherence than model size". Like they say both reasoning length is the cause for higher incoherence but also reduces incoherence. What am I missing?

**A:** Good catch — it does look contradictory on the surface, but they're talking about two different things:

**Natural reasoning length** is an *observed symptom*. When a model naturally produces a longer chain of thought on a given question, that's a signal it's struggling — exploring multiple paths, second-guessing itself. This correlates with incoherence because the model is in uncertain territory. It's not that the length *causes* incoherence; both are downstream of task difficulty/model confusion.

**Reasoning budgets** are an *externally imposed constraint* (the API parameter like OpenAI's `reasoning_effort`). Setting a higher budget gives the model more structured compute to work through the problem systematically. This is deliberate allocation, not emergent floundering.

The analogy: if a student spends 3 hours on a problem that usually takes 30 minutes, that's a bad sign (natural length → confusion → inconsistent answers). But if you *schedule* 3 hours of structured study time for an exam, that helps (budget → more resources → more consistent answers).

The paper is actually internally consistent here — they even say the budget effect is much weaker than the natural variation effect (Section 3.3.1 header: "Reasoning budgets reduce incoherence, but natural variation has a much stronger effect"). The structured allocation helps a little, but the emergent struggle effect dominates.

The honest admission about not knowing *how* budgets help is separate — they can observe the effect without understanding the mechanism, since the implementation is proprietary.

**Q:** What exactly are inverse scaling trends/law and how does that relate to incoherence of this paper?

**A:** **Inverse scaling** is when making a model bigger or giving it more compute makes it *worse* at a task — the opposite of the normal expectation that scale improves performance.

The classic "scaling laws" say: more parameters, more data, more compute → better performance. Inverse scaling is when that relationship flips for specific tasks. The Inverse Scaling Prize (2023) collected concrete examples — things like models becoming more confidently wrong on trick questions, or repeating a user's false premise more readily as they get larger.

**How it connects to this paper:**

The related work section (page 10) cites recent studies finding that extended reasoning *degrades* performance — a form of inverse scaling where the "scale" axis is reasoning length rather than model size. More thinking → worse answers on some tasks.

This paper's contribution is more specific than just "inverse scaling exists." They decompose the error into **bias** and **variance**:

- **Bias** = systematic, consistent errors (the model reliably gets something wrong the same way)
- **Variance/incoherence** = inconsistent errors (the model gives different answers each time)

Previous inverse scaling work showed "performance gets worse" but didn't distinguish *how* it gets worse. This paper says: the degradation from longer reasoning is primarily **variance** (incoherence), not bias. The model isn't learning a consistent wrong answer — it's becoming *flaky*.

That distinction matters for safety: a biased model pursues a stable (if wrong) goal — you can detect and correct that. An incoherent model fails unpredictably — the "hot mess" scenario. Their argument is that as we deploy more capable models on longer tasks, the dominant failure mode won't be scheming AIs with coherent misaligned goals, but unreliable systems that fail in ways that don't correspond to *any* stable objective.

**Q (p.10):** But the paper also says overall error reduces but the variance share increases, that is not inverse scaling, is it?

**A:** You're right, and I overstated the connection. Let me correct that.

**This paper's core finding is NOT inverse scaling.** Overall error goes down with scale — models genuinely get better. What changes is the *composition* of the remaining error: bias shrinks faster than variance, so incoherence becomes a larger *share* of a smaller total error.

That's a subtler and more interesting claim than "bigger models are worse." It's more like: as you squeeze out the easy, systematic mistakes through scaling, what's left over is increasingly the noisy, inconsistent kind.

**The inverse scaling literature** is cited in related work as *adjacent* — those papers found cases where longer reasoning actively hurts performance. This paper's relationship to that work is:

1. They *confirm* that naturally longer reasoning correlates with worse outcomes (the variance accumulation finding)
2. But their framing is different — they're not saying "scale is bad," they're saying "even when scale helps on average, the residual failures become dominated by incoherence"

The safety implication is about the *limit*: as models approach very low overall error, the errors that remain will be disproportionately incoherent rather than systematic. So the threat model shifts from "the AI consistently pursues a wrong goal" toward "the AI is mostly great but fails unpredictably in ways that don't correspond to any goal at all."

So yes — calling it inverse scaling would be wrong. It's more like **asymmetric error reduction**: bias scales away; variance is stickier.

## Connections

<!-- How this paper relates to others we've read -->
<!-- Use wikilinks: [[other-paper]] -->

## Open Questions

<!-- Things we want to investigate further -->
