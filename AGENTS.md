# AGENTS.md

## Project Snapshot

Toolzoo is a local-first workspace manager for AI coding CLIs. The product is currently documentation-first: this repository mostly contains planning docs and ADRs, not a finished monorepo scaffold.

Do not assume `apps/`, `packages/`, or runtime code already exist. Check the actual tree before proposing or making implementation changes.

## Current Repository Structure

- [`README.md`](./README.md): short project summary.
- [`docs/README.md`](./docs/README.md): entry point for project docs.
- [`docs/prd.md`](./docs/prd.md): product scope, goals, and non-goals.
- [`docs/architecture.md`](./docs/architecture.md): runtime boundaries and persistence model.
- [`docs/use-cases-and-ports.md`](./docs/use-cases-and-ports.md): domain objects, use cases, and ports.
- [`docs/monorepo-structure.md`](./docs/monorepo-structure.md): target repository layout. This is a planned structure, not the current tree.
- [`docs/agent-context.md`](./docs/agent-context.md): implementation guardrails for AI agents.
- [`docs/adr/README.md`](./docs/adr/README.md): accepted architecture decisions.
- [`docs/backlog.md`](./docs/backlog.md): release slices and implementation priorities.
- [`CONTRIBUTING.md`](./CONTRIBUTING.md): branch and pull request workflow.

## Working Rules For Agents

- Start with [`docs/README.md`](./docs/README.md), then read the specific design doc for the area you are changing.
- Treat [`docs/architecture.md`](./docs/architecture.md), [`docs/agent-context.md`](./docs/agent-context.md), and the ADRs as the main source of truth.
- If code or scaffolding changes the documented architecture, update the relevant docs in the same change.
- Keep MVP constraints intact unless the task explicitly changes them and the documentation is updated.

## Product Invariants

- Browser-first UI with a local daemon in MVP.
- The daemon is the source of truth for live sessions.
- Browser refresh should reconnect to the same live `sessionId` while the daemon is alive.
- GitHub Issues support is GitHub-only in MVP and goes through `gh`.
- Do not introduce app-managed model credentials.

## Scaffolding Guidance

- When implementation starts, follow the boundaries in [`docs/monorepo-structure.md`](./docs/monorepo-structure.md).
- Keep frontend code decoupled from infrastructure modules.
- Add or update an ADR when changing runtime model, security boundaries, or supported integrations.
- Use feature branches and pull requests. Do not commit or push directly to `main`.
