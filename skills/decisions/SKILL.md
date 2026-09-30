---
name: decisions
description: Create or update DECISIONS.md, a log of settled design decisions with reasons, so reviewers and future sessions do not reopen them. Use at the end of a spec or planning session, before context is compacted, or before starting a new phase.
argument-hint: [path/to/DECISIONS.md or its directory]
disable-model-invocation: true
---

# Decisions log

DECISIONS.md records what has been decided and why, so that reviewers
treat those choices as closed and new sessions inherit the reasoning
without the conversation history.

Arguments: $ARGUMENTS

If a path is given, use it (a directory means DECISIONS.md inside it).
Otherwise ask the user where it should live. Suggest a directory both
the spec and the plan sit under, since both reviewers look next to their
file and then one level up (with Superpowers, `docs/superpowers/`).

## What counts as a decision

Include:
- technology, library, and architecture choices
- scope cuts and explicit non-goals
- alternatives that were considered and rejected
- constraints the user stated that shaped the design
- review findings that were rejected and upheld

Exclude:
- wording or formatting changes
- anything still undecided (those are open questions, not decisions)
- decisions you would recommend but that were not actually made

Only record decisions that were actually made in this conversation or
are stated in the plan, spec, or existing DECISIONS.md. Do not invent
reasons. If a decision is clear but its reason is not, write
"Reason: not recorded" rather than guessing.

## Process

1. If DECISIONS.md exists, read it and note the highest ID.
2. Gather decisions from this conversation and the relevant plan or
   spec. Skip any already recorded.
3. Show the proposed new entries to the user and wait for approval.
   The user may cut, edit, or add entries.
4. Write only the approved entries. Append to an existing file; never
   rewrite or renumber existing entries. If an approved entry
   supersedes an old one, add the new entry and mark the old one's
   status as "Superseded by D-NNN".

## Format

```markdown
# Decisions

## D-001: Short title of the decision
Date: YYYY-MM-DD
Status: Active
Decision: What was chosen
Reason: Why, including anything the user said that drove it
Rejected: Alternatives considered, if any
Revisit if: The condition that would justify reopening this
```

"Revisit if" matters most: it tells reviewers what counts as new
evidence. Make it concrete (a metric, a user need, a failure), not
"if requirements change".
