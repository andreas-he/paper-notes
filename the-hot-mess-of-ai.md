---
title: "The Hot Mess of AI"
authors: [McKee et al.]
year: 2026
arxiv: "2601.23045"
url: "https://arxiv.org/abs/2601.23045"
tags: [alignment, evaluation, scaling, bias-variance, incoherence]
status: discussed
date_read: 2026-04-06
date_discussed: 2026-04-07
connections:
  - "[[hot-mess-theory-of-ai-misalignment]]"
  - "[[inverse-scaling-test-time-compute]]"
code: ""
---

# The Hot Mess of AI

> **One-line takeaway:** As AI models scale, their failures become more random (incoherent) rather than more systematic — they become "hot messes" rather than reliably wrong.

## Summary

This ICLR 2026 paper empirically tests Jascha Sohl-Dickstein's "hot mess theory" of AI misalignment. Using a bias-variance decomposition of model error, they show that larger models don't just get more accurate — their remaining errors shift from systematic (biased) to random (high-variance). The **incoherence ratio** (variance / total error) increases with scale, meaning bigger models fail in less predictable ways.

## Key Contributions

- Formal bias-variance decomposition for **cross-entropy loss** using KL divergence (not just squared error)
- The **incoherence metric**: variance / total error — quantifies how "randomly wrong" vs "systematically wrong" a model is
- Empirical evidence across model families that incoherence increases with scale
- Connects to Sohl-Dickstein's theoretical prediction that more capable agents behave less coherently when they fail

## Method

### Bias-Variance Decomposition (Cross-Entropy / KL version)

The key formula decomposes expected cross-entropy error:

$$\mathbb{E}_\varepsilon[\text{CE}(y, f_\varepsilon)] = D_{\text{KL}}(y \| \bar{f}) + \mathbb{E}_\varepsilon[D_{\text{KL}}(\bar{f} \| f_\varepsilon)]$$

Where:
- **ε** = randomness source (different seeds, sampling temperatures). Each ε gives a different model run
- **f_ε** = model's output for one specific run — a probability distribution over C classes (e.g., [0.7, 0.2, 0.1])
- **f̄** = average prediction across all runs — the "consensus" prediction
- **y** = true answer (one-hot vector)
- **D_KL** = KL divergence between probability distributions

**Left side (Total Error):** Run the model many times, measure cross-entropy against truth each time, average. The expanded form Σ y[c] log(f_ε[c]) just says: for each class, multiply the true label (0 or 1) by the log of predicted probability. Since y is one-hot, only the correct class survives — so it measures how much probability mass landed on the right answer.

**Bias² = D_KL(y ‖ f̄):** How far is the consensus prediction from truth? If you average 1000 runs and the consensus says [0.4, 0.3, 0.3] but truth is [1, 0, 0], that gap is bias. The model's average aim is off — no amount of re-running fixes this.

**Variance = E_ε[D_KL(f̄ ‖ f_ε)]:** How much does any individual run stray from its own consensus? The consensus might be [0.4, 0.3, 0.3], but run #17 outputs [0.1, 0.8, 0.1]. This is pure inconsistency — even if the consensus were perfect, individual runs scatter.

### Why KL Divergence Instead of Squared Error?

Because outputs are probability distributions, not single numbers. KL divergence is the natural "distance" between distributions — it asks "how many extra bits of information do I need if I use distribution q when truth is p?" It's the information-theoretic equivalent of squared error, and what these models actually minimize during training (cross-entropy loss = KL divergence + constant).

### The Dartboard Analogy

- **Bias** = your darts cluster away from the bullseye (systematic aim problem)
- **Variance** = your darts scatter widely (shaky hand)
- **Incoherence ratio** = what fraction of your error is shakiness vs. bad aim

As models scale: aim improves (bias drops), but the remaining misses become scattered rather than clustered → incoherence ratio rises.

### Brier Score Connection

The Brier score (mean squared error of probabilistic predictions) is used as an alternative error metric:

$$\text{Brier} = \frac{1}{N} \sum_{i=1}^{N} (p_i - o_i)^2$$

- **0** = perfect predictions
- **0.25** = no skill (always predicting 50%)
- **1** = perfectly wrong

It's a **proper scoring rule** — the optimal strategy is to report true beliefs. Can't game it by hedging. This makes it the right foundation for bias-variance decomposition where you need to separate "coherently wrong" from "randomly wrong."

## Results

- Incoherence ratio consistently increases with model scale across families
- Larger models are more accurate overall, but their remaining errors are less predictable
- This holds across different benchmarks and model families

## Discussion Notes

- The paper is an empirical test of Sohl-Dickstein's 2023 blog post theory
- Key implication for alignment: we can't rely on failures being systematic/predictable at scale — safety testing needs to account for incoherent failure modes
- Connects to inverse scaling phenomena — more compute doesn't always help, and *how* it fails matters as much as *how often*

## Connections

- [[hot-mess-theory-of-ai-misalignment]] — Sohl-Dickstein's 2023 original theory that this paper empirically validates
- [[inverse-scaling-test-time-compute]] — Gema et al. 2025 — complementary finding: more test-time compute can actively degrade performance. Hot Mess shows failures become incoherent with scale; Inverse Scaling shows more compute can hurt. Both challenge "more compute = better"
- [[thought-anchors]] — causal attribution within CoT traces; tool for investigating *where* incoherent failures originate inside a single reasoning chain
- [[thought-branches]] — branch point detection in reasoning; tool for finding *where* reasoning could have diverged toward a different (correct or incorrect) answer
- [[research-inverse-scaling-incoherence]] — our research project applying these tools to explain *why* inverse scaling and incoherence occur

## Open Questions

- Does the incoherence ratio plateau or keep increasing with scale?
- How does this interact with RLHF/fine-tuning — does alignment training make failures more or less coherent?
- Can the incoherence metric be used as a practical safety evaluation tool?
- What does this mean for ensemble methods — if errors are incoherent, ensembling should help more at scale?
- Connection to deceptive alignment: could an incoherent failure mode mask systematic deception?

## Code & Experiments

<!-- Links to notebooks, scripts, or repos where we tested ideas from this paper -->
