# Toolzoo PRD

**Status**: Draft  
**Date**: 2026-03-31

## Product Summary

Toolzoo is a local-first workspace manager for AI coding CLIs. It gives one user a single browser UI to launch, monitor, and switch between multiple local repositories and multiple CLI sessions per repository without opening several IDE windows or storing model credentials inside the product.

## Problem

Advanced AI coding workflows now span several repositories at once. Existing CLI tools are powerful, but operating them across multiple projects requires a fragmented setup: several IDE windows, several terminal panes, repeated context switching, and poor visibility into which agent is waiting, running, or finished. The result is wasted attention and slow operator response time.

## Goals

- Provide a single local interface for multiple repositories and multiple CLI sessions.
- Preserve workspace layout and open tabs across browser refresh and normal restarts.
- Reconnect the UI to existing live sessions via stable `sessionId` values while the daemon remains alive.
- Surface a real-time dashboard showing session state transitions and attention signals.
- Avoid app-managed secrets and reuse already-installed CLI tooling.
- Keep the architecture open-source friendly, debuggable, and easy to extend.

## Non-Goals

- Building a full IDE or code editor.
- Running proprietary orchestration in the cloud.
- Replacing the CLI UX of Codex, Claude Code, `gh`, or shell tools.
- Supporting every issue tracker in MVP.
- Providing true process checkpoint/resume across machine reboot.

## Primary User

- A solo developer or technical lead working across 2-10 active repositories.
- Uses terminal-based AI coding tools daily.
- Already has local auth configured for some CLIs.
- Wants operational visibility more than a new editing surface.

## Jobs To Be Done

- "When several agents are working in parallel, I want to see which project needs my attention next."
- "When I refresh the browser, I want my workspace and live sessions to reconnect without rebuilding the layout."
- "When I add a project, I want Toolzoo to launch the tool in the correct repo context."
- "When a tool is missing or not authenticated, I want clear install or fix guidance."
- "When I need to file follow-up work, I want to create a GitHub issue without leaving the local workflow."

## MVP Scope

### Functional Requirements

- FR-001: Users can create, rename, reorder, and delete workspaces.
- FR-002: Each workspace can contain multiple attached local projects.
- FR-003: Each project can contain multiple session tabs backed by local CLI tools.
- FR-004: Toolzoo discovers supported CLIs on the host machine and reports availability status.
- FR-005: Users can launch a session in a selected project working directory.
- FR-006: Users can close a session tab without deleting the project.
- FR-007: The daemon persists workspace layout, open projects, open tabs, and session metadata locally.
- FR-008: On browser refresh, the UI reconnects to live sessions by `sessionId` while the daemon is alive.
- FR-009: A dashboard shows a feed of runtime events across all workspaces.
- FR-010: Sessions expose states at least as `starting`, `running`, `idle`, `needs_attention`, `exited`, and `failed`.
- FR-011: Users can receive local notifications for `needs_attention` and `finished` events.
- FR-012: GitHub Issues can be listed and created via the locally authenticated `gh` CLI.

### UX Requirements

- The workspace switcher must support quick navigation similar to browser tabs.
- Projects within a workspace must support list, horizontal, and grid layouts.
- Session tabs must remain readable when several sessions are open in the same project.
- The dashboard must make "needs attention" states visually obvious.

### Operational Requirements

- The daemon must bind only to localhost or a local socket.
- Toolzoo must not store model API keys or replicate external auth flows.
- Session logs stored on disk must support basic secret redaction.

## Success Criteria

- A user can monitor at least four active repositories from one workspace.
- Refreshing the browser reconnects the UI to live sessions without losing layout state.
- A user can identify the next required action from the dashboard without opening each project manually.
- Initial installation and local startup are understandable from documentation alone.

## Future Work

- Desktop wrapper via Tauri.
- Sandbox runner mode for untrusted repositories.
- `tmux`-backed persistent runtime for stronger session continuity.
- Issue tracker adapters for Linear and Jira.
- Pull request review and approval views.

