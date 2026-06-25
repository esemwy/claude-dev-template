---
name: prd-to-issues
description: Break a PRD into independently-workable issues and create them in the project's issue tracker (GitLab via glab, GitHub via gh, or none). Use when the user wants to turn a PRD into a list of concrete tasks.
---

# PRD to Issues

Break a PRD into independently-grabbable issues as vertical slices (tracer bullets) and create them in the detected issue tracker.

## Process

### 1. Detect the hosting provider

Run:

```sh
git rev-parse --is-inside-work-tree 2>/dev/null && git remote get-url origin 2>/dev/null
```

Classify the result:

| Condition | Provider | CLI |
|-----------|----------|-----|
| URL contains `gitlab` | GitLab | `glab` |
| URL contains `github.com` | GitHub | `gh` |
| Not a git repo, or no remote, or unrecognised host | None | — |

If the provider is **None**, issues will be written as markdown files under `issues/` in the project root (creating the directory if needed).

### 2. Verify tracker access

- **GitLab**: run `glab repo view`. If it fails, offer `glab repo create` and retry.
- **GitHub**: run `gh repo view`. If it fails, offer `gh repo create` and retry.
- **None**: skip this step.

### 3. Locate the PRD

Read `PRD.md` from the repo root. If it doesn't exist, ask the user for the path.

### 4. Explore the codebase (optional)

If you have not already explored the codebase, do so to understand the current state of the code.

### 5. Draft vertical slices

Break the PRD into **tracer bullet** issues. Each issue is a thin vertical slice that cuts through ALL integration layers end-to-end, NOT a horizontal slice of one layer.

Slices may be 'HITL' or 'AFK'. HITL slices require human interaction, such as an architectural decision or a design review. AFK slices can be implemented and merged without human interaction. Prefer AFK over HITL where possible.

<vertical-slice-rules>
- Each slice delivers a narrow but COMPLETE path through every layer (schema, API, UI, tests)
- A completed slice is demoable or verifiable on its own
- Prefer many thin slices over few thick ones
</vertical-slice-rules>

### 6. Quiz the user

Present the proposed breakdown as a numbered list. For each slice, show:

- **Title**: short descriptive name
- **Type**: HITL / AFK
- **Blocked by**: which other slices (if any) must complete first
- **User stories covered**: which user stories from the PRD this addresses

Ask the user:

- Does the granularity feel right? (too coarse / too fine)
- Are the dependency relationships correct?
- Should any slices be merged or split further?
- Are the correct slices marked as HITL and AFK?

Iterate until the user approves the breakdown.

### 7. Create issues

Create issues in dependency order (blockers first) so you can reference real issue numbers in later issues' "Blocked by" sections.

Use the command matching the detected provider:

**GitLab:**
```
glab issue create --title "<title>" --description "<body>" --label "<AFK or HITL>"
```

**GitHub:**
```
gh issue create --title "<title>" --body "<body>" --label "<AFK or HITL>"
```

**None:** write each issue as `issues/<NNN>-<slug>.md` (e.g. `issues/001-user-auth.md`), numbering sequentially from 001. Use the local file number as the reference ID in "Blocked by" fields.

Capture the issue URL/number from each command's output (or the local file number) before moving to the next issue so you can reference it in dependent issues.

Format the description/body using this template:

<issue-template>
## What to build

A concise description of this vertical slice. Describe the end-to-end behavior, not layer-by-layer implementation.

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Blocked by

- #NNN <title> (if any — use the real issue number captured from the CLI output)

Or "None — can start immediately" if no blockers.

## User stories addressed

Reference by number from PRD.md:

- User story 3
- User story 7
</issue-template>

After all issues are created (or written to disk), show a summary table with titles and URLs (or file paths if no tracker).
