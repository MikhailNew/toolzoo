# ADR-0004: GitHub Issues via `gh` CLI

**Status**: Accepted
**Date**: 2026-03-31

## Context

Toolzoo wants to support issue creation in MVP without introducing app-managed OAuth, token storage, or a broader integration surface. The user already prefers a workflow that reuses local CLI authentication.

## Use Cases

- UC1: Create a GitHub issue from the current local project context.
- UC2: List project issues using existing local authentication.
- UC3: Keep integration scope small for the first release.

## Options Considered

### Option 1: Use `gh` CLI

**Pros**:
- Reuses existing local auth
- Aligns with Toolzoo's CLI-first philosophy
- Keeps MVP integration surface small

**Cons**:
- Depends on `gh` being installed and authenticated

### Option 2: Native GitHub OAuth inside Toolzoo

**Pros**:
- Full control over API calls
- Fewer external binary dependencies

**Cons**:
- Adds auth flow, token storage, and maintenance burden
- Violates the MVP principle of no app-managed credentials

### Option 3: No issue integration in MVP

**Pros**:
- Minimal implementation scope

**Cons**:
- Misses a workflow the user explicitly values

## Decision

We chose **Option 1**. Toolzoo will list and create GitHub issues through the local `gh` CLI in MVP.

## Consequences

- The tool registry must detect whether `gh` is installed and authenticated.
- Missing or broken `gh` state must result in clear user guidance.
- Issue tracker abstractions for Linear and Jira can remain future work.

