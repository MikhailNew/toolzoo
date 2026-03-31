# Toolzoo Planning Docs

**Status**: Draft  
**Last Updated**: 2026-03-31

Toolzoo is a browser-first, local-only control plane for AI coding CLIs. It does not replace Codex, Claude Code, or GitHub CLI. It provides a single interface to organize workspaces, attach local repositories, run multiple CLI sessions per project, and monitor attention states across all active work.

## Current Decisions

- MVP is **browser-first** with a **local daemon**. Desktop wrappers are deferred until the web + daemon architecture is stable.
- The daemon owns terminal sessions. Page refresh reconnects to the same `sessionId` when the daemon is still alive.
- Workspaces, projects, layouts, open tabs, and session metadata are persisted locally.
- True cross-reboot session resume is out of scope for MVP. After daemon restart or OS reboot, sessions are marked terminated and can be reopened.
- GitHub Issues support is limited to **GitHub via `gh` CLI** in MVP.
- No additional app-level authentication is introduced. Toolzoo relies on already-installed CLIs and their existing local auth state.
- Docker is used for development, CI, and a future sandbox mode. It is not the primary runtime model for end users.

## Document Map

- [Product Requirements](./prd.md)
- [Architecture](./architecture.md)
- [Monorepo Structure](./monorepo-structure.md)
- [Use Cases and Ports](./use-cases-and-ports.md)
- [Backlog](./backlog.md)
- [Agent Context](./agent-context.md)
- [ADR Index](./adr/README.md)

## Recommended Reading Order

1. Read [Product Requirements](./prd.md) for scope.
2. Read [Architecture](./architecture.md) for runtime boundaries and persistence semantics.
3. Read [Use Cases and Ports](./use-cases-and-ports.md) before implementing modules.
4. Read [Agent Context](./agent-context.md) before delegating work to AI coding agents.
5. Read [Backlog](./backlog.md) when planning slices and milestones.

