---
name: sprint-recap
description: Use when the user wants to understand what the team built or worked on recently, get a weekly summary of git activity, reconstruct the team's goals from recent commits, or produce a changelog from a product perspective. Triggers on phrases like "what did we ship this week", "sprint recap", "week in review", "summarize our commits", "what was the team working on", "what changed this week", "generate a changelog", "what did we build", "show me recent progress", or any request to understand recent engineering work through a user or product lens. Use this whenever someone asks about recent work or recent changes in a project — even if they don't say "sprint" or "git".
version: 0.1.0
---

# Sprint Recap

Analyze recent git commits and produce a factual, user-centric report that answers: *what changed this week that users or the team would actually notice?*

The goal is not to list files, and not to editorialize. Report what changed — not whether it worked, not whether it was impressive, not what it implies about the team's direction. Leave judgments about impact and quality to the reader.

## Persona

Adopt this voice throughout:
- Factual and direct — report what changed, not what it means or whether it was a success
- No praise, no spin — phrases like "significantly improved", "robust", "powerful new capability" are editorializing; avoid them
- Selective — surface the 3–5 most meaningful functional changes, not everything; an exhaustive list is noise
- Honest about what you can and can't infer from commit messages and diffs alone

## Process

### Phase 1 — Determine the time window

Default to the **last 7 days**. If the user has specified a different range ("last 2 weeks", "since March 1", "this sprint"), honor that exactly. If ambiguous, proceed with 7 days and note the window at the top of the report.

Check that you're in a git repository. If not, say so and stop.

### Phase 2 — Gather git data

Run these commands to collect the raw material:

```bash
# All commits in the window with abbreviated hash and subject
git log --since="7 days ago" --pretty=format:"%h %s" --date=short

# Commits with file-level stats (to understand scope of each change)
git log --since="7 days ago" --pretty=format:"=== %h %s" --stat

# Aggregate: which files changed the most across all commits
git log --since="7 days ago" --pretty=format: --numstat | awk '{if ($1 != "") print $1+$2, $3}' | sort -rn | head -20

# Contributors
git log --since="7 days ago" --pretty=format:"%an" | sort | uniq -c | sort -rn
```

Adjust `--since` to match whatever time window was determined in Phase 1.

If there are **zero commits** in the window, say so plainly and offer to extend the range.

### Phase 3 — Classify each commit

For each commit, assign exactly one type. When the message is ambiguous, let the files changed be the tiebreaker.

**New Feature** — Adds user-facing capability that didn't exist. Signals: `feat:` prefix, new files in feature/component directories, new routes or endpoints, new UI components.

**Improvement / Bug Fix** — Makes existing functionality work better or more reliably. Signals: `fix:`, `patch:`, `improve:` prefixes; targeted edits to existing logic; changes to error handling or edge cases.

**Refactor** — Restructures internals without changing behavior. Signals: `refactor:`, `chore:`, `cleanup:` prefixes; file renames or moves; no new exports or routes.

**Infrastructure / Tooling** — CI, dependencies, configs, build. Signals: `.github/`, `package.json`, `Dockerfile`, config files changed.

**Documentation** — Docs and comments only. Signals: only `.md` or comment-only changes.

### Phase 4 — Map file paths to user-facing areas

This is the most important inference step. Look at which directories and files changed most, and reason about what part of the user experience they touch.

Patterns to look for:
- Files in `components/`, `pages/`, `views/`, `screens/` → directly user-facing UI
- Files in `api/`, `routes/`, `endpoints/` → backend that users or clients call
- Files in `auth/`, `login/` → authentication and account access
- Files in `lib/`, `utils/`, `helpers/` → shared logic (user impact depends on what uses it)
- Files in `.github/`, `scripts/`, `config/` → developer tooling, no user impact
- Files in `test/`, `spec/`, `__tests__/` → test coverage, no direct user impact
- Migration files → data model changes, may or may not be user-visible

If 70%+ of changes concentrate in one area, that's the week's center of gravity — name it explicitly.

### Phase 5 — Select and describe

From all the user-impact commits (New Feature and Improvement/Bug Fix only), pick the **3–5 most significant** by functional importance — not by lines of code, but by how meaningfully they change what users can do or experience. Ignore refactors, infrastructure, CI, and documentation entirely unless the user explicitly asks for them.

For each selected item, write 1–2 sentences stating:
- What changed (factually)
- What it means for users (only what's directly observable — no assumptions about downstream impact)

Avoid:
- File names and directory paths in the narrative sections
- Evaluative language: "significantly", "greatly", "powerful", "robust", "important"
- Causal assumptions you can't verify from the diff ("this will make X faster", "users will now be able to…" unless the feature directly enables it)
- Listing every qualifying commit — pick the most important ones and stop

If you can't identify any user-impact commits (the week was all refactoring/infra/docs), say so plainly in "What We Shipped" and omit the feature/improvement sections entirely.

### Phase 6 — Produce the report

Write a clean markdown report using the format below.

## Output Format

Always use this exact structure. Omit any section where there's genuinely nothing to say.

```markdown
# Sprint Recap: [start date] – [end date]

## What We Shipped
[2–3 sentences. Name the dominant theme of the week in plain terms — what area of the
product saw the most work? Do not editorialize about quality or impact. If it was a
refactoring/maintenance week with no user-facing changes, say that plainly.]

## New Features
[Only include if there were genuine new-feature commits. Maximum 4 items — pick the most
significant by functional importance, not commit count.]

- **[Feature name]**: [1–2 sentences describing what changed for users, factually]

## Bug Fixes & Improvements
[Only bug fixes and measurable improvements to existing behavior. No refactors.
Maximum 3 items — the ones with clearest user impact.]

- **[What changed]**: [1 sentence, factual]

## By the Numbers
- **Time window**: [date range]
- **Commits**: [N total, note if a significant portion are automated/bot commits]
- **Contributors**: [names if ≤ 4, otherwise "N contributors"]
- **User-facing vs. internal**: [optional — if you can estimate the split between
  user-impact commits vs. refactoring/infra/docs, include it here]
```

## Critical Rules

- **No editorializing**: Do not write "significantly improved", "powerful", "robust", "meaningfully expanded", or any other evaluative language. State what changed, not how good it is.
- **No assumed impact**: Do not write "users will now be able to…" or "this makes the system more reliable" unless the change directly and obviously enables it. Report the change itself.
- **No refactoring in the itemized lists**: Refactors, code cleanup, CI, dependency updates, and internal restructuring do not appear in New Features or Bug Fixes & Improvements. If the week was mostly this kind of work, say so in "What We Shipped" and keep the lists short or absent.
- **Pick the most important items**: New Features max 4 items, Bug Fixes & Improvements max 3. If there are more candidates, pick the ones with the clearest functional impact and drop the rest.
- **No file paths in narrative sections**: Technical paths belong nowhere in the report except optionally in "By the Numbers" context.
- Never call something a new feature unless it clearly adds user-visible capability — a refactor with a `feat:` prefix is still a refactor.
- If a commit message is opaque ("fix stuff", "wip"), infer the type from files changed.
- The "By the Numbers" section must always appear.
- Keep the full report under 400 words.
