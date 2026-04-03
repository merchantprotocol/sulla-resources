# Approval Helper Skill

**Skill Slug:** approval-helper  
**Purpose:** Detect Docker/n8n/systemd actions and require explicit user confirmation before executing sensitive operations.  
**Created:** 2026-03-26 (P0 from Daily Learning #109)

## Overview

This skill ensures Sulla NEVER starts n8n, Docker, or other services without explicit user permission — a hard preference confirmed on March 25, 2026.

## Functions

### `require_approval(action: string, details: object) -> ApprovalResult`

**Purpose:** Detect if an action requires approval and prompt user if needed.

**Detection Patterns (auto-trigger):**
- `docker` / `docker-compose` / `dockerd` — any Docker daemon operation
- `n8n` — n8n workflow automation
- `systemctl` / `service` — systemd service management
- `podman` — container runtime
- Any command containing: `up`, `start`, `run`, `serve` with service-related keywords

**Parameters:**
- `action`: Human-readable description of what you want to do
- `details`: Object containing `{command: string, reason: string, risk: string}`

**Returns:**
```typescript
type ApprovalResult = 
  | { status: 'approved', timestamp: string, approval_id: string }
  | { status: 'denied', reason: string }
  | { status: 'pending' }
```

### `check_pending_approvals() -> PendingApproval[]`

Returns list of actions awaiting user approval.

### `record_approval(approval_id: string, approved: boolean, notes?: string)`

Records user's approval decision for audit trail.

## Usage in Agents

Before any Docker/n8n/systemd command, agents MUST call:

```javascript
const result = await skill.approval.require_approval(
  "Start n8n container",
  { 
    command: "docker-compose up -d n8n",
    reason: "n8n needed for workflow automation",
    risk: "Modifies running services, consumes system resources"
  }
);

if (result.status === 'approved') {
  // Execute the action
} else if (result.status === 'denied') {
  // Do not execute, log the denial
}
```

## Storage

Approvals stored in: `~/sulla/data/approvals/`
- `pending.json` — actions awaiting approval
- `approved.json` — completed approvals with timestamps
- `denied.json` — rejected actions

## Hard Constraint

> **NEVER bypass this approval check for Docker, n8n, or systemd operations.**
> This is a HARD RULE, not a guideline.
