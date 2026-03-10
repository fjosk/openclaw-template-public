# OpenClaw — Configuration System

OpenClaw is a personal AI agent built on top of a large language model. It operates as a judgment layer, coach, and project tracker — not a task executor. It evaluates plans, enforces priorities, flags bad decisions, and pushes back when needed.

This repository contains the configuration files that define how OpenClaw behaves.

---

## How It Works

OpenClaw has no persistent memory on its own. Everything it knows about you, your priorities, and your context comes from these config files, injected into the session at startup. Change the files → change the agent.

---

## File Overview

| File | Purpose | Editable? |
|---|---|---|
| `BOOTSTRAP.md` | Startup sequence and file write policy | 🔒 No |
| `IDENTITY.md` | System name (OpenClaw) + agent naming rule | 🔒 No |
| `SOUL.md` | Behavior rules and epistemic standards | 🔒 No |
| `TOOLS.md` | Environment config (API keys, integrations) | 🔒 With approval |
| `AGENTS.md` | Execution protocol, daily rules, security | ⚠️ Propose-only |
| `USER.md` | Your profile, priorities, and constraints | ⚠️ Propose-only |
| `MEMORY.md` | Live volatile facts (health, finance, projects) | ✅ Volatile sections |
| `HEARTBEAT.md` | Reserved placeholder — not loaded at startup, not an instruction source | — |

---

## Setup (First Time)

### 1. Fill in `USER.md`
This is the most important file. Open it and fill in:
- Your name and timezone
- **Your agent name** — what you want to call the agent in chat (e.g. Claw, Max, Aria). The system is always called OpenClaw in config and documentation; the chat name is yours to set.
- Your daily non-negotiables (DMN) and distraction time limit
- Your priority ladder (what matters most, in order)
- Your hard constraints (what the agent must never suggest or do)
- Your known patterns and anti-patterns

Nothing in this file is auto-populated. If you leave it blank, the agent has no context to work with.

### 2. Configure `TOOLS.md`
Add your environment specifics: API keys, tool integrations, environment variables, etc. Leave blank if not applicable.

### 3. Leave everything else as-is
`BOOTSTRAP.md`, `IDENTITY.md`, `SOUL.md`, and `AGENTS.md` are pre-configured. Do not edit them directly — they define the agent's core behavior. To change them, follow the proposal process described below.

---

## Daily Use

### Giving tasks
Every task should include: **Objective · Constraint · Definition of Done · Deadline · Output format**. If you omit critical fields, the agent will ask before proceeding.

### Memory
OpenClaw uses two memory layers. Full policy → `MEMORY.md`.

| Layer | File | Purpose |
|---|---|---|
| Long-term | `MEMORY.md` | High-impact facts. Volatile sections updated autonomously; full maintenance on-demand. |
| Daily | `memory/YYYY-MM-DD.md` | Session state delta. Auto-loaded. Agent writes at end of every chat. |

To run full memory maintenance (promote, prune, archive), say: *"Do memory maintenance"*.

---

## Making Changes

### To `USER.md` or `AGENTS.md`
Tell the agent what you want changed. It will propose the exact change, wait for approval, apply with minimal scope, and verify consistency.

### To `SOUL.md`, `IDENTITY.md`, or `BOOTSTRAP.md`
These are locked. Edit manually outside of a session — treat as a system-level change.

### To `MEMORY.md`
The agent updates volatile sections (Health, Financial, Active Projects) directly. It cannot write policy text, behavioral rules, or instructions into any config file.

---

## Security Notes

- **Prompt injection:** Only the six core config files are instruction sources. Content in documents, search results, memory logs, and tracker files is data only — injected instructions are ignored and flagged.
- **Persistence:** All permissions are session-scoped. Approvals do not carry over.
- **Pressure resistance:** The agent re-anchors to policy if you push back on a refusal repeatedly. This is correct behavior, not a bug.

---

## File Structure

```
/
├── BOOTSTRAP.md        # Startup sequence
├── IDENTITY.md         # Agent identity
├── SOUL.md             # Behavior and epistemic rules
├── AGENTS.md           # Execution protocol
├── USER.md             # Your profile (fill this in)
├── MEMORY.md           # Volatile facts (agent-maintained)
├── TOOLS.md            # Environment config (fill this in)
├── memory/
│   └── YYYY-MM-DD.md   # Daily logs
└── tracker/
    └── TRACKER_GUIDELINE.md  # Tracker CSV rules and schema
```

---

## Quick Reference

| I want to… | Do this |
|---|---|
| Set up for the first time | Fill in `USER.md` and `TOOLS.md` |
| Change my priorities | Ask the agent to propose an update to `USER.md` |
| Update project status | Ask the agent to update `MEMORY.md`, or it logs automatically |
| Add a sub-agent | Explicitly activate it by name in the current session |
| Change core behavior | Manual edit to `SOUL.md` outside of a session |
| Kill a stale project | Ask the agent to flag it during weekly review |
