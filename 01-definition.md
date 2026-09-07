# Definition — what {{PROJECT}} is

## Shape

One page that lets someone decide in five minutes whether a request belongs to
this project at all: who uses it, what they can do, and what it deliberately
does not do.

| ID | Law | Class | Gate |
|---|---|---|---|
| SPC-4 | Every capability names its actor and its outcome; a capability with no actor is a feature nobody asked for. | constitutional | `manual` |
| SPC-5 | A capability states its unhappy paths: not allowed, not found, already done, and blocked by state. | constitutional | `gate:capability-unhappy-paths` |
| SPC-15 | Every acceptance criterion carries an id (`AC-n`) and is written Given / When / Then. A request arrives in someone's own words; the ids are what turn it into something a test can cite. | constitutional | `gate:acceptance-ids` |

**What a filled definition looks like:**

```markdown
## What it is
Two or three sentences. What problem, for whom.

## Actors
| Actor | Is | Can |
|---|---|---|
| Operator | staff who fulfil orders | read every order, cancel, refund |
| Customer | the person who bought | read their own orders, request cancellation |

## Capabilities
| Capability | Actor | Outcome | Detailed in |
|---|---|---|---|
| Cancel an order | Operator | order stops being fulfilled, shipments released | below |

## Out of scope
What this project deliberately does not do, and where that lives instead.
```

**What a capability looks like**, and why the unhappy paths table is the part
that matters — those four rows are the ones a task discovers at implementation
time, when the person who could answer them has moved on:

```markdown
### Cancel an order

Actor and trigger: an operator, from the orders table or the order detail.
Preconditions: the order exists, belongs to the operator's organization, is not delivered.
Outcome: the order moves to `cancelled`; every reserved shipment is released.

| Situation | What happens |
|---|---|
| Not allowed | the action stays visible, inert, with the reason |
| Not found | the address keeps working; the surface says it is gone |
| Already cancelled | refused, with a message; nothing changes |
| Delivered | refused; a delivered order is returned, not cancelled |

Blast radius: releases the order's reserved shipments. The confirmation shows
how many, read from the order's shipments.

| AC | Given | When | Then |
|---|---|---|---|
| AC-1 | an order in `paid`, owned by the operator's org | the operator confirms the cancellation | the order is `cancelled` and its shipments are released |
| AC-2 | an order already `cancelled` | the operator cancels again | nothing changes, and the refusal says it is already cancelled |
| AC-3 | an operator without `order:cancel` | the orders table renders | the action is visible, inert, and states the reason |
| AC-4 | an order in `delivered` | the operator cancels | refused; the message names return, not cancellation |
```

The **blast radius** line exists because the frontend must show it before the
confirm (`ARC-CON-11`) and cannot invent the list.

## Why the criteria carry ids

Nobody asks for work in acceptance criteria. A request arrives as *"deixa o
operador cancelar um pedido"* — a sentence, in someone's own words, on a
Tuesday. That is the right way to ask, and it is the input the harness is
built to take.

The ids are what happen next. `turystack-proof-mode` resolves that sentence
to the capability it belongs to and lists the `AC-n` it will satisfy, and from
that point the task is anchored to something that does not move:

```text
"deixa o operador cancelar um pedido"
    → Capability: Cancel an order
    → AC-1, AC-2, AC-3, AC-4
    → four tests, each citing the criterion it proves
```

Without ids the same amount of work happens and none of it is checkable: the
delivery report can list tests and specs, and no machine can say whether the
list covers what was asked. The delivery report needs a spec↔test link, and a link
needs two ends that hold still. A sentence does not.

An `AC-n` is Given / When / Then because those three are exactly what a test
needs — arrange, act, assert — and because writing them forces the unhappy
paths into the open while the person who knows the answer is still in the
conversation.

The **out of scope** section is the one people skip and the one that saves the
most time: it ends the fourth conversation about whether this should also do
invoicing.

## {{PROJECT}}

<!-- turystack:unfilled -->
> ⛔ **Not filled in.** {{PROJECT}}'s definition has not been written. A task
> that needs to know what {{PROJECT}} does, who its actors are, or what a
> capability's unhappy paths are stops here and asks — it does not guess.
>
> Replace this block with the sections above, filled for {{PROJECT}}.

## Never do

- A capability with no named actor (`SPC-4`).
- A capability that describes only the successful path (`SPC-5`).
- Deciding an unhappy path in code because this section was silent.
- Writing the wording of a message here; wording belongs to `{{PROJECT}}-uiux`.
- A capability whose criteria have no ids, leaving the delivery report unable to
  say whether what was asked for is what was proved (`SPC-15`).
