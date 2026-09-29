# review-gates

Review gates for plan-driven agentic development in Claude Code.

When an agent writes a plan or spec and another agent reviews it, the two
can loop forever: the reviewer always finds something, the author always
accepts it, and every revision gives the reviewer new material. These
skills break the loop with fixed review criteria, severity gating, an
explicit APPROVED exit, a human tiebreaker, and a log of settled
decisions that reviewers treat as closed.

## Skills

| Skill | Runs in | What it does |
|---|---|---|
| `plan-review` | Fresh subagent | Checks a plan's direction: goal, approach, missing pieces, sequencing, risk, scope. Ignores missing detail. |
| `spec-review` | Fresh subagent | Finds only problems that would block implementation. BLOCKERs require a concrete failure. |
| `triage-review` | Authoring session | Marks each finding ACCEPT, REJECT, or PARTIAL, waits for your calls, applies approved changes, proposes decisions. |
| `decisions` | Authoring session | Creates or appends to DECISIONS.md with reasons and "revisit if" conditions. |

All skills are manual only (`disable-model-invocation: true`), so nothing
starts a review round without you.

## Workflow

```
write plan
/review-gates:decisions <dir>          save reasoning before context compacts
commit
/review-gates:plan-review <plan>
/review-gates:triage-review <plan>     you make the calls
commit
write spec
/review-gates:spec-review <spec> <plan>
/review-gates:triage-review <spec>
commit
build
```

Add `round 2` to a reviewer's arguments to review only the uncommitted
diff. Commit before the first review so the diff is meaningful. After two
rounds, make the remaining calls yourself.

Works alongside Superpowers: use its brainstorming and writing stages to
produce the documents, and these skills as the gates between them.

## Install

```
/plugin marketplace add <github-user>/review-gates
/plugin install review-gates@review-gates
```

For local development:

```
claude --plugin-dir ./review-gates
```
