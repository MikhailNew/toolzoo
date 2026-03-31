# ADR-0002: Session Persistence and Reconnect Semantics

**Status**: Accepted
**Date**: 2026-03-31

## Context

The user experience must remain stable when the browser refreshes or closes temporarily. At the same time, PTY sessions are not trivially checkpointed across daemon restart or machine reboot without introducing a more complex runtime.

## Use Cases

- UC1: Refresh the browser without losing the current workspace and open tabs.
- UC2: Reconnect to live sessions after reopening the browser while the daemon is still alive.
- UC3: Avoid false claims about true process resume after reboot.

## Options Considered

### Option 1: Reconnect to live sessions while daemon is alive

**Pros**:
- Solves the main operator workflow issue
- Simple and reliable enough for MVP
- Compatible with stable `sessionId` semantics

**Cons**:
- Does not preserve live processes after daemon crash or reboot

### Option 2: Full process checkpoint/resume

**Pros**:
- Strongest continuity story
- Survives broader failure conditions in theory

**Cons**:
- Too complex for MVP
- Depends on runtime technology not yet chosen

### Option 3: No reconnect, restore layout only

**Pros**:
- Very simple implementation

**Cons**:
- Breaks the primary value proposition
- Forces users to relaunch tools after every refresh

## Decision

We chose **Option 1**. Toolzoo persists workspaces, projects, tabs, and session metadata locally, and reconnects the UI to live sessions by `sessionId` while the daemon is alive.

## Consequences

- The daemon must own an in-memory session registry keyed by `sessionId`.
- SQLite stores reconnect metadata and last known state.
- Sessions become historical after daemon restart or host reboot and can be reopened manually.

