# Toogly Workflow Guide

Internal reference for team members and AI agents.

## Repositories

| Repo | Description | Visibility |
|------|-------------|------------|
| `toogly/backend` | Go API server | Private |
| `toogly/tui` | Terminal UI (Go / Bubble Tea) | Private (will be open-sourced) |
| `toogly/desktop` | Desktop app (Tauri + Svelte) | Private |
| `toogly/docs` | Product docs, roadmap, architecture | Private |
| `toogly/.github` | Org profile, labels, templates, workflows | Public |

## Branches

Format: `<type>/<issue-number>-<short-description>`

| Prefix | Use |
|--------|-----|
| `feature/` | New functionality |
| `fix/` | Bug fix |
| `chore/` | Dependencies, CI, configs |
| `docs/` | Documentation only |
| `refactor/` | Code improvement, no behavior change |

Rules:
- Lowercase only, hyphens between words
- Always include issue number if one exists
- Description under 50 characters
- Base branch: `main`

Examples:
```
feature/42-keyboard-navigation
fix/87-crash-empty-project-list
chore/15-update-dependencies
```

## Commits

[Conventional Commits](https://www.conventionalcommits.org/) format:

```
feat: add timer pause/resume keyboard shortcut
fix: handle empty project list without crash
chore: update Bubble Tea to v2.1.0
docs: add API authentication guide
refactor: extract timer model into separate package
test: add handler tests for task endpoints
ci: add label sync workflow
```

- Lowercase prefix, no period at end
- Imperative mood: "add" not "added"
- Subject line under 72 characters
- Body for context when needed, separated by blank line

## Issues

### Naming

Action-oriented titles in English:
- "Add keyboard shortcuts for timer controls"
- "Fix crash when project list is empty"

Do NOT use:
- `[Category]` prefixes — the repo already identifies the area
- Vague titles like "Timer API" — describe what needs to happen

### Templates

All repos inherit issue templates from `toogly/.github`:
- **Bug Report** (`01-bug.yml`) — auto-labels `type: bug`
- **Feature Request** (`02-feature.yml`) — auto-labels `type: feature`
- **Task** (`03-task.yml`) — auto-labels `type: chore`

Blank issues are disabled. Use templates or Discussions.

### Sub-issues

Use sub-issues for:
- Cross-repo dependencies
- Tasks assigned to different people
- Tasks needing their own labels and discussion

Use simple checklists (`- [ ]`) for personal to-dos within a single issue.

## Pull Requests

### PR Template

All repos inherit the PR template from `toogly/.github/PULL_REQUEST_TEMPLATE.md`.

### Flow

1. Create branch from `main`
2. Push and open PR
3. CI must pass (test, lint, vulncheck)
4. Squash merge into `main`
5. Delete branch after merge

### Linking Issues

Use keywords in PR description to auto-close issues:
```
Fixes #42
Closes toogly/backend#15
```

## Labels

Unified across all repos via `toogly/.github/labels.yml`, synced weekly by `EndBug/label-sync`.

### Type (what kind of work)

| Label | Color |
|-------|-------|
| `type: bug` | `#d73a4a` |
| `type: feature` | `#0075ca` |
| `type: chore` | `#006b75` |
| `type: docs` | `#0052cc` |

### Priority

| Label | Color |
|-------|-------|
| `priority: critical` | `#b60205` |
| `priority: high` | `#d93f0b` |
| `priority: low` | `#0e8a16` |

No `priority: medium` — absence of a priority label means medium.

### Status

| Label | Color |
|-------|-------|
| `status: blocked` | `#fbca04` |
| `status: needs-info` | `#fef2c0` |

### Closure

| Label | Color |
|-------|-------|
| `duplicate` | `#cfd3d7` |
| `wontfix` | `#eeeeee` |

### Open Source

| Label | Color |
|-------|-------|
| `good first issue` | `#7057ff` |
| `help wanted` | `#008672` |

### Security

| Label | Color |
|-------|-------|
| `security` | `#e11d48` |

## Project Board

Single org-level project: [Tasks](https://github.com/orgs/toogly/projects/1)

### Custom Fields

| Field | Type | Values |
|-------|------|--------|
| Status | Single Select | Backlog, Todo, In Progress, In Review, Done |
| Phase | Single Select | Phase 0: Foundation, Phase 1: Web + Launch, Phase 2: Public Launch, Phase 3: Desktop, Phase 4: Mobile |
| Priority | Single Select | High, Medium, Low |
| Effort | Number | Fibonacci: 1, 2, 3, 5, 8 |
| Start Date | Date | — |
| Target Date | Date | — |

### Views

| View | Layout | Filter | Purpose |
|------|--------|--------|---------|
| Kanban | Board | `-status:Done -status:Backlog` | Daily work |
| Backlog | Table | `-status:Done` | Triage and planning, grouped by Priority |
| Roadmap | Roadmap | — | Timeline by Phase |
| My Work | Board | `assignee:@me -status:Done -status:Backlog` | Personal focus |
| Needs Triage | Table | `no:priority status:Backlog` | Unprocessed issues |

### Filter Syntax

```
repo:toogly/backend           # By repository
assignee:@me                  # My tasks
status:"In Progress"          # By status
-status:Done                  # Exclude Done
no:assignee                   # Unassigned
no:priority                   # No priority set
phase:"Phase 1: Web + Launch" # By phase
```

### Automations (Built-in Workflows)

| Trigger | Action |
|---------|--------|
| Item added to project | Status → Todo |
| Item closed | Status → Done |
| Item reopened | Status → Todo |
| PR linked to issue | Status → In Progress |
| PR merged | Status → Done |
| Code review approved | Status → In Review |
| Code changes requested | Status → In Progress |
| Auto-add sub-issues | Enabled |
| Auto-add to project | Enabled for all repos |
| Auto-archive | Enabled |

### No Milestones

Milestones are not used. Use the **Phase** field on the project board instead — it works cross-repo.

## CI

All Go repos (backend, tui) run on PR and push to main:
- `go vet ./...`
- `go test -race ./...`
- `golangci-lint`
- `govulncheck`

## Pre-commit Checklist (Go)

```bash
go build ./...
go vet ./...
golangci-lint run ./...
go test -race ./...
```
