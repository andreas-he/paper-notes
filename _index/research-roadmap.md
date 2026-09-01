---
title: Research Roadmap
draft: true
---

# Research Roadmap

A living synthesis across paper notes — questions being chased, directions worth exploring, and gaps in the literature.

## Active Research Questions

- **True elimination vs. suppression under training-time interventions.** When a training-time intervention (e.g. inoculation prompting) reduces an undesired test-time behavior, is the underlying capability genuinely removed or merely suppressed conditional on prompt format? Probe-level and OOD-elicitation tests distinguish these but are rarely run. ([[inoculation-prompting-wichers]])
- **Curative variants of preventive interventions.** Most training-time defenses prevent a misalignment from forming. Can the same intervention reverse it after the fact, and on what timescale and data ratio? ([[inoculation-prompting-wichers]])
- **Does the inverse-effort intuition generalise beyond test-time compute?** [[inverse-scaling-test-time-compute]] shows added deliberation degrading model performance; [[half-assing-it-with-everything-youve-got]] makes the analogous argument for human effort. Whether that is a shared structure (optimising a proxy past the point it tracks the objective) or two unrelated phenomena sharing a shape is unresolved — and only the ML half is empirical.
- **Pre-training-time signals as predictors of post-training-time generalization.** The pre-fine-tune elicitation heuristic in [[inoculation-prompting-wichers]] is one instance of a broader pattern — cheap base-model measurements predicting expensive fine-tune outcomes. Worth tracking across papers.

## Promising Directions

- **Combining prompt-level and data-level interventions.** Training-time prompt rewrites are data-cheap; data-level interventions are mechanism-richer. The composition is underexplored.
- **Mechanistic readouts of behavioral training-time interventions.** Most papers report behavioral metrics; activation-patching or SAE-feature analyses on the same model organisms would test the gradient-attribution story directly.

## Key Paper Connections

- [[inoculation-prompting-wichers]] → Tan et al. 2025 (`2510.04340`) via the same training-time-prompt-intervention mechanism, applied at different scopes (SFT prompt vs. system prompt).
- [[inverse-scaling-test-time-compute]] → [[half-assing-it-with-everything-youve-got]] via non-monotonicity in effort/compute: both undercut the assumption that additional optimisation pressure is weakly beneficial. Flagged as analogy, not mechanism.

## Open Gaps

- No probe-level or activation-patching study published yet that distinguishes "inoculated but capability-preserved" from "inoculated and capability-eliminated" models.
- Composition of inoculation prompts when training data contains multiple undesired behaviors is untested.
- Transfer of training-time prompt interventions to RL-style training (where "prompt" framing is less natural) is open.
- No treatment anywhere in the notes of how to set a quality target when the acceptance bar is unpublished — the case that covers most research work and most selection processes. ([[half-assing-it-with-everything-youve-got]])

## Prioritised Reading Backlog

- Tan et al. 2025 — *Inoculation Prompting* system-prompt variant (`2510.04340`). Direct companion to [[inoculation-prompting-wichers]].
- Betley et al. 2025 — *Emergent Misalignment* (`2502.17424`). The canonical phenomenon the inoculation thread defends against.
- Wang et al. 2025 — *Persona Features Control Emergent Misalignment* (`2506.19823`). Mechanistic alternative for measuring "true elimination."
- Kaczér et al. 2025 — *In-Training Defenses against Emergent Misalignment* (`2508.06249`). Baseline bar.
- Anthropic 2025 — *Natural Emergent Misalignment from Reward Hacking in Production RL* (`2511.18397`). Strongest existing evidence prompt-level interventions scale.
- Soares 2015 — remainder of the *Replacing Guilt* series (mindingourway.com). [[half-assing-it-with-everything-youve-got]] defers its treatment of guilt-over-wasted-motion to later posts; the substitute-motivator argument is only stated there.
