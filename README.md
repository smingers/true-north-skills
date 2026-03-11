# vibeloupe

LEARNING FAST >>> CODING FAST

Vibeloupe is an IDE/dev tool-native workflow designed to help fast-moving teams figure out what to ship next, how to measure success, and when to double-down or pivot. It's built to work with any AI-enabled coding apps. The more experiments you run over time, the smarter your team gets, the more you deliver what matters for your customers.

The skills form a loop: **/hypothesis-validator** helps you design the right test before you build. **/learn-loop** tracks what you ran and what you actually learned. **/spec-review** stress-tests a plan before you commit to it.

---

## Installation

**Requires:** [Claude Code](https://claude.ai/code) (CLI)

```bash
# Add this marketplace
claude plugin marketplace add smingers/vibeloupe

# Install the plugin
claude plugin install vibeloupe
```

Once installed, the three slash commands are available in any Claude Code session.

---

## Usage

```
/spec-review [paste your PRD or plan]
/hypothesis-validator [describe your idea or problem]
/learn-loop              # auto-detects planning vs. reflection based on your log
/learn-loop plan         # force planning mode
/learn-loop reflect      # force reflection mode
```

---

## Skills

### `/spec-review`
Sharp, opinionated feedback on any plan, PRD, or strategic document. Runs it through eight analytical lenses — argument chain, hidden assumptions, motivated reasoning, pre-mortem, moonshot alternatives, and more — then surfaces the top hypotheses worth testing before you proceed.

### `/hypothesis-validator`
Takes a raw idea or hunch and turns it into a testable experiment. Identifies your riskiest assumptions, ranks them, and outputs a tiered test plan from cheapest to most expensive — with a specific recommendation for what to run first. Includes Mom Test coaching for customer interview questions.

### `/learn-loop`
Runs a weekly Build-Measure-Learn session. In planning mode, sharpens your learning goals into falsifiable hypotheses with pass/fail criteria. In reflection mode, debriefs what you actually ran and what changed in your beliefs. Writes everything to `LEARNING_LOG.md` — a permanent, append-only record of your experiments.

### `/sprint-recap`
Reads git history (default 7 days), classifies commits into user-facing vs. internal, and infers what problem the team was solving. Flags mismatches between effort and user impact.

---

## The Core Loop

```
  ┌─────────────────────────────────────────────────────────┐
  │                    LEARNING_LOG.md                      │
  │        (persistent record across all sessions)          │
  └─────────────────────────────────────────────────────────┘
                              ▲
        ┌─────────────────────┼─────────────────────┐
        │                     │                     │
        ▼                     ▼                     ▼
┌───────────────┐     ┌───────────────┐     ┌───────────────┐
│  /hypothesis- │     │  /learn-loop  │     │  /spec-review │
│   validator   │     │               │     │               │
└───────────────┘     └───────────────┘     └───────────────┘
   "I have an           "Plan week"           "Review this
    idea to test"       "Reflect on week"      PRD/spec"
```

## How They Connect

1. **Idea → Test Plan**: You have a hunch → `/hypothesis-validator` turns it into an experiment → that experiment goes into your `LEARNING_LOG.md`

2. **Weekly Cadence**: Every week, `/learn-loop plan` sets up your experiments → you run them → `/learn-loop reflect` captures what you learned

3. **Spec Check**: Before committing to a big plan, `/spec-review` surfaces the assumptions worth testing → those become experiments for `/learn-loop`

4. **Retrospective**: `/sprint-recap` shows what the team actually built → you compare that against what you planned to learn

The **LEARNING_LOG.md** is the connective tissue — every skill reads from it and writes to it, so your learning accumulates over time and patterns become visible across weeks.

---

Much more to come very soon!
