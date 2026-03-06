# Contributing to Toogly

Thanks for your interest in contributing!

## Getting Started

1. Fork the repository
2. Create a branch: `<type>/<issue-number>-<short-description>`
   - Types: `feature/`, `fix/`, `chore/`, `docs/`, `refactor/`
   - Example: `feature/42-keyboard-navigation`
3. Make your changes
4. Run tests: `go test -race ./...`
5. Run linting: `go vet ./...`
6. Open a Pull Request

## Branch Naming

```
feature/42-keyboard-navigation
fix/87-crash-empty-project-list
chore/15-update-dependencies
docs/23-api-reference
refactor/31-simplify-timer-model
```

## Commit Messages

We use [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add timer pause/resume keyboard shortcut
fix: handle empty project list without crash
chore: update Bubble Tea to v2.1.0
docs: add API authentication guide
refactor: extract timer model into separate package
```

## Code Style

- Follow standard Go conventions
- Run `go vet` and `staticcheck` before submitting
- Wrap errors with context: `fmt.Errorf("operation: %w", err)`

## Reporting Bugs

Use the [Bug Report](../../issues/new?template=01-bug.yml) issue template.

## Suggesting Features

Use the [Feature Request](../../issues/new?template=02-feature.yml) issue template.
