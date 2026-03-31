# Contributing

## Branch Workflow

- Do not commit directly to `main`.
- Do not push directly to `main`.
- Create a feature branch and merge through a pull request.

## Local Guardrails

This repository includes versioned git hooks in [`.githooks/`](./.githooks/).

Run this once after cloning:

```sh
./scripts/install-git-hooks.sh
```

The hooks block:

- commits made while your current branch is `main`
- pushes that update remote `main`

Emergency local bypasses exist via `ALLOW_MAIN_COMMIT=1` and `ALLOW_MAIN_PUSH=1`, but the primary protection should live in GitHub repository rules.

## GitHub Repository Protection

For a public open source repository, protect `main` in GitHub with a ruleset or branch protection rule:

- require a pull request before merging
- require at least one approval
- require status checks before merging
- require conversation resolution before merging
- block force pushes
- block branch deletion
- do not allow bypassing the rule
- apply the rule to administrators as well
