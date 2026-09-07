---
name: "{{PROJECT}}-spec"
description: "What {{PROJECT}} does — its definition, actors and capabilities, its domains with their entities, states and legal transitions, the rules that cross domains, the end-to-end flows, and the glossary all of them are written in. Read it before implementing anything in {{PROJECT}}: the product's own decisions live here, and a task that needs one looks here first. Use it when a spec is missing, ambiguous or silent on an unhappy path; when adding an entity, a state or a transition; when a rule touches more than one domain; when a term means different things to different people; and whenever you need to know what should happen rather than how to write it. Every decision here carries a permanent id, and the board in `board/` says which task implements each one and holds the delivery report that proved it. A section still carrying the unfilled marker is a decision nobody has made — it stops the task and becomes a question for a person, never a guess. {{PROJECT}}-uiux owns how it looks and sounds; the turystack-* pattern skills own how the code is written."
---

# {{PROJECT}}-spec

Everything {{PROJECT}} decided about **what it does**. This skill is the
project's own; it was materialized from `@turystack/spec-template` and it is
yours to fill and keep.

## How this skill works

Each section has two halves:

```text
Shape        what this document must answer, and what a good answer looks like.
             Comes from the template. Leave it — it is what keeps the next
             person's answer in the same form as yours.

{{PROJECT}}  your content. Starts unfilled. You replace the marker.
```

An unfilled section is not a gap to be improvised over. It is a decision nobody
has made yet, and `turystack-proof-mode` treats it exactly that way: the task
stops on the part that needs it and asks a person.

```markdown
<!-- turystack:unfilled -->
```

That marker is machine-readable. `turystack-proof` reads it, so "the spec is
missing" is a gate result, not an opinion.

## Routing

| You need | Read |
|---|---|
| What {{PROJECT}} is, who uses it, what it can do | `01-definition.md` |
| A domain's entities, states and legal transitions | `02-domains.md` |
| A rule that touches more than one domain | `03-rules.md` |
| A flow from trigger to outcome, across domains | `04-flows.md` |
| What a term means, exactly, and what it never means | `05-glossary.md` |
| How to fill a section, and when to create one | `06-filling.md` |
| What work this spec turned into, and where a task's report is | `07-board.md` |

Read `05-glossary.md` first, once. Everything else uses its words.

## What never goes here

| Belongs to | Not here |
|---|---|
| `turystack-architecture-pattern` | architectural law — layers, consistency, idempotency |
| `turystack-backend-pattern` / `-frontend-pattern` | how the code is written |
| `{{PROJECT}}-uiux` | how it looks, how it is worded, which component |
| the generated SDK | the API contract's shape |

The line is: this skill decides **what happens**, never **how it is built** and
never **how it appears**.

## Before you rely on a section

1. **Filled.** Is the section you need actually filled, or does it still carry
   the marker? (`SPC-1`)
2. **Ownership.** Does exactly one section own this decision, or did you find it
   in two? (`SPC-2`)
3. **Unhappy paths.** Does the capability say what happens when the actor is not
   allowed, the thing is not found, and the action is already done? (`SPC-5`)
4. **Transitions.** Is the change of state you need in the transition table, or
   are you inferring it? (`SPC-6`)
5. **Language.** Is the term in `05-glossary.md`, with the meaning you are
   assuming? (`SPC-11`)

## Ownership rule

This skill answers **what {{PROJECT}} does**. `{{PROJECT}}-uiux` answers **how
it looks and sounds**. The constitution answers **which architectural law
applies**. The pattern skills answer **how the code is written**. When a
decision could live in two of them, it lives in the one closest to the product
and is cited from the others.
