---
title: "Code Plan: Inverse Scaling Intra-Trace Analysis"
tags: [research-project, inverse-scaling, code, implementation]
status: planning
last_updated: 2026-04-07
---

# Code Components for Inverse Scaling Research

> What needs to be built, in dependency order, with tooling recommendations.

---

## Overview

The codebase has five major components:

```
inverse-scaling-research/
├── data/                    # Benchmark tasks and cached traces
├── trace_collection/        # Generate reasoning traces from models
├── causal_analysis/         # Thought Anchors + Thought Branches pipeline
├── feature_extraction/      # Analyze turning points (attention, repr, emotion)
├── interventions/           # Phase 4 mitigation experiments
├── analysis/                # Statistical analysis and visualization
├── configs/                 # Experiment configs (models, tasks, params)
└── notebooks/               # Exploratory analysis and paper figures
```

---

## Component 1: Trace Collection Pipeline

**Purpose:** Generate reasoning traces of varying lengths for tasks that exhibit inverse scaling.

**What to build:**
- Task loader that reads benchmark datasets from Gema et al. (their code/data should be available on GitHub)
- Prompt templates for controlling reasoning length (e.g., "Think step by step", "Think carefully for exactly N steps", no-CoT baseline)
- Model interface supporting both local (HuggingFace/vLLM) and API models (Claude, OpenAI)
- Trace storage format: JSON with full token-level logprobs, attention maps (for local models), and metadata
- Batch runner with checkpointing (traces are expensive)

**Key decisions:**
- **Inference engine:** vLLM for local models (fast batched inference with logprob access)
- **Storage:** Parquet for tabular metadata, JSON for full traces, HDF5 for attention/activation tensors
- **Trace format per sample:**
  ```json
  {
    "task_id": "...",
    "model": "...",
    "prompt": "...",
    "trace_tokens": ["Think", "ing", " about", ...],
    "trace_text": "Thinking about this problem...",
    "final_answer": "B",
    "correct": false,
    "logprobs": [...],
    "temperature": 0.0,
    "max_tokens": 512,
    "metadata": {}
  }
  ```

**Existing code to leverage:**
- Gema et al. likely have benchmark code — check their paper's GitHub repo
- vLLM / HuggingFace Transformers for inference
- lm-eval-harness for standardized benchmark evaluation

**Estimated effort:** ~1 week

---

## Component 2: Thought Anchors Implementation

**Purpose:** Identify which tokens in a trace causally determine the final answer.

**What to build:**
- Port or wrap the Thought Anchors codebase (check Pfau et al.'s GitHub release)
- **Token-level ablation pipeline:** For each token in the trace, replace it (with padding/alternative) and re-run the rest of the trace to see if the answer changes
- **Sentence-level ablation:** Coarser but faster — ablate entire sentences
- **Anchor scoring:** For each token/sentence, compute an "anchor score" = P(answer flips | token ablated)
- **Commitment point detection:** Find the earliest position after which no single-token ablation flips the answer
- Efficient batching — naive token-by-token ablation is O(n²), need KV-cache reuse

**Key decisions:**
- **Ablation strategy:** Token masking vs. counterfactual replacement vs. resampling from the model's own distribution. Start with masking (simplest), validate with resampling.
- **Efficiency:** Use KV-cache prefill to avoid recomputing the prefix for each ablation. For a trace of length N, we need N forward passes but each only computes from the ablation point onward.
- **Granularity:** Start at sentence level (faster iteration), drill into token level for interesting cases.

**Existing code to leverage:**
- Thought Anchors paper's open-source implementation
- TransformerLens for hook-based intervention on HuggingFace models
- nnsight as an alternative for intervention/ablation

**Estimated effort:** ~2 weeks (1 week if existing code is well-packaged)

---

## Component 3: Thought Branches Implementation

**Purpose:** Identify branching points where the trace could have gone differently.

**What to build:**
- Port or wrap the Thought Branches codebase
- **Branch point detection:** At each token position, measure the entropy of the next-token distribution. High entropy = the model could have gone multiple ways.
- **Counterfactual branching:** At detected high-entropy points, sample alternative continuations and check if they lead to different final answers
- **Branch tree construction:** Build a tree of possible reasoning paths from a single prompt, annotated with answer outcomes
- **Divergence analysis:** For each branch point, characterize what the alternatives were (different reasoning strategies, different intermediate conclusions, etc.)

**Key decisions:**
- **Entropy threshold** for branch point detection — needs tuning per model
- **How many alternative continuations** to sample per branch point (start with 10-20)
- **Depth of branching** — full tree exploration is exponential; use pruning (only branch at top-K entropy points)

**Existing code to leverage:**
- Thought Branches paper's open-source implementation
- The trace collection pipeline (Component 1) for generating continuations

**Estimated effort:** ~2 weeks

---

## Component 4: Feature Extraction at Turning Points

**Purpose:** Characterize what's happening at identified anchor/branch points.

### 4a. Content Analysis
- Sentence classifier for turning point content: {factual_recall, logical_step, restatement, hedging, self_correction, emotional_language, distractor_engagement}
- Can use an LLM-as-judge for initial labeling, then train a lightweight classifier
- N-gram and token-level statistics around turning points

### 4b. Attention Pattern Analysis
- Extract attention maps at turning points (requires local model with hook access)
- Measure attention to: (a) original problem tokens, (b) model's own prior reasoning, (c) specific constraint tokens
- Track attention concentration/dispersion metrics over the trace
- "Lost in the middle" analysis: does attention to early problem tokens decay over the trace?

### 4c. Representation Drift Tracking
- At each token position, extract residual stream activations
- Compute cosine similarity between current representation and: (a) initial problem encoding, (b) representation at key earlier positions
- Detect "drift then snap" patterns (gradual drift followed by sudden shift to wrong-answer representation)
- PCA/UMAP visualization of representation trajectories

### 4d. Constraint Tracking
- For tasks with explicit constraints (math, logic), identify constraint tokens in the problem
- Track whether the model's representation maintains information about each constraint over the trace
- Probe classifiers: train a linear probe to predict "is constraint C still being tracked?" at each position

### 4e. Emotion Concept Vectors (Exploratory)
- Implement or adapt emotion concept extraction from Trott et al.
- Extract emotion concept activations at each trace position
- Test whether emotion activations correlate with turning points or predict trace failure

**Existing code to leverage:**
- TransformerLens / nnsight for activation extraction and hooks
- Baukit (David Bau's toolkit) for representation analysis
- Trott et al.'s emotion concept code (if released)
- scikit-learn for probing classifiers

**Estimated effort:** ~3-4 weeks (most variable component — scope depends on Phase 2 findings)

---

## Component 5: Intervention Pipeline

**Purpose:** Test whether identified mechanisms can be mitigated.

**What to build (depends on Phase 3 findings):**
- **Steering vector application:** Apply representation engineering vectors at specific trace positions
- **Attention forcing:** Modify attention patterns to maintain focus on problem tokens
- **Selective regeneration:** At detected branch points, force the model to regenerate and pick the alternative path
- **Adaptive trace length:** Monitor commitment quality metrics in real-time, stop generation when quality degrades
- **A/B evaluation:** Compare intervention vs. baseline on inverse scaling benchmarks

**Existing code to leverage:**
- RepE (representation engineering) codebase
- TransformerLens / nnsight for activation steering
- Component 1's evaluation pipeline for measuring impact

**Estimated effort:** ~2-3 weeks

---

## Component 6: Analysis & Visualization

**Purpose:** Statistical analysis and paper-ready figures.

**What to build:**
- Commitment point distribution plots (Phase 1)
- Anchor score heatmaps overlaid on trace text (Phase 1)
- Branch trees with color-coded outcomes (Phase 1)
- Feature importance analysis for turning point characterization (Phase 2)
- Representation trajectory visualizations (Phase 2)
- Hypothesis test results with confidence intervals (Phase 3)
- Intervention effect size plots (Phase 4)

**Stack:** matplotlib + seaborn for static figures, plotly for interactive exploration, Jupyter notebooks for narrative analysis.

**Estimated effort:** Ongoing, ~1 week dedicated for paper figures

---

## Dependency Graph

```
Component 1 (Trace Collection)
    ├── Component 2 (Thought Anchors)  ─┐
    └── Component 3 (Thought Branches) ─┤
                                         ├── Component 4 (Feature Extraction)
                                         │       └── Component 5 (Interventions)
                                         └── Component 6 (Analysis & Viz)
```

Components 2 and 3 can be developed in parallel after Component 1 is done.
Component 4 depends on outputs from 2 and 3.
Component 5 depends on findings from 4.
Component 6 runs throughout.

---

## Tech Stack Summary

| Layer | Tool | Why |
|-------|------|-----|
| Inference | vLLM | Fast batched inference, logprob access, KV-cache control |
| Model access | HuggingFace Transformers | Weight loading, tokenizers |
| Mechanistic tools | TransformerLens or nnsight | Hook-based activation access, ablation, steering |
| Experiment tracking | Weights & Biases | Track runs, hyperparams, results |
| Data storage | Parquet + HDF5 | Tabular metadata + dense tensors |
| Compute | GPU cluster (A100s preferred) | Ablation is compute-heavy |
| Analysis | pandas + scipy + statsmodels | Statistical testing |
| Visualization | matplotlib + seaborn + plotly | Publication figures + exploration |
| Notebooks | Jupyter | Narrative analysis |
| Config | Hydra or simple YAML | Reproducible experiment configs |

---

## Getting Started: First Week Checklist

1. [ ] Clone and test Thought Anchors repo — does it work out of the box?
2. [ ] Clone and test Thought Branches repo — same check
3. [ ] Find and clone Gema et al.'s benchmark code/data
4. [ ] Pick 3-5 tasks with strongest inverse scaling signal
5. [ ] Set up vLLM with one open-weight model (e.g., Llama 3 8B)
6. [ ] Generate 50 traces per task at 3 reasoning lengths
7. [ ] Run Thought Anchors on a small subset — validate we get meaningful anchor scores
8. [ ] Visualize first results: anchor heatmaps on 10 traces
