---
title: "Research Project: Why Does Inverse Scaling Happen?"
tags: [research-project, inverse-scaling, incoherence, mechanistic-interpretability, test-time-compute]
status: planning
papers:
  - "[[hot-mess-of-ai]]"
  - "[[inverse-scaling-test-time-compute]]"
  - "[[emotion-concepts-llm]]"
  - "[[thought-anchors]]"
  - "[[thought-branches]]"
last_updated: 2026-04-07
---

# Why Does Inverse Scaling Happen? Mechanistic Analysis of Intra-Trace Incoherence

> **Research question:** What mechanistic or behavioral explanations account for inverse scaling — the phenomenon where more capability or more test-time reasoning degrades model performance?

---

## 1. Motivation

Two Anthropic-adjacent papers establish that:

1. **Inverse Scaling in Test-Time Compute** (Gema et al., 2025): Extended reasoning can *reduce* accuracy. Models get distracted by irrelevant details, overfit misleading framings, latch onto spurious correlations, or lose track of constraints over long deductions.

2. **The Hot Mess of AI** (McKee et al., 2026): As models scale, remaining errors shift from systematic (biased) to random (high-variance). The *incoherence ratio* (variance / total error) increases with scale.

**The gap:** Both papers document *what* happens but leave *why* largely unexplored. Hot Mess focuses on *inter-trace* variance (different sampled traces → different outcomes). Nobody has systematically studied *intra-trace* dynamics — what happens *inside* a single reasoning chain that causes it to go off the rails.

**Why it matters:** As we delegate more complex tasks to AI, we need models to be *more* reliable, not less. Understanding the mechanism behind inverse scaling is a prerequisite for mitigating it.

---

## 2. Research Plan Overview

### Phase 1: Intra-Trace Causal Analysis
Map where within reasoning traces the model commits to (or flips away from) its final answer, using causal attribution methods from Thought Anchors and Thought Branches.

### Phase 2: Pattern Discovery
Characterize the turning points. What distinguishes traces that succeed from those that fail? What happens at the token/sentence level right before and after a flip?

### Phase 3: Mechanistic Hypotheses
Formulate and test specific hypotheses about *why* these turning points occur. Candidate lenses include emotion concept activations, attention pattern shifts, representation drift, and constraint-tracking failure.

### Phase 4: Intervention & Mitigation
If we identify a mechanism, test whether targeted interventions (e.g., steering vectors, selective token masking, trace truncation) can reduce inverse scaling.

---

## 3. Detailed Research Structure

### 3.1 Phase 1 — Intra-Trace Causal Mapping

**Goal:** For a set of tasks exhibiting inverse scaling, identify which tokens/sentences within each reasoning trace causally determine the final answer.

**Method:**
- Select benchmark tasks from the inverse scaling literature where extended reasoning degrades performance (datasets from Gema et al.)
- Generate multiple reasoning traces per task at varying lengths (controlling via max_tokens or explicit "think step by step for N steps" prompts)
- Apply **Thought Anchors** analysis: determine which tokens, when ablated or counterfactually replaced, flip the model's final answer
- Apply **Thought Branches** analysis: identify branching points where the trace could have diverged toward a different conclusion
- Label each trace with: (a) final answer correctness, (b) identified anchor tokens, (c) branch points, (d) the "commitment point" — the earliest token after which the answer no longer changes under perturbation

**Key outputs:**
- Distribution of commitment points relative to trace length (do models commit early or late?)
- Correlation between commitment point position and answer correctness
- Taxonomy of branch point types (e.g., factual recall, logical step, framing interpretation)

### 3.2 Phase 2 — Pattern Discovery at Turning Points

**Goal:** Characterize what's happening at anchor/branch points. Look for systematic patterns that distinguish success from failure.

**Analyses:**
1. **Content analysis** of anchor tokens — are they reasoning steps, restated premises, hedging language, emotional language, self-corrections?
2. **Attention pattern shifts** — do attention heads reorganize at branch points? Is there a "lost in the middle" effect within the trace?
3. **Representation drift** — track how the model's internal representation of the problem drifts over the trace. Does the residual stream diverge from the problem encoding?
4. **Constraint tracking** — for tasks with multiple constraints (e.g., multi-step math, logic puzzles), do branch points coincide with the model dropping or confusing constraints?
5. **Emotion concept activation** (exploratory) — using the Emotion Concepts framework (Trott et al.), measure whether emotion-adjacent activations (frustration, confidence, uncertainty) spike at turning points

**Key outputs:**
- Feature vectors characterizing successful vs. failed turning points
- Statistical tests for which features predict trace failure
- Qualitative case studies of representative success/failure traces

### 3.3 Phase 3 — Mechanistic Hypothesis Testing

Based on Phase 2 patterns, formulate specific hypotheses. Candidate hypotheses (to be refined):

**H1: Attention Dilution.** Longer traces dilute attention over earlier tokens, causing the model to lose track of the original problem constraints. Predict: anchor points cluster at positions where key constraint tokens fall below an attention threshold.

**H2: Representation Drift.** The model's internal representation of the problem gradually drifts as the chain of thought progresses, eventually crossing into a basin of attraction for the wrong answer. Predict: cosine similarity between the residual stream at trace start vs. each subsequent step shows a characteristic "drift then snap" pattern for failed traces.

**H3: Confidence Cascade.** The model generates a tentative (wrong) intermediate conclusion, then treats it as established fact in subsequent reasoning. Predict: branch points show a spike in "certainty" features (e.g., tokens like "clearly", "therefore", "we know") right before committing to an error.

**H4: Sycophantic Self-Reinforcement.** The model's reasoning sycophantically reinforces its own earlier statements rather than critically evaluating them. Predict: in failed traces, later tokens attend disproportionately to the model's own prior conclusions rather than the original problem statement.

**H5: Emotional Framing Sensitivity.** Emotionally charged language in the problem or in the model's own reasoning destabilizes the trace. Predict: emotion concept vector activations at turning points are higher in failed traces than successful ones.

**Testing approach:** Each hypothesis generates a quantitative prediction. Design targeted experiments (ablations, probing, causal interventions) to test each.

### 3.4 Phase 4 — Intervention & Mitigation

**Goal:** If a mechanism is confirmed, test whether we can reduce inverse scaling through targeted intervention.

**Candidate interventions** (depends on which hypothesis is supported):
- **Attention anchoring:** Force attention back to problem statement tokens at regular intervals
- **Trace truncation:** Identify the optimal reasoning length before commitment quality degrades
- **Confidence calibration:** Detect and flag high-confidence intermediate conclusions for re-evaluation
- **Steering vectors:** Apply representation engineering to counteract drift at identified branch points
- **Selective backtracking:** At detected branch points, force the model to regenerate from that point

---

## 4. Datasets & Models

### Tasks / Benchmarks
- **Primary:** Tasks from Gema et al. (2025) that show inverse scaling with extended reasoning
  - Focus on tasks where performance degrades monotonically with reasoning length (strongest signal)
  - Categories: multi-step math, logic puzzles, reading comprehension with distractors
- **Secondary:** MMLU subsets, BBH tasks where Hot Mess found high incoherence ratios

### Models
- **Primary:** Open-weight models with accessible internals (Llama 3, Qwen 2.5, Gemma 2) — needed for representation analysis
- **Secondary:** API models (Claude, GPT) — for behavioral-only analyses (trace content, commitment points)
- Model size sweep within a family to test whether intra-trace patterns change with scale

### Scale
- Start with ~100 tasks × 50 traces each = 5,000 traces for Phase 1
- Scale up for Phase 3 hypothesis testing based on effect sizes from Phase 2

---

## 5. Related Work & Positioning

| Paper | What it shows | What it leaves open |
|-------|--------------|-------------------|
| Inverse Scaling (Gema et al.) | More test-time compute can hurt | *Why* — no mechanistic explanation |
| Hot Mess (McKee et al.) | Errors become incoherent with scale | Focuses on inter-trace; no intra-trace analysis |
| Thought Anchors (Pfau et al.) | Causal attribution within CoT traces | Applied to correctness, not inverse scaling |
| Thought Branches (Pfau et al.) | Branch points in reasoning | Not applied to failure mode analysis |
| Emotion Concepts (Trott et al.) | LLMs have emotion-like representations | Not connected to reasoning quality |
| Faithfulness of CoT (Lanham et al.) | CoT doesn't always reflect model's actual reasoning | Complements our work — unfaithful CoT could drive incoherence |

**Our contribution:** First systematic study of *intra-trace* causal dynamics in inverse scaling settings, connecting mechanistic interpretability tools to a safety-relevant behavioral phenomenon.

---

## 6. Timeline (Rough)

| Phase | Duration | Milestone |
|-------|----------|-----------|
| **Phase 1** | 4-6 weeks | Intra-trace causal maps for selected tasks |
| **Phase 2** | 3-4 weeks | Pattern characterization, feature analysis |
| **Phase 3** | 4-6 weeks | Hypothesis testing, mechanistic evidence |
| **Phase 4** | 3-4 weeks | Intervention experiments |
| **Writing** | 2-3 weeks | Paper draft |

Total: ~4-5 months for a full paper.

---

## 7. Risks & Mitigations

- **Thought Anchors/Branches tools may not scale to long traces.** Mitigation: start with shorter traces where inverse scaling is still visible; optimize tools or use approximations for longer traces.
- **Inverse scaling effect may be weak for open-weight models.** Mitigation: validate effect exists in our models before committing to full analysis; supplement with API model behavioral analysis.
- **No clear mechanistic pattern emerges.** Mitigation: Phase 2 is designed as exploratory — even a null result (no single mechanism dominates) is publishable if well-characterized.
- **Emotion concepts may be a red herring.** Mitigation: it's one of several Phase 2 lenses, not the sole bet. We follow the data.

---

## 8. Open Questions for Discussion

1. Should we focus on a single model family for depth or sweep across families for breadth?
2. Which specific Gema et al. tasks show the strongest inverse scaling — need to reproduce their results first
3. How do we handle the faithfulness problem — if CoT doesn't reflect true computation, are we analyzing the right thing?
4. Is there a connection between incoherence and sycophancy? (Both increase with scale)
5. Should we try to get access to reasoning traces from Claude or o1-style models, or stick to open-weight?
