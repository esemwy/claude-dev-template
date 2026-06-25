# Claude Dev Template

A starting point for new projects that use Claude Code. It bundles three custom skills and a `CLAUDE.md` that teaches Claude how to work with your issue tracker.

## What's included

### `CLAUDE.md`

Project-level instructions for Claude. It teaches Claude to:

- Detect whether the project uses GitLab, GitHub, or no remote, and use the right CLI (`glab` / `gh` / local `issues/` files) automatically.
- Find the PRD at `PRD.md` in the repo root.

### Skills

Skills are slash commands available in any Claude Code session inside the project.

| Skill | Command | What it does |
|-------|---------|--------------|
| **Write a PRD** | `/write-a-prd` | Interviews you, explores the codebase, and writes a structured `PRD.md`. |
| **PRD to Issues** | `/prd-to-issues` | Reads `PRD.md` and breaks it into vertical-slice issues, creating them in GitLab, GitHub, or local `issues/` files. |
| **Grill Me** | `/grill-me` | Stress-tests a plan by asking probing questions one at a time until you reach a shared understanding. |

## Requirements

- [Claude Code](https://claude.ai/code) (CLI or IDE extension)
- **GitLab projects**: [`glab`](https://gitlab.com/gitlab-org/cli) authenticated to your instance
- **GitHub projects**: [`gh`](https://cli.github.com/) authenticated to GitHub
- **No remote**: no extra tools needed — issues are written to `issues/` as markdown files

## Usage

1. Use this repo as a template (or copy `.claude/` and `CLAUDE.md` into an existing project).
2. Open the project in Claude Code.
3. Run `/write-a-prd` to capture requirements, then `/prd-to-issues` to turn them into tracked work.

Skills are loaded automatically when Claude Code starts in this directory — no configuration needed.
