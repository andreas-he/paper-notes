# Paper Notes & Knowledge Base

A growing collection of paper reading notes, discussions, and connections — eventually published as a [Quartz](https://quartz.jzhao.xyz/) blog.

## Structure

```
papers/
├── index.md              # Blog home page
├── _templates/            # Note templates
│   ├── paper-note.md      # For individual paper discussions
│   └── topic-note.md      # For cross-paper topic threads
├── <paper-slug>.md        # Individual paper notes
├── topics/                # Cross-cutting topic threads
│   └── <topic>.md
└── code/                  # Experiment notebooks & scripts
    └── <paper-slug>/
```

## Conventions

- **File names:** `kebab-case.md` matching the paper's short name
- **Links:** Use wikilinks `[[paper-slug]]` to connect papers
- **Tags:** Use consistent tags in frontmatter for filtering
- **Status:** `reading` → `discussed` → `reviewed` → `implemented`
- **Math:** Use `$inline$` and `$$block$$` LaTeX (Quartz + Obsidian compatible)
- **Code:** Linked experiments live in `code/<paper-slug>/` or separate repos

## Content Sources

1. **Reading sessions with the brain** — paper discussions become structured notes
2. **Notion read/watch list** — existing reading database, synced periodically
3. **Code experiments** — implementations, reproductions, explorations

## Roadmap

- [x] Content structure and templates
- [ ] GitHub repo (`andreas-he/paper-notes`)
- [ ] Quartz setup + GitHub Pages deployment
- [ ] Notion reading list sync
- [ ] Obsidian as local editor
- [ ] Custom domain
