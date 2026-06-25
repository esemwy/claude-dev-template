# Project conventions

## Issue tracking

Detect the hosting provider before running any issue commands:

```sh
git rev-parse --is-inside-work-tree 2>/dev/null && git remote get-url origin 2>/dev/null
```

- URL contains `gitlab` → use `glab`
- URL contains `github.com` → use `gh`
- No remote or unrecognised host → no issue tracker available

| Task | GitLab (`glab`) | GitHub (`gh`) |
|------|-----------------|---------------|
| List open issues | `glab issue list` | `gh issue list` |
| View an issue | `glab issue view <number>` | `gh issue view <number>` |
| Search issues | `glab issue list --search "<query>"` | `gh issue list --search "<query>"` |
| List closed issues | `glab issue list --state closed` | `gh issue list --state closed` |
| Create an issue | `glab issue create --title "…" --description "…"` | `gh issue create --title "…" --body "…"` |

If there is no remote or the project is not a git repo, issues are stored as markdown files under `issues/` in the project root.

## PRD

The product requirements document lives at `PRD.md` in the repo root.
