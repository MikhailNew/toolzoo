# Toolzoo Architecture

**Status**: Draft  
**Date**: 2026-03-31

## Architectural Principles

- Local-first: no hosted control plane is required for core usage.
- Browser-first: UI is delivered as a web app, not as a desktop shell in MVP.
- Clear boundaries: frontend UI, local daemon, and runtime adapters are separate concerns.
- Port-driven runtime: terminal, notifications, issue tracking, and persistence are abstracted behind ports.
- Honest persistence semantics: reconnect to live sessions while the daemon is alive; do not fake full process resume after reboot.
- Host auth reuse: Toolzoo leverages existing CLI authentication rather than introducing app-level auth.

## High-Level System

### Web App

- React + Vite application.
- `shadcn/ui` for UI primitives.
- `xterm.js` for terminal rendering.
- React Query for daemon-backed server state.
- Zustand for client-only UI state such as selected workspace, layout mode, and filters.

### Local Daemon

- Node.js + TypeScript service running on the host machine.
- Owns PTY session lifecycle and reconnection.
- Exposes HTTP endpoints for commands and WebSocket streams for terminal output and runtime events.
- Persists metadata and dashboard feed to SQLite.

### Local Runtime Dependencies

- Installed CLI tools such as Codex CLI, Claude Code, `gh`, and generic shell commands.
- Local filesystem access limited to user-selected project roots.
- OS notification APIs via a host adapter.

## Core Runtime Model

1. The user opens the web app.
2. The web app loads persisted workspaces, projects, tabs, and settings from the daemon.
3. When a user opens or restores a session tab, the UI requests attachment by `sessionId`.
4. If the daemon still owns a live PTY for that `sessionId`, the UI reconnects via WebSocket and continues streaming output.
5. If the session no longer exists, the UI shows the session as terminated and offers reopen or cleanup actions.

## Session Persistence Semantics

### Persisted

- Workspaces
- Projects attached to each workspace
- Layout mode per workspace
- Open tabs per project
- Session metadata such as tool, label, cwd, timestamps, and last known state
- Recent event feed and optionally terminal scrollback snapshots

### Not Guaranteed in MVP

- Live process survival after daemon crash
- True session checkpoint/resume after machine reboot
- Reattaching to sessions if the underlying CLI process has exited

### Why This Model

This preserves the user experience that matters most for MVP: refreshing the page or reopening the browser does not destroy the active control surface. At the same time, it avoids pretending that PTY sessions are durable across host restarts without introducing a more complex runtime such as `tmux`.

## State Model

Each session should expose one of the following runtime states:

- `starting`
- `running`
- `idle`
- `needs_attention`
- `exited`
- `failed`

`needs_attention` is heuristic in MVP. It should be derived from configurable tool markers, inactivity windows, and explicit user override where necessary.

## Security Model

- The daemon binds only to localhost or a local socket.
- The daemon accepts only UI requests from the local app origin and uses a local session token for mutation calls.
- Sessions launch only inside registered project roots.
- Environment propagation is allowlisted, not fully inherited by default.
- Stored logs pass through basic secret redaction before persistence.
- Toolzoo never stores model API keys or duplicates external auth tokens.
- Host-mode runtime is considered trusted-user mode. A future sandbox mode can isolate untrusted repositories.

## Deployment Model

### MVP

- `apps/daemon` runs locally on the host.
- `apps/web` runs locally in the browser and talks to the daemon.
- Development uses Docker for tooling support only, not as the main end-user runtime.

### Later

- A Tauri wrapper may embed the same web app and launch the daemon as a managed local process.
- A sandbox runner may execute sessions inside containers for stricter isolation.

