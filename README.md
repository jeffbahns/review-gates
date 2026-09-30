# review-gates

Review gates for spec-driven agentic development in Claude Code.

When an agent writes a spec or plan and another agent reviews it, the two
can loop forever: the reviewer always finds something, the author always
accepts it, and every revision gives the reviewer new material. These
skills break the loop with fixed review criteria, severity gating, an
explicit APPROVED exit, a human tiebreaker, and a log of settled
decisions that reviewers treat as closed.

## Skills

| Skill | Runs in | What it does |
|---|---|---|
| `spec-review` | Fresh subagent | Checks the design before planning: goal, approach, scope, behavior, data, interfaces. BLOCKERs require a concrete failure. |
| `plan-review` | Fresh subagent | Checks the implementation plan against the spec: coverage, task order, tasks executable as written. BLOCKERs require a concrete failure. |
| `triage-review` | Authoring session | Marks each finding ACCEPT, REJECT, or PARTIAL, waits for your calls, applies approved changes, proposes decisions. |
| `decisions` | Authoring session | Creates or appends to DECISIONS.md with reasons and "revisit if" conditions. |

All skills are manual only (`disable-model-invocation: true`), so nothing
starts a review round without you.

## Workflow

```
/superpowers:brainstorming                   writes and commits the spec,
                                             then asks you to review it
/review-gates:decisions docs/superpowers     save reasoning before context compacts
/review-gates:spec-review <spec>
/review-gates:triage-review <spec>           you make the calls
commit, then approve the spec                brainstorming moves on to writing-plans
/review-gates:plan-review <plan> <spec>
/review-gates:triage-review <plan>
commit, then execute
```

Brainstorming hands off to writing-plans as soon as you approve the
spec, so run the spec gate at its "please review the spec" pause, before
you say yes.

Add `round 2` to a reviewer's arguments to review only the uncommitted
diff. Commit before the first review so the diff is meaningful. After two
rounds, make the remaining calls yourself.

Built to sit between Superpowers stages: brainstorming produces the spec,
writing-plans produces the plan, and these skills are the independent
gates after each. Superpowers' own self-review runs in the authoring
session; these run in a fresh context. Without Superpowers, pass file
paths explicitly.

## Install

```
/plugin marketplace add jeffbahns/review-gates
/plugin install review-gates@review-gates
```

For local development:

```
claude --plugin-dir ./review-gates
```
