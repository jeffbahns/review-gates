---
name: triage-review
description: Respond to a plan or spec review from the session that wrote the document. Marks each finding ACCEPT, REJECT, or PARTIAL with reasons, waits for the user's calls, applies only approved changes, then proposes DECISIONS.md entries.
argument-hint: <path/to/reviewed file> [path/to/DECISIONS.md]
disable-model-invocation: true
---

# Triage a review

Run this in the session that wrote the reviewed document, because you
have context the reviewer did not: what the user told you, what was
ruled out, and why.

Arguments: $ARGUMENTS

The review to triage is the most recent plan-review or spec-review
output in this conversation. If there is none, ask the user to paste it
and stop.

## Step 1: Respond, do not edit

For each BLOCKER, SHOULD, and risk in the review,
respond with one of:

- ACCEPT: the concern is valid. Describe the specific change you would
  make.
- REJECT: explain why, citing context the reviewer did not have
  (something the user said, a prior decision, a constraint). If you
  cannot point to a specific source for the reason, say so plainly
  rather than reconstructing one.
- PARTIAL: state which part is valid and what you would change.

Skip NITs unless one is a one-line fix worth making.

Be honest in both directions. Do not accept a point just because a
reviewer raised it, and do not reject one just because it conflicts
with what you already wrote.

Then stop and wait for the user. Do not edit any files in this step.

## Step 2: Apply approved changes

After the user confirms or overrules your calls, apply only the changes
they approved. Keep edits minimal: fix what was raised, do not rewrite
surrounding sections. Do not commit; the user will review the diff.

## Step 3: Propose decisions

Draft DECISIONS.md entries for:

- every REJECT the user upheld (the decision is to keep the current
  approach)
- every ACCEPT or PARTIAL that changed a design choice (not wording
  fixes)

Use the format from the `review-gates:decisions` skill and continue the existing
numbering. Show the proposed entries and wait for the user's approval
before writing them. If DECISIONS.md does not exist, propose creating it
where both the spec and the plan can find it (with Superpowers,
`docs/superpowers/DECISIONS.md`).

After writing, remind the user that a round 2 review is optional, and
that more than two rounds means the user should make the remaining
calls directly.
