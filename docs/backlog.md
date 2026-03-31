# Toolzoo Backlog

**Status**: Draft  
**Date**: 2026-03-31

## Release Slices

### Slice 0: Foundation

- Create monorepo scaffold with `apps/web`, `apps/daemon`, and shared packages.
- Establish coding conventions, testing layout, and documentation rules.
- Implement ADR workflow and document accepted decisions.
- Add basic CI for lint, typecheck, unit tests, and docs validation.

### Slice 1: Core Workspace Shell

- Create workspace switcher UI.
- Persist workspaces and project attachments in SQLite.
- Support project list, row, and grid layout modes.
- Build project shell with tabbed session chrome.
- Restore workspace layout and open tabs on page load.

### Slice 2: Session Runtime

- Implement tool registry and host discovery.
- Launch PTY sessions in project `cwd`.
- Stream terminal output into `xterm.js`.
- Support tab close, session stop, session restart, and resize.
- Reconnect to active sessions on browser refresh by `sessionId`.

### Slice 3: Dashboard and Attention

- Append structured runtime events for session lifecycle transitions.
- Build dashboard feed across all workspaces.
- Add workspace badges for `needs_attention`.
- Add local notifications for `needs_attention` and `finished`.
- Add heuristic classifier for attention signals.

### Slice 4: GitHub Issues

- Resolve project GitHub context from local repo metadata.
- List issues via `gh`.
- Create issues from a simple local form via `gh`.
- Handle missing `gh` or unauthenticated state with clear recovery guidance.

### Slice 5: Hardening and OSS Release

- Add secret redaction for persisted logs and snapshots.
- Add crash handling and clearer terminated-session recovery states.
- Write installation, setup, troubleshooting, and contribution docs.
- Add Docker-based development environment and CI helpers.
- Prepare first public beta release checklist.

## P0 Stories

- As a user, I can create a workspace and attach multiple local projects.
- As a user, I can open multiple CLI tabs inside one project.
- As a user, refreshing the browser preserves my layout and reconnects to live sessions.
- As a user, I can see which workspace or session needs my attention next.

## P1 Stories

- As a user, I can receive a local notification when a session needs input.
- As a user, I can reopen a terminated session with the same tool and project context.
- As a user, I can create a GitHub issue without leaving Toolzoo.

## P2 Stories

- As a user, I can switch to a desktop wrapper without changing the product model.
- As a user, I can run sessions inside a future sandbox mode for untrusted repositories.
- As a user, I can use stronger runtime persistence through a future `tmux` adapter.

## Future Work

- Tauri wrapper
- `tmux` adapter
- Pull request view
- Review workflows
- Issue tracker adapters for Linear and Jira
- Multi-user or remote daemon mode

