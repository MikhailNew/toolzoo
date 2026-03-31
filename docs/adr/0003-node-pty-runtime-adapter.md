# ADR-0003: `node-pty` as the Initial Session Runtime Adapter

**Status**: Accepted
**Date**: 2026-03-31

## Context

Toolzoo needs a practical way to spawn and control local terminal sessions from a local daemon. The first runtime should support interactive CLI tools, terminal resizing, stdin writes, and output streaming without locking the product into a single long-term implementation.

## Use Cases

- UC1: Launch a CLI tool in a project working directory.
- UC2: Stream output to a browser terminal.
- UC3: Reattach the browser UI to a live local process owned by the daemon.

## Options Considered

### Option 1: `node-pty`

**Pros**:
- Purpose-built for PTY-backed terminal sessions
- Good fit for a Node.js daemon
- Supports interactive tool workflows well

**Cons**:
- Child processes inherit host-level permissions
- Does not solve cross-reboot persistence by itself

### Option 2: `tmux` as the initial runtime

**Pros**:
- Better foundation for long-lived sessions
- Stronger story for reattachment and process continuity

**Cons**:
- More operational complexity for MVP
- Harder to keep the first implementation focused

### Option 3: Plain `child_process` without PTY

**Pros**:
- Simpler process API

**Cons**:
- Poor fit for interactive CLI tools
- Weak terminal fidelity

## Decision

We chose **Option 1**. Toolzoo will start with `node-pty` behind a `PtyRuntimePort`, keeping the door open for a future `tmux`-backed adapter if stronger session durability is required later.

## Consequences

- Runtime logic must stay abstracted behind ports.
- Security guidance must clearly state that host-mode sessions run with host user privileges.
- Future work can introduce a different runtime without rewriting application use-cases.

