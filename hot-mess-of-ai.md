---
title: "The Hot Mess of AI"
authors: [McKee et al.]
year: 2026
arxiv: "2601.23045"
url: "https://arxiv.org/abs/2601.23045"
tags: [alignment, evaluation, scaling, bias-variance, incoherence, ensembling, dynamical-systems]
status: discussed
date_read: 2026-04-07
date_discussed: 2026-04-08
connections:
  - "[[inverse-scaling-test-time-compute]]"
  - "[[thought-anchors]]"
  - "[[thought-branches]]"
  - "[[inverse-scaling-incoherence]]"
code: ""
---

# The Hot Mess of AI

> **One-line takeaway:** As AI models scale, their failures become more random (incoherent) rather than more systematic — they become "hot messes" rather than reliably wrong. This is *not* inverse scaling: overall error still decreases, but the remaining error is increasingly dominated by variance.

## Summary

This ICLR 2026 paper empirically tests Jascha Sohl-Dickstein's "hot mess theory" of AI misalignment. Using a bias-variance decomposition of model error, they show that larger models don't just get more accurate — their remaining errors shift from systematic (biased) to random (high-variance). The **incoherence ratio** (variance / total error) increases with scale, meaning bigger models fail in less predictable ways. They validate this across MCQ benchmarks (GPQA, MMLU), agentic coding (SWE-Bench), open-ended generation, and a synthetic optimizer task.

## Key Contributions

- Formal bias-variance decomposition for **cross-entropy loss** using KL divergence (not just squared error)
- The **incoherence metric**: variance / total error — quantifies how "randomly wrong" vs "systematically wrong" a model is
- Empirical evidence across model families that incoherence increases with scale
- Key distinction: **natural reasoning length** correlates with *higher* incoherence (a symptom of struggle), while **reasoning budgets** slightly *reduce* it (structured compute allocation) — but natural variation dominates
- Ensembling reduces variance at 1/E rate without affecting bias, but is impractical for agentic workflows where state can't be reset
- Connects to Sohl-Dickstein's theoretical prediction that more capable agents behave less coherently when they fail

## Method

### Bias-Variance Decomposition (Cross-Entropy / KL version)

The key formula decomposes expected cross-entropy error:

$$\mathbb{E}_\varepsilon[\text{CE}(y, f_\varepsilon)] = D_{\text{KL}}(y \| \bar{f}) + \mathbb{E}_\varepsilon[D_{\text{KL}}(\bar{f} \| f_\varepsilon)]$$

Where:
- **ε** = randomness source (different seeds, sampling temperatures). Each ε gives a different model run
- **f_ε** = model's output for one specific run — a probability distribution over C classes
- **f̄** = average prediction across all runs — the "consensus" prediction
- **y** = true answer (one-hot vector)
- **D_KL** = KL divergence between probability distributions

**Bias² = D_KL(y ‖ f̄):** How far is the consensus prediction from truth? This is the systematic error that re-running can't fix.

**Variance = E_ε[D_KL(f̄ ‖ f_ε)]:** How much does any individual run stray from its own consensus? This is pure inconsistency.

### The Dartboard Analogy

- **Bias** = your darts cluster away from the bullseye (systematic aim problem)
- **Variance** = your darts scatter widely (shaky hand)
- **Incoherence ratio** = what fraction of your error is shakiness vs. bad aim

As models scale: aim improves (bias drops), but the remaining misses become scattered rather than clustered → incoherence ratio rises.

### Measurement Varies by Task Type

- **MCQ (GPQA, MMLU):** Extract model's probability distribution over choices. KL-based decomposition.
- **SWE-Bench:** Test coverage as metric — which tests pass/fail across runs. Incoherence = variance in coverage / total coverage error.
- **Open-ended (MWE):** No ground truth distribution possible — embed text responses and measure **embedding variance** across runs. No bias term, just pure variance.

### Brier Score Connection

Brier score = MSE applied to probability predictions: Brier = (p − y)². It's a **proper scoring rule** (can't game it by hedging). Decomposes into Brier Bias = (p̄ − ȳ)² and Brier Variance = E[(p − p̄)²]. They switch to Brier for the ensembling experiment (Fig 7b) because averaging E samples reduces variance by exactly 1/E — a clean theoretical relationship KL doesn't offer.

## Results

### Core Findings

- Incoherence ratio consistently increases with model scale across families
- Larger models are more accurate overall, but their remaining errors are less predictable
- **Reasoning length is a stronger indicator of incoherence than model size**
- QWEN3 family: scaling from 1.7B to 32B improves accuracy but does *nothing* to reduce incoherence — slopes cluster at α ≈ 0.34–0.43. The incoherence power law is a property of architecture/training recipe, not scale.
- Frontier models (Sonnet 4, o3-mini, o4-mini) have noticeably different slopes (0.56, 0.40, 0.29) — training approach matters more than size

### SWE-Bench (Agentic Coding)

As agents take more actions, errors become increasingly dominated by variance. Sonnet 4 and o3-mini show clear power-law scaling (α ≈ 0.40–0.49, high R²). o4-mini's fit is poor (R² = 0.27) — less predictable incoherence pattern.

### Ensembling (Fig 7)

Averaging multiple independent samples reduces variance at 1/E rate without affecting bias → incoherence drops. But ensembling is impractical for agentic action loops where state can't be reset.

### Synthetic Optimizer Experiment

Training transformers to mimic an optimizer descending a quadratic. **Bias scales away much faster than variance** (α=2.165 vs α=0.878). Models learn *what* to optimize before they learn to do it *consistently*.

## Discussion Notes

### The Core Safety Question (p.7)

When a capable model fails, two possible failure modes:

1. **Wrong goal, effective optimizer** (high bias, low variance) — coherent but misaligned. Classic mesa-optimization worry (Hubinger et al. 2019).
2. **Right goal, ineffective optimizer** (low bias, high variance) — aligned but incoherent.

The finding: current capability gains buy models that know the right answer in expectation but give wildly different outputs each time. Arguably less dangerous than coherent misalignment, but means you can't trust any single output on hard tasks.

### Why Bigger Models Are More Incoherent

The intuition: larger models have access to more reasoning strategies. On hard questions where no single path dominates, different samples explore different viable-but-uncertain directions → higher variance. Smaller models are "stuck" in a narrower corridor — more consistently wrong, but consistently so.

The key variable is search space size *relative to task difficulty*:
- Easy task + big model → many paths converge → low incoherence
- Hard task + big model → many paths, none dominant → high incoherence
- Hard task + small model → few paths, consistently wrong → low incoherence, high bias

The dynamical systems framing (p.10): models trace trajectories in high-dimensional state space. The set of dynamical systems that act as optimizers of a fixed loss is **measure zero** in the space of all dynamical systems. As models scale, their state/action space expands, making it harder to constrain them to act as coherent optimizers.

### Natural Reasoning Length vs. Reasoning Budgets

These sound contradictory but aren't:
- **Natural length** = observed symptom. Longer CoT on a question signals struggle — both length and incoherence are downstream of task difficulty.
- **Reasoning budgets** = externally imposed structured compute. Helps a little, but the effect is much weaker than natural variation.

Analogy: a student spending 3 hours on a 30-min problem is a bad sign (confusion). Scheduling 3 hours of structured study is helpful (resources).

### This Is NOT Inverse Scaling

Important distinction: overall error *does* decrease with scale. What changes is the *composition* of residual error — bias shrinks faster than variance. This is **asymmetric error reduction**, not inverse scaling. The threat model shifts from "AI consistently pursues wrong goal" toward "AI is mostly great but fails unpredictably in ways corresponding to no stable objective."

### Intelligence and Incoherence May Correlate Generally

The survey ranking (Fig 4b) shows ρ=0.65 correlation between judged intelligence and judged incoherence across AI models, non-human beings, humans, and organizations. This may be a general property, not AI-specific.

## Research Questions & Ideas

### From the reading:
- How can ensembling be adapted to agentic workflows where state can't be reset? What alternative error correction mechanisms exist?
- If overall error goes down but variance share goes up, isn't the system still strictly better? (Yes, but individual outputs are less trustable)
- Can you decompose Bias further into Bias(mesa) and Bias(spec) — separating "wrong specification" from "wrong mesa-objective"? Is this mathematically tractable?
- What's the effect of ensemble size and voting mechanism on safety/alignment?
- Will compute abundance eventually make ensembling practical enough to improve alignment?

### From the paper:
- Does the incoherence ratio plateau or keep increasing with scale?
- How does RLHF/fine-tuning affect coherence of failures?
- Can the incoherence metric serve as a practical safety evaluation tool?
- Could incoherent failure modes mask systematic deception?
- What are the mechanistic origins of incoherence? → see [[inverse-scaling-incoherence]]

## Connections

- [[inverse-scaling-test-time-compute]] — Gema et al. 2025 — complementary finding: more test-time compute can actively degrade performance. Hot Mess shows failures become incoherent with scale; Inverse Scaling shows more compute can hurt. Both challenge "more compute = better"
- [[thought-anchors]] — causal attribution within CoT traces; tool for investigating *where* incoherent failures originate inside a single reasoning chain
- [[thought-branches]] — branch point detection in reasoning; tool for finding *where* reasoning could have diverged toward a different answer
- [[inverse-scaling-incoherence]] — our research project applying these tools to explain *why* inverse scaling and incoherence occur mechanistically

## Code & Experiments

<!-- Links to notebooks, scripts, or repos where we tested ideas from this paper -->
