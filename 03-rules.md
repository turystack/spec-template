# Rules — the ones that cross domains

## Shape

Most rules belong to one entity and live with its domain. This section is for
the rest: the ones no single domain owns, and that therefore get re-implemented
differently in each place that needs them.

| ID | Law | Class | Gate |
|---|---|---|---|
| SPC-8 | A cross-domain rule is written once, here, with an id, and cited by that id from the code that enforces it. | constitutional | `manual` |
| SPC-9 | A rule with more than two conditions is written as a decision table, never as prose. | constitutional | `manual` |

**Where a rule belongs:**

```text
touches one entity only          → that entity's domain (02-domains.md)
touches one capability only      → that capability (01-definition.md)
touches two or more domains      → here
is about money, time or access   → here, almost always
```

Money, time and access are listed because they look local and never are: a
refund window is an order rule until billing needs it, then support needs it,
and by then there are three windows.

**What a filled rule looks like:**

```markdown
### RULE-4 · Refund window

Applies to: orders, billing, support
Statement: an order may be cancelled with a full refund within 72 hours of
payment confirmation.

| Elapsed | Order state | Result |
|---|---|---|
| ≤ 72h | placed | full refund |
| ≤ 72h | shipped | full refund minus shipping |
| > 72h | placed | no refund; cancellation still allowed |
| > 72h | shipped | refused; the customer returns instead |

Time is measured from payment confirmation, in the organization's timezone,
against an instant the operation receives (`ARC-TOP-7`).
```

Three parts make it usable: **who it applies to**, **one sentence anyone can
read**, and **a table for the combinations**. `SPC-9` exists because prose with
four conditions is prose two people read differently, and both are sure they
are right.

**The timezone line is not decoration.** A rule about time that does not say
whose time behaves differently per deploy region. Writing it here is what lets
the backend inject the right instant instead of reading the runtime clock.

## {{PROJECT}}

<!-- turystack:unfilled -->
> ⛔ **Not filled in.** No cross-domain rules have been written for
> {{PROJECT}} — which may be true, or may mean nobody looked. A task that finds
> the same rule in two domains stops here and adds it, rather than implementing
> it twice.
>
> Replace this block with the rules, each with an id.

## Never do

- Copying a cross-domain rule into a second section "for convenience"
  (`SPC-8`).
- A four-condition rule written as a paragraph (`SPC-9`).
- A rule about time with no timezone and no source for "now".
- Enforcing a rule in code with no id to point back at.
