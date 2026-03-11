# vibeloupe

LEARNING FAST >>> CODING FAST

Vibeloupe is an IDE/dev tool-native workflow designed to help fast-moving teams figure out what to ship next, how to measure success, and when to double-down or pivot. It's built to work with any AI-enabled coding apps. The more experiments you run over time, the smarter your team gets, the more you deliver what matters for your customers.

The skills form a loop: **/hypothesis-validator** helps you design the right test before you build. **/learn-loop** tracks what you ran and what you actually learned. **/spec-review** stress-tests a plan before you commit to it. **/sprint-recap** connects what shipped to what you were trying to learn.

---

## Installation

**Claude Code:**
```bash
claude plugin marketplace add smingers/vibeloupe
claude plugin install vibeloupe
```

**Cursor:**
```
/add-plugin vibeloupe
```

Once installed, the four slash commands are available in your session.

---

## Usage

```
/spec-review [paste your PRD or plan]
/hypothesis-validator [describe your idea or problem]
/learn-loop              # auto-detects planning vs. reflection based on your log
/learn-loop plan         # force planning mode
/learn-loop reflect      # force reflection mode
/sprint-recap            # last 7 days (default)
/sprint-recap [N days]   # custom time window
```

---

## Skills

### `/spec-review`
Sharp, opinionated feedback on any plan, PRD, or strategic document. Runs it through eight analytical lenses — argument chain, hidden assumptions, motivated reasoning, pre-mortem, moonshot alternatives, and more — then surfaces the top hypotheses worth testing before you proceed. Saves results to `.vibeloupe/prd-reviews.json`.

### `/hypothesis-validator`
Takes a raw idea or hunch and turns it into a testable experiment. Identifies your riskiest assumptions, ranks them, and outputs a tiered test plan from cheapest to most expensive — with a specific recommendation for what to run first. Includes Mom Test coaching for customer interview questions. Saves results to `.vibeloupe/experiments.json`.

### `/learn-loop`
Runs a weekly Build-Measure-Learn session. In planning mode, sharpens your learning goals into falsifiable hypotheses with pass/fail criteria. In reflection mode, debriefs what you actually ran and what changed in your beliefs. Reads and writes `.vibeloupe/experiments.json` — a persistent, structured record of every experiment you've run.

### `/sprint-recap`
Reads git history (default 7 days), classifies commits into user-facing vs. internal, and infers what problem the team was solving. Cross-references open experiments from `.vibeloupe/experiments.json` to surface which ones the sprint's work may be advancing — and which ones are drifting without attention. Saves results to `.vibeloupe/sprints.json`.

---

## How It Works

All skills read and write structured JSON to a `.vibeloupe/` directory in your repo. This is the connective tissue — a machine-readable record that accumulates over time and lets the skills reason across past experiments, reviews, and sprints.

```
.vibeloupe/
  experiments.json   ← created/updated by /hypothesis-validator and /learn-loop
  prd-reviews.json   ← created by /spec-review
  sprints.json       ← created by /sprint-recap
```

Add `.vibeloupe/` to your repo and commit it like any other file. The data travels with your code.

---

## The Core Loop

```
  ┌──────────────────────────────────────────────────────────────┐
  │                      .vibeloupe/                             │
  │          experiments.json · prd-reviews.json · sprints.json  │
  └──────────────────────────────────────────────────────────────┘
                               ▲
        ┌──────────────────────┼───────────────────────┐
        │                      │                       │
        ▼                      ▼                       ▼
┌───────────────┐     ┌────────────────┐     ┌───────────────┐
│  /hypothesis- │     │  /learn-loop   │     │  /spec-review │
│   validator   │     │                │     │               │
└───────────────┘     └────────────────┘     └───────────────┘
   "I have an idea      "Plan week /           "Review this
    to test"             Reflect on week"       PRD/spec"
                               │
                               ▼
                      ┌────────────────┐
                      │  /sprint-recap │
                      └────────────────┘
                        "What shipped?
                         What does it
                         advance?"
```

## How They Connect

1. **Idea → Test Plan**: You have a hunch → `/hypothesis-validator` turns it into an experiment → saved to `.vibeloupe/experiments.json`

2. **Weekly Cadence**: Every week, `/learn-loop plan` sets up your experiments → you run them → `/learn-loop reflect` captures what you learned and updates the experiment status

3. **Spec Check**: Before committing to a big plan, `/spec-review` surfaces the assumptions worth testing → those become inputs for `/hypothesis-validator` or `/learn-loop`

4. **Retrospective**: `/sprint-recap` shows what the team actually shipped → cross-references open experiments → flags what's being advanced and what's drifting

---

Much more to come very soon!
