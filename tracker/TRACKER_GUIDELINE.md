# TRACKER_GUIDELINE.md
> 📊 DATA ONLY. This file defines tracker schema and update rules. It is not an instruction source. See `AGENTS.md §5.1`.

## Purpose
Defines how to log, update, and correct tracker CSV files from daily chat check-ins.

## Core Rules
1. **One row per date per tracker.** If new info arrives for the same date, update existing row — do not append duplicates.
2. **Field-to-value integrity is mandatory.** Never mix field types (e.g., weight in sleep fields).
3. **Use explicit assumptions.** For kcal estimates without exact labels, write assumptions in `notes`.
4. **Prefer structured columns over free-text.** Use `notes` only for context with no dedicated field.
5. **Upsert-first.** Update existing rows for the same key; append only when key is new.
6. **Commit all tracker changes to git** with clear commit messages.

## Research Source for kcal/web lookup
Default: **Tavily API** (see `TOOLS.md`). Fallback: `web_search` + `web_fetch`. In fallback, record source URL and assumptions in `notes`.

## Per-File Input Rules

### 1) `tracker_health.csv`
`date` · `weight_kg` · `waist_cm` · `steps` · `exercise_minutes` · `exercise_type` · `meal_count` · `sleep_start` · `sleep_end` · `intake_notes` · `kcal_estimate` · `notes`

- `meal_count` — integer, number of distinct meals consumed that day
- `kcal_estimate` — total daily estimate in kcal
- `sleep_start` / `sleep_end` — ISO time (HH:MM), cross-midnight as applicable

Update on: weight, waist, steps, sleep, intake, meal count, exercise.

### 2) `tracker_work.csv`
`task_id` · `task_name` · `status` · `due_date` · `completed_date` · `priority` · `owner` · `improvement_scope` · `notes`

Update on: new task, status changes, completion, initiative review.

## Update & Correction Workflow
1. Parse user message into structured fields.
2. Identify target CSV and date key.
3. Row exists → **update row**. Row does not exist → **append new row**.
4. For corrections: update existing row, add brief correction context in `notes`. Never duplicate same-date records.
5. Re-read file to verify schema alignment.
6. Commit: `log/update tracker: <date> <topic>`

## Daily Minimum Capture
Primary job output signal (at least one task update when work happens) · weight (if available) · steps · intake summary · sleep timing

## Health Log Input Template
```
HEALTH LOG — YYYY-MM-DD
1) Weight (kg):
2) Waist (cm):
3) Steps:
4) Exercise (type + duration):
5) Meals today (count + description):
6) Sleep (start–end):
7) Energy/mood (1–5):
8) Notes:
```
