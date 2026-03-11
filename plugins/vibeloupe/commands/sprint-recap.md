---
description: "Analyze the last 7 days of git commits and produce a user-centric weekly report"
argument-hint: "[optional: time range, e.g. 'last 2 weeks' or 'since March 1']"
---

# Sprint Recap

Analyze recent git commits and produce a clear, user-centric report. Reconstruct what the team was working on and why — not just what files changed.

## Input

A time range may be provided as arguments. If none is given, default to the last 7 days.

**Arguments:** $ARGUMENTS

## Instructions

Read `../skills/sprint-recap/SKILL.md` and execute it exactly as written — do not skip any phase.

Critical phases that must not be skipped:

**Bootstrap (before anything else):**
- Check for `.vibeloupe/sprints.json` — create with `[]` if missing, read for prior patterns if it exists
- Check for `.vibeloupe/experiments.json` — read it and extract all experiments with `status` of `"untested"` or `"running"` for use in Phase 5.5

**Phase 5.5 — Cross-reference open experiments:**
- After inferring the problem being solved, compare the sprint's work against every open experiment
- Surface which experiments this sprint advances, and which are drifting

**Write to Data (after producing the report):**
- Append a record to `.vibeloupe/sprints.json` including `linked_experiment_ids` for any experiments rated Advances
