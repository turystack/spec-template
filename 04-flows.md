# Flows — end to end, across domains

## Shape

A capability says what one action does. A flow says what happens after it:
which reactions fire, which are awaited, and what the user is shown while the
rest of the system catches up.

| ID | Law | Class | Gate |
|---|---|---|---|
| SPC-10 | A flow states which steps are synchronous and which are reactions, because that decides what the user is shown at the end of the first step. | constitutional | `manual` |
| SPC-20 | A published event carries an id and lists every handler that reacts to it; a schedule carries an id and says what it does when it fails. Neither is discovered by reading the code that happens to subscribe. | constitutional | `manual` |

**What a filled flow looks like:**

```markdown
### Cancelling an order

Trigger: an operator confirms cancellation.

| # | Step | Domain | Sync? | On failure |
|---|---|---|---|---|
| 1 | order transitions to cancelled | orders | sync | nothing happened; the user sees the error |
| 2 | shipments released | shipping | sync, same transaction | rolls back with step 1 |
| 3 | `order.cancelled` published | orders | after commit | — |
| 4 | refund requested | billing | reaction | retried; failure alerts, order stays cancelled |
| 5 | customer notified | messaging | reaction | retried; failure is not user visible |

**What the operator sees:** the confirmation returns after step 2. Steps 4 and 5
are not awaited — the order is cancelled whether or not the refund provider is
reachable.

**Compensation:** step 4 has none. A failed refund does not un-cancel the order;
it raises an operational alert and stays queued.
```

**What the flow publishes, and what reacts to it:**

```markdown
| Event | Id | Published when | Handled by | Effect | On failure |
|---|---|---|---|---|---|
| order.cancelled | EVN-3 | the transition commits | billing | requests the refund | retried; alerts after 3, order stays cancelled |
| order.cancelled | EVN-3 | the transition commits | messaging | notifies the customer | retried; not user visible |

| Schedule | Id | Runs | Does | On failure |
|---|---|---|---|---|
| reservation sweep | JOB-1 | every 10 minutes | releases reservations older than 24h | retried; alerts after 3 |
```

**Why the handlers are listed with the event.** `SPC-20` exists because a
handler is invisible from every side but its own: the flow says an event is
published, the code that subscribes lives in another module, and the only place
the two meet is here. A handler nobody wrote down is a side effect discovered
during an incident.

**Why a schedule is a decision and not a detail.** It writes without a user,
usually to rows somebody else is editing, and it does so on a clock nobody is
watching. What it may touch, and what happens when it fails, are product
decisions — the retry policy and the decorator that implements them are not.

**Why the sync column decides the UI.** It is the most consequential column in
the document: it determines whether the frontend can say "cancelled and
refunded" or must say "cancelled; the refund is on its way". Getting it wrong
produces a message that is false for a few seconds — exactly long enough for a
support ticket. It also decides the backend's consistency strategy
(`ARC-CON-1`).

**Ordering is not guaranteed.** Reactions can arrive twice and out of order
(`ARC-IDM-1`, `ARC-IDM-3`). A flow must not promise an order it cannot deliver.

**Asking what a flow leaves open** — name the branch and the cost of each
answer, so a non-engineer can decide in one message:

```text
"Refund fails after the order is already cancelled: does the operator see
 anything? (a) nothing, alert only — no UI work; (b) a banner on the order —
 needs a refund state on the read model. (b) changes the contract, so it is
 worth deciding now."
```

## {{PROJECT}}

<!-- turystack:unfilled -->
> ⛔ **Not filled in.** {{PROJECT}} has no flows written down. A task whose
> effect crosses a domain boundary stops here and asks what the user is shown
> and what happens when a later step fails.
>
> Replace this block with one section per flow.

## Never do

- A flow that lists steps without saying which are awaited (`SPC-10`).
- An event with no id, or an event whose handlers live only in the code that
  subscribes to it (`SPC-20`).
- A step that can fail after a commit with no stated compensation — and no
  statement that there is none.
- Promising ordering between two reactions.
- Describing the queue, the retry policy or the decorator here — that is
  `turystack-backend-pattern`.
