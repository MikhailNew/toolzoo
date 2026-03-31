# ADR-0001: Browser-First Web App with Local Daemon

**Status**: Accepted
**Date**: 2026-03-31

## Context

Toolzoo needs to manage local repositories and local CLI tools while staying easy to ship as an open-source MVP. A desktop wrapper is attractive, but it increases packaging, native runtime, and platform complexity before the core workflow is validated.

## Use Cases

- UC1: Run the product locally without introducing a hosted backend.
- UC2: Launch and monitor multiple CLI sessions from one interface.
- UC3: Keep the product architecture compatible with a future desktop shell.

## Options Considered

### Option 1: Browser-first web app plus local daemon

**Pros**:
- Fastest path to MVP
- Clean separation between UI and runtime
- Easy to keep packaging-agnostic for a future desktop wrapper

**Cons**:
- Requires explicit reconnect handling between browser and daemon
- Native window affordances are deferred

### Option 2: Tauri-first desktop app

**Pros**:
- Native shell and windowing from day one
- Stronger permission model than a typical desktop shell

**Cons**:
- Extra complexity before validating core runtime behavior
- PTY runtime still needs careful integration

### Option 3: Electron-first desktop app

**Pros**:
- Mature ecosystem for terminal-style apps
- Large amount of community examples

**Cons**:
- Heavier runtime
- Security hardening surface is larger

## Decision

We chose **Option 1**. Toolzoo will ship its MVP as a browser-first web app backed by a local daemon, and desktop wrappers will be considered later if the core workflow proves valuable.

## Consequences

- The UI and runtime can evolve independently.
- Session persistence and reconnect semantics must be defined clearly.
- A future Tauri wrapper can reuse the same web app and daemon boundaries.

