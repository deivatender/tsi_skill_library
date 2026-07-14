# Multica Setup Guide

Multica is an open-source task collaboration platform where humans and AI coding agents work together in the same workspace. Issues get assigned to agents the way you'd assign them to a teammate, and agents execute the work, report progress, and reply in comments.

This guide covers installation and configuration of the six core building blocks: **Workspace, Projects, Skills, Agents, Squads, and Tasks**.

---

## 1. How Multica Works (Read This First)

Multica does **not** execute agent work on its own servers. Agents run locally, driven by a **daemon** process on your machine (or a teammate's machine) that talks to one of 14 supported AI coding tools, including Claude Code, Codex, Cursor, Copilot, and CodeBuddy. Your API keys, source code, and toolchain stay on the machine running the daemon — Multica's server only coordinates task queueing, status, and comments.

Two backend options:

| Option | Description |
|---|---|
| **Multica Cloud** | Managed backend hosted by Multica. You install the CLI and run the daemon locally, then connect to the hosted server. ~5 minutes to set up. |
| **Self-host** | Run the full backend (server, database, storage) on your own infrastructure via Docker Compose or Helm. |

A **desktop app** is also available and works with either backend — it bundles the CLI and starts the daemon automatically on launch, so no manual `multica setup` step is needed.

---

## 2. Prerequisites

Before starting, confirm:

- [ ] At least one supported AI coding tool is installed locally (Claude Code recommended as the first pick — most complete feature set)
- [ ] macOS, Linux, or Windows machine to run the daemon
- [ ] Admin rights on the workspace if you'll be creating agents, skills, and squads

---

## 3. Install and Set Up the Workspace

### 3.1 Create an account

Sign up at multica.ai using email (6-digit verification code) or Google login. A default workspace is created automatically — no setup wizard required.

### 3.2 Install the CLI

**macOS / Linux (Homebrew):**
```bash
brew install multica-ai/tap/multica
```

**macOS / Linux (no Homebrew):**
```bash
curl -fsSL https://raw.githubusercontent.com/multica-ai/multica/main/scripts/install.sh | bash
```

**Windows (PowerShell):**
```powershell
irm https://raw.githubusercontent.com/multica-ai/multica/main/scripts/install.ps1 | iex
```

Verify:
```bash
multica version
```

### 3.3 Log in and start the daemon

```bash
multica setup
```

This single command:
1. Points the CLI at Multica Cloud
2. Opens the browser for login
3. Stores a personal access token (PAT) in `~/.multica/config.json`
4. Starts the daemon automatically (polls for tasks every 3 seconds, sends heartbeats every 15 seconds)

Check status any time:
```bash
multica daemon status
```

If using the **desktop app**, the daemon starts automatically on launch — `multica setup` is not required.

### 3.4 Verify the runtime is online

In the web UI: **Settings → Runtimes**. One active runtime should appear per locally installed AI coding tool.

### 3.5 Configure the workspace

A workspace is the self-contained space where a team collaborates — every issue, member, comment, agent, and skill belongs to one. Three settings are decided at creation and matter long-term:

| Setting | Notes |
|---|---|
| **Workspace name** | Display name, editable later |
| **Slug** | Used in the workspace URL. Lowercase letters/digits only. **Cannot be changed after creation.** |
| **Issue prefix** | e.g. `TSI` in `TSI-101`. Uppercase letters/digits, up to 10 characters. **Avoid changing later** — it breaks every historical reference. |

Create via CLI:
```bash
multica workspace create
```

Add teammates under **Settings → Members** and assign roles (owner / admin / member — see the Members and Roles doc for the full permission matrix).

---

## 4. Projects

A **project** groups related issues into one trackable unit — used when work is bigger than a single issue but smaller than the whole workspace (a migration, a release, a multi-part feature).

Each project has:
- Name, icon, description
- **Lead** — a workspace member or an agent
- **Status** — `planned` / `in_progress` / `paused` / `completed` / `cancelled`
- **Priority** — `urgent` / `high` / `medium` / `low` / `none`
- **Progress %** — auto-derived from the status of linked issues (cancelled issues excluded; backlog issues count toward the total but not the completed count)

**Setup steps:**
1. Go to **Projects → New Project** and fill in name, description, lead, priority.
2. Attach one or more GitHub repositories under the project's **Resources** section — any agent working on an issue inside this project can read/write to those repos automatically.
3. Pin frequently used projects to your sidebar (personal, per-user).

Notes:
- An issue belongs to at most one project; linking/unlinking is reversible any time.
- Deleting a project does **not** delete its issues — they simply unlink back to the flat workspace issue list.
- Set an **agent** as project lead for projects that are mostly agent-delegated (e.g., "Weekly Bug Triage" led by a triage agent).

---

## 5. Agents

An **agent** is an AI teammate backed by a specific AI coding tool. Creating one requires only a **name** and a **runtime** (the coding tool) — everything else is optional and can be changed later.

### 5.1 Create an agent

Web UI: **Agents → + New**, or via CLI:
```bash
multica agent create
```

### 5.2 Configuration reference

| Field | Purpose |
|---|---|
| **Name** | Unique within the workspace; shown on boards/comments |
| **Runtime / Provider** | The AI coding tool (Claude Code, Codex, Cursor, Copilot, CodeBuddy, etc.) |
| **Model** | Optional; leave blank for the tool's default |
| **Instructions** | System prompt — role and rules prepended to every task |
| **custom_env** | Environment variables injected at execution time (e.g. API keys). Stored in plaintext server-side; use scoped, rotatable credentials only — never production-grade secrets |
| **custom_args** | CLI flags appended to the tool's command line, e.g. `["--max-turns", "100"]` |
| **Visibility** | `workspace` (any member can assign) or `private` (owners/admins/creator only). Defaults to `private` |
| **Concurrency limit** | `max_concurrent_tasks`, default 6. Extra tasks queue rather than reject |

Example instructions block:
```
You're a frontend code-review agent. Read the diff first. Focus only on:
- Styling issues (Tailwind class names, box model)
- Accessibility (a11y)
Don't change code — leave suggestions in a comment.
```

### 5.3 Archive / restore

Archiving an agent immediately cancels all its unfinished tasks and removes it from assignment pickers. Historical data (tasks, comments) is preserved, and it can be restored later.

---

## 6. Skills

A **Skill** is a reusable capability bundle — a `SKILL.md` file plus optional supporting files — that gets delivered to the AI coding tool automatically at task execution time.

### 6.1 Types

| Type | Where it lives |
|---|---|
| **Workspace skill** | Stored in Multica's cloud; synced to the daemon at execution time. Standard way to share skills across the team. |
| **Local skill** | Lives in a directory on your machine (tool-specific default path, e.g. Claude Code's `~/.claude/skills/`). Scanned and imported manually. |

### 6.2 Adding a skill

- Create a new skill from scratch in the web UI
- Import from GitHub or ClawHub
- Scan and import from an existing local skill directory

Then attach it to one or more agents on the agent's configuration page.

### 6.3 Security note

Skills imported from GitHub or ClawHub may include scripts or executable content. **Multica does not sign, audit, or sandbox third-party skills** — review the `SKILL.md` and every accompanying file before importing, especially for projects touching sensitive data. Prefer locally authored skills for anything security-sensitive, and only import third-party skills from trusted sources.

Editing a skill only affects **newly created** tasks — tasks already running continue on the old version.

For TSI's existing SKILL.md library (`.NET performance optimization`, HTML development, Power BI paginated reports, SOC2 PHI documentation, delivery lifecycle, code reviewer, etc.), these can be imported as **workspace skills** and attached to the relevant agents so the whole team's agents share the same conventions.

---

## 7. Squads

A **squad** is a named group of agents (and optionally human members) with one designated **leader agent**. Assign an issue to the squad, and the leader decides who on the team should pick it up — useful when you have several specialists and don't know in advance which one fits a given issue.

### 7.1 Create a squad

Web UI: **Squads → New squad**, providing:
- **Name** (e.g. `Frontend Team`, `Bug Triage`)
- **Description** (optional)
- **Leader** — an existing agent (added automatically with role `leader`)

CLI:
```bash
multica squad create --name "Frontend Team" --leader frontend-lead-agent
multica squad member add <squad-id> --member-id <agent-or-user-uuid> --type agent --role "Owns Tailwind / shadcn surface"
```

### 7.2 How it runs

1. Assigning an issue to a squad enqueues a task for the **leader** only.
2. The leader is briefed with a system-managed **Squad Operating Protocol**, the **Squad Roster** (exact `@mention` markdown for each member), and any custom **Squad Instructions** you've written (e.g. routing rules like "DB work to Alice, frontend to Bob").
3. The leader posts one delegation comment `@`-mentioning the chosen member(s), which triggers a task for each mentioned agent, then records its evaluation and stops.
4. The leader is re-triggered automatically on most subsequent comments (except when a member has already explicitly `@`-mentioned someone else — that's treated as a completed hand-off).

### 7.3 Permissions

| Action | Who |
|---|---|
| Create / update / archive a squad | Owner or admin |
| Add/remove members, change roles | Owner or admin |
| Assign an issue to a squad | Any workspace member |
| `@`-mention a squad in a comment | Any workspace member |

Archiving a squad transfers any issues currently assigned to it back to the leader agent, so work doesn't go silent.

---

## 8. Tasks

A **task** is the unit of every agent run. Assigning an issue, `@`-mentioning an agent, sending a chat message, or an Autopilot firing on schedule — each produces a new task.

### 8.1 Task lifecycle

`Queued → Dispatched → Running → Completed / Failed / Cancelled`

- **Queued** — waiting for a daemon to claim it
- **Dispatched** — a daemon claimed it, starting the AI tool
- **Running** — the AI tool is actively working
- **Completed** — output written back (comments, commits, status changes)
- **Failed** — errored or timed out; retried automatically if the reason is retryable
- **Cancelled** — manually cancelled

### 8.2 Timeouts and retries

| Situation | Timeout |
|---|---|
| Dispatched but never started | 5 minutes |
| Running too long | 2.5 hours |

Retryable failures (`runtime_offline`, `runtime_recovery`, `timeout`) are automatically requeued — **maximum 2 attempts total**, and only for issue- or chat-triggered tasks (not Autopilot tasks). Non-retryable failures (`agent_error`, e.g. tool-side API errors or quota limits) stay failed.

If an issue-assigned task fails without a successful retry, the issue's status automatically rolls back from `in_progress` to `todo`, so it's visible on the board as needing another look.

### 8.3 Manual rerun

```bash
multica issue rerun <issue-id>
```

Starts a brand-new task with a fresh agent session (no inherited context) — use this when the previous output was wrong, as opposed to automatic retry, which resumes the prior session and is meant for infrastructure failures only.

### 8.4 Assigning your first task

Web UI, or via CLI:
```bash
multica issue create --title "Add an ASCII architecture diagram to the README"
multica issue assign TSI-1 --to my-agent-name
```

Watch progress live in the web UI (WebSocket-updated) as the task moves from `queued` → `dispatched` → `running` → `completed`.

---

## 9. Recommended Rollout Order

1. Create the workspace (name, slug, issue prefix) and invite the team
2. Install the CLI and start the daemon on each machine that will run agents
3. Create 2–3 pilot agents against the existing TSI stack (.NET/C#, JS/React, Python)
4. Import existing SKILL.md files as workspace skills and attach to the relevant agents
5. Create a project for the pilot initiative and attach its GitHub repo(s)
6. Assign a few real issues to individual agents to validate output quality
7. Once specialties are clear, group agents into squads (e.g. Frontend, Backend, QA) with a leader per squad
8. Expand to Autopilots for recurring work (standups, audits) once the core flow is trusted

---

## 10. Reference Links

- Docs home: https://multica.ai/docs
- Cloud quickstart: https://multica.ai/docs/cloud-quickstart
- Self-host quickstart: https://multica.ai/docs/self-host-quickstart
- CLI command reference: https://multica.ai/docs/cli
- AI coding tools matrix: https://multica.ai/docs/providers
- GitHub repository: https://github.com/multica-ai/multica

*Prepared for Tender Software India (TSI) — internal reference for Multica pilot evaluation.*
