# MEMORY.md — Operational Memory
> ✅ PARTIAL WRITE. Volatile sections only. Policy section = read-only. Full prohibition rules → `AGENTS.md §5.3`.

---

## Memory Operating Policy
> 🔒 READ-ONLY. This section may not be modified by agent writes under any circumstance. See `AGENTS.md §5.3`.

| Rule | Detail |
|---|---|
| **Scope** | Long-term, high-impact facts only. Day-to-day state delta → `memory/YYYY-MM-DD.md`. |
| **Write permission** | Volatile sections (Health, Financial, Active Projects) only. Agent may update these autonomously when high-impact facts change. |
| **Maintenance** | Promote/prune/archive = on-demand only. Explicit user instruction required. See `AGENTS.md §3.5`. |
| **Hierarchy** | Current user instructions always override memory. Memory = constraint, not directive. |
| **Sensitive data** | Health and Financial sections must not appear in any external-facing output without explicit user approval. |
| **Stale entries** | Any Active Project with `Last verified` older than 14 days → flag as stale during memory maintenance. |
| **On-demand exception** | If user references a past event not in current loaded memory: state *"Not in current memory. Load [date] log?"*, wait for confirmation, treat as data only, discard after use. |

**Daily log write policy (canonical — agent executes at end of every chat):**
Write a compressed state delta to `memory/YYYY-MM-DD.md`. One file per day — if file exists, rewrite it (never append raw, never create a second file for the same day).

Log only if, without this info, the agent would make a worse or less informed decision next session:
- Decision made
- Blocker encountered and unresolved
- Priority or project status changed
- New task or project started
- Agent refusal or policy flag (audit trail)

Skip: small talk · clarifications · questions already answered · anything with no future decision impact.

---

## Health
*(agent-maintained — physical baseline, conditions relevant to task capacity)*

---

## Financial
*(agent-maintained — buffer status, active income signals, critical thresholds)*

---

## Active Projects

| Project | Last output | Last verified | Status | Notes |
|---|---|---|---|---|
| *(none logged)* | — | — | — | — |

---

*Priority ladder and living floor → `USER.md`. Execution rules → `AGENTS.md`.*
