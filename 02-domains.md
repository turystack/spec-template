# Domains — what exists and how it changes

## Shape

One block per domain. Three questions an implementer asks constantly: what
entities exist, what states they can be in, and which transitions are legal.

| ID | Law | Class | Gate |
|---|---|---|---|
| SPC-6 | Every stateful entity has an explicit transition table. A transition absent from it is illegal, not undefined. | constitutional | `gate:transition-table` |
| SPC-7 | Every entity declares its identity — the fields another domain may reference — separately from its full shape. | constitutional | `manual` |

**What a filled domain looks like:**

```markdown
### Orders

| Entity | Is | Identity |
|---|---|---|
| Order | a customer's purchase | orderId, code |
| Shipment | a reserved dispatch | shipmentId, trackingCode |

**Order states**

| State | Means | Terminal |
|---|---|---|
| placed | paid, awaiting fulfilment | no |
| shipped | handed to the carrier | no |
| delivered | received | yes |
| cancelled | stopped before delivery | yes |

**Transitions**

| From | To | Trigger | Guard |
|---|---|---|---|
| placed | shipped | fulfilment dispatches | every item reserved |
| placed | cancelled | operator cancels | within the refund window |
| shipped | cancelled | operator cancels | carrier accepts recall |
| shipped | delivered | carrier confirms | — |

Anything not in this table cannot happen. `delivered → cancelled` is absent, so
it is illegal — a delivered order is returned, which is a different capability.

**Invariants**

| Rule | Why |
|---|---|
| An order always has at least one item once placed | an empty order cannot be fulfilled |
```

**Why the transition table is the important part.** Three things read it
directly and none can be correct without it: the entity guard that refuses an
illegal transition, the actions a surface offers and which are inert, and the
tests — which need to know what must fail, not only what must work. A domain
that lists states but not transitions pushes that decision onto whoever
implements first, and the second implementer disagrees.

**Identity versus shape.** `SPC-7` mirrors `ARC-CTR-3`: another domain
references an entity by its identity — the key plus what makes it recognisable
to a human — never by embedding the whole entity.

## {{PROJECT}}

<!-- turystack:unfilled -->
> ⛔ **Not filled in.** {{PROJECT}} has no domains written down. A task that
> needs an entity's states, a legal transition or an identity stops here and
> asks.
>
> Replace this block with one section per domain, in the shape above.

## Never do

- States with no transition table (`SPC-6`).
- Adding a transition in code that the table does not contain (`SPC-6`).
- Letting another domain reference an entity's full shape (`SPC-7`).
- Describing tables, columns or indexes here — that is
  `turystack-backend-pattern` › `05-repositories.md`.
