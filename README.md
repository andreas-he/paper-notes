# Paper Notes & Knowledge Base

A growing collection of paper reading notes, research project plans, and code
experiments — published as a [Quartz](https://quartz.jzhao.xyz/) blog and
editable in Obsidian.

For agents working in this repo, see [`CLAUDE.md`](CLAUDE.md) — it defines the
directory layout, frontmatter schema, wikilink rules, privacy constraints, and
status workflow.

## Structure

```
paper-notes/
├── CLAUDE.md              # Agent guide: conventions + contracts
├── README.md              # This file
├── index.md               # Blog home page
│
├── _templates/            # Note templates
│   ├── paper-note.md
│   ├── research-project.md
│   └── topic-note.md
│
├── <paper-slug>.md        # Individual paper notes (flat at root)
│
├── research/              # Research project plans + code plans
│   └── <project-slug>.md
│
├── topics/                # Cross-cutting topic threads
│   └── <topic-slug>.md
│
├── code/                  # Experiment notebooks & scripts
│   └── <slug>/
│
└── assets/                # Figures, diagrams, exported plots
    └── <slug>/
```

## Conventions

- **File names:** `kebab-case.md` matching a short slug for the paper or project
- **Links:** Use wikilinks `[[slug]]` — slug only, never full paths
- **Tags:** Use consistent tags in frontmatter for filtering
- **Status:** `reading` → `discussed` → `reviewed` → `implemented`
- **Math:** Use `$inline$` and `$$block$$` LaTeX (Quartz + Obsidian compatible)
- **Code:** Experiments live in `code/<slug>/` with a `README.md`

## Content Sources

1. **Reading sessions** — paper discussions become structured notes
2. **Notion read/watch list** — existing reading database, synced periodically
3. **Code experiments** — implementations, reproductions, explorations

## Roadmap

- [x] Content structure and templates
- [x] GitHub repo (`andreas-he/paper-notes`)
- [x] Agent metadata (`CLAUDE.md`)
- [ ] Quartz setup + GitHub Pages deployment
- [ ] Notion reading list sync
- [ ] Obsidian as local editor
- [ ] Custom domain
