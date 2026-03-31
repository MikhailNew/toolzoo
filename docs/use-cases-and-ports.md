# Toolzoo Use Cases and Ports

**Status**: Draft  
**Date**: 2026-03-31

## Core Domain Objects

- `Workspace`: named container of projects and layout preferences
- `Project`: attached local repository or local document root
- `Session`: a live or historical CLI runtime instance within a project
- `ToolDefinition`: metadata and launch contract for a supported CLI tool
- `DashboardEvent`: structured runtime event used for projections and notifications
- `IssueDraft`: normalized issue payload before GitHub adapter execution
- `AppSettings`: notification, terminal, and security preferences

## Use Cases

### Workspace Management

- `CreateWorkspace`
- `RenameWorkspace`
- `DeleteWorkspace`
- `ReorderWorkspaces`
- `SetActiveWorkspace`
- `SetWorkspaceLayout`

### Project Management

- `AttachProjectToWorkspace`
- `DetachProjectFromWorkspace`
- `RenameProjectAlias`
- `ReorderProjectsInWorkspace`
- `RestoreWorkspaceProjects`

### Session Runtime

- `DiscoverAvailableTools`
- `ValidateToolAvailability`
- `StartSession`
- `AttachToSession`
- `WriteToSession`
- `ResizeSession`
- `StopSession`
- `RestartSession`
- `RestoreOpenSessions`
- `PersistSessionSnapshot`

### Dashboard and Attention

- `AppendRuntimeEvent`
- `ListDashboardFeed`
- `ClassifySessionAttention`
- `MarkSessionAttentionResolved`
- `CountWorkspaceBadges`
- `PublishLocalNotification`

### GitHub Issues

- `ListGitHubIssues`
- `CreateGitHubIssue`
- `OpenProjectGitHubContext`

### Settings and Policy

- `LoadSettings`
- `UpdateSettings`
- `UpdateNotificationPreferences`
- `UpdateTerminalPreferences`
- `UpdateSecurityMode`

## Domain Ports

### Persistence Ports

- `WorkspaceRepository`
  - Save and load workspaces, layout mode, ordering, and active workspace.
- `ProjectRepository`
  - Save and load project attachments and aliases within workspaces.
- `SessionRepository`
  - Save and load session metadata, open tabs, and reconnect candidates.
- `EventRepository`
  - Append and query structured dashboard events.
- `SettingsRepository`
  - Persist user preferences and policy flags.

### Runtime Ports

- `ToolCatalogPort`
  - Discover supported tools, check versions, and report availability.
- `PtyRuntimePort`
  - Spawn, attach, write to, resize, and terminate PTY-backed sessions.
- `OutputSnapshotPort`
  - Persist and restore terminal scrollback snapshots.
- `NotificationPort`
  - Deliver local notifications for attention and completion events.

### Integration Ports

- `IssueTrackerPort`
  - List and create issues for the current project context.
- `ProjectMetadataPort`
  - Resolve repository-level metadata such as remote URL or default branch.

### Cross-Cutting Ports

- `ClockPort`
  - Provide deterministic timestamps.
- `IdGeneratorPort`
  - Create stable IDs for workspaces, projects, sessions, and events.
- `SecretRedactorPort`
  - Sanitize persisted output before writing logs or snapshots.
- `FilesystemPolicyPort`
  - Validate path scope and project root access.

## Priority Adapters

- SQLite implementation for repositories
- `node-pty` implementation for `PtyRuntimePort`
- `gh` implementation for `IssueTrackerPort`
- Local OS notifications adapter for `NotificationPort`
- Host PATH scanning adapter for `ToolCatalogPort`

## Session Lifecycle Notes

- A session receives a stable `sessionId` at creation time.
- The daemon keeps an in-memory registry of active PTY processes keyed by `sessionId`.
- The UI stores the open tab list and reconnects by `sessionId`.
- If reconnect fails because the daemon no longer owns the live PTY, the session becomes historical and can be reopened as a new runtime instance.

