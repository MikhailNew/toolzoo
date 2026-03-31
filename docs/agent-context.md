# Toolzoo Agent Context

**Status**: Draft  
**Date**: 2026-03-31

This file exists to preserve implementation context for AI coding agents working on Toolzoo.

## Product Invariants

- Toolzoo is a control plane for local CLI tools, not a replacement model runtime.
- MVP is browser-first. Do not introduce a desktop-first architecture without an ADR.
- The daemon is the source of truth for live terminal sessions.
- Browser refresh must reconnect to the same live `sessionId` while the daemon remains alive.
- Workspace layout, project attachments, open tabs, and session metadata must persist locally.
- GitHub Issues are GitHub-only in MVP and must go through `gh`, not through embedded OAuth.
- No app-managed model API keys should be stored.

## Implementation Guardrails

- Keep runtime logic behind ports so the PTY backend can evolve later.
- Do not bind the daemon to a public interface.
- Do not couple frontend code to infrastructure modules.
- Do not promise cross-reboot terminal resume unless the runtime truly supports it.
- Treat `needs_attention` as heuristic unless a tool-specific adapter can prove stronger semantics.
- Add or update ADRs when changing runtime model, security boundaries, or supported integrations.

## Documentation Guardrails

- Keep `docs/toolzoo/README.md` updated as the entry point.
- Update the relevant ADR when a previously accepted decision changes.
- Keep `backlog.md` aligned with implemented slices and released scope.
- Document any new tool adapter before or together with implementation.

