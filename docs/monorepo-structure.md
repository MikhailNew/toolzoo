# Toolzoo Monorepo Structure

**Status**: Draft  
**Date**: 2026-03-31

## Proposed Layout

```text
toolzoo/
  apps/
    web/                     # React UI
    daemon/                  # Local Node.js runtime and API

  packages/
    contracts/               # Shared API DTOs, event schemas, typed messages
    domain/                  # Entities, value objects, ports
    application/             # Use-cases and orchestration
    infrastructure/          # SQLite, PTY, gh, notifications, discovery adapters
    ui/                      # Shared UI kit and layout primitives
    config/                  # Shared config, constants, feature flags
    testing/                 # Factories, mocks, integration helpers

  docs/
    adr/
    architecture.md
    prd.md
    monorepo-structure.md
    use-cases-and-ports.md
    backlog.md
    agent-context.md
```

## Package Responsibilities

### `apps/web`

- Routes and layout composition only.
- Session views, workspace screens, dashboard, settings, issues.
- Reads daemon state via HTTP and WebSocket APIs.

### `apps/daemon`

- Entry point for local API, WebSocket streams, and process lifecycle.
- Wires all runtime dependencies and adapters.
- Applies application use-cases.

### `packages/contracts`

- REST DTOs
- WebSocket message shapes
- Event names and payload schemas
- Shared validation contracts

### `packages/domain`

- Entities such as `Workspace`, `Project`, `Session`, `ToolDefinition`, `IssueDraft`
- Value objects such as `SessionId`, `ProjectPath`, `LayoutMode`
- Ports for persistence, runtime, notifications, and issue tracking

### `packages/application`

- Use-cases such as `StartSession`, `AttachProject`, `ReconnectSession`, `CreateIssue`
- Cross-port orchestration and invariant enforcement

### `packages/infrastructure`

- `node-pty` adapter
- SQLite repositories
- `gh` issue tracker adapter
- Tool discovery and validation
- Local notifications
- Secret redaction

### `packages/ui`

- Shared design system wrappers
- Dashboard widgets
- Project shells
- Session tab chrome

### `packages/config`

- Default tool registry
- Runtime limits
- Feature flags
- UI defaults

### `packages/testing`

- Domain factories
- WebSocket and PTY test doubles
- Integration helpers for daemon and web app

## Import Boundaries

- `apps/web` may import `packages/contracts`, `packages/ui`, and `packages/config`.
- `apps/web` must not import `packages/infrastructure`.
- `apps/daemon` may import `packages/contracts`, `packages/domain`, `packages/application`, `packages/infrastructure`, and `packages/config`.
- `packages/application` imports only `packages/domain`.
- `packages/infrastructure` implements ports from `packages/domain`.
- `packages/contracts` stays framework-agnostic and dependency-light.

## Frontend State Management

- React Query: daemon-backed server state such as workspaces, sessions, dashboard feed, issue lists
- Zustand: UI-only state such as selected workspace, filters, layout mode, and view preferences

## Test Strategy

- Unit tests for domain entities, state classifiers, redactors, and mappers
- Integration tests for daemon use-cases and repository adapters
- Browser integration tests for workspace restore, session reconnect, and dashboard attention flow

