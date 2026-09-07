# Filling — how this skill grows

## Shape

How {{PROJECT}} goes from a freshly materialized skill to a filled one, and how
it stays true afterwards. Written for the first day and for the hundredth.

| ID | Law | Class | Gate |
|---|---|---|---|
| SPC-13 | A section is filled when there is something real to write. An unfilled marker is honest; an invented paragraph is not. | constitutional | `gate:spec-unfilled` |
| SPC-14 | A decision answered by a person lands in this skill in the same change, not only in the code or the conversation. | constitutional | `manual` |

**Day one — two sections, in this order, and nothing else:**

```text
1. 05-glossary.md     six to ten terms, with the Not column
2. 01-definition.md   actors, capabilities, out of scope
```

The rest fills as {{PROJECT}} grows into it. A domain gets its block when it has
states worth a table; a capability gets its unhappy paths when someone is about
to implement it; `03-rules.md` fills the first time a rule refuses to belong to
one domain.

`SPC-13` is why the markers stay until then: an invented paragraph reads as
answered, and the next task builds on it without knowing it was a guess.

**The trigger is always a task, never a documentation sprint:**

| The task | Fills or updates |
|---|---|
| implements a capability | its unhappy paths, before the code |
| adds a state or transition | the domain's transition table |
| finds a rule in two domains | a row in `03-rules.md`, and deletes both copies |
| names something new | a glossary row |
| gets an unhappy path answered by a person | that capability's table |

The last row closes the loop with `turystack-proof-mode`: when a missing input is
answered, **the answer lands here**, not only in the code. Otherwise the next
task asks the same question, and the person answers it slightly differently.

**Keeping it honest.** `SPC-14` is the only maintenance rule that works: this
skill changes in the same commit as the behavior. Not the same sprint, not a
follow-up ticket.

**When {{PROJECT}} already exists.** Do not write it all. Start where the pain
is: the glossary, from terms that already caused an argument; the transition
table of the entity with the most states; capabilities as they are next touched.
Reconstructing everything up front documents what someone remembers, which is
not what the code does.

**Updating the shape.** The Shape halves came from `@turystack/spec-template`.
When the template gains a section, this skill does not update itself — it was
materialized once and belongs to {{PROJECT}}. Pull the new shape in by hand, and
only if it is worth it.

## {{PROJECT}}

<!-- turystack:unfilled -->
> ⛔ **Not filled in.** Nothing project-specific is required in this section —
> it is about process. Replace this block with {{PROJECT}}'s own conventions if
> it has any (who reviews a spec change, where design decisions are recorded),
> or delete the marker to declare that the defaults above are the process.

## Never do

- Filling a section with a plausible guess to remove the marker (`SPC-13`).
- Answering a question in chat and leaving this skill unchanged (`SPC-14`).
- Documenting the current implementation and calling it the spec, when the two
  disagree — decide which is wrong first.
- Materializing every section as empty prose so the folder looks complete.
