# Board — the work this spec turns into

## Shape

The tasks {{PROJECT}} is built from, derived from the sections above and kept in
this skill. State in one file, a page that renders it, and the delivery report
of every finished task reachable from the task that asked for it.

| ID | Law | Class | Gate |
|---|---|---|---|
| SPC-16 | The board is this skill's own: `board/tasks.json` holds the state and `board/template.html` renders it. The page is never the place a change is recorded. | constitutional | `gate:board-in-sync` |
| SPC-17 | Every task names the ids it satisfies, and every id this skill declares is named by some task. A task with no id is not delivery; an id with no task is work nobody scheduled. | constitutional | `gate:board-covers-spec` |
| SPC-18 | A task is done when the delivery report that proved it is attached to it. A status set by hand, with no report, is a claim. | constitutional | `gate:board-report-linked` |
| SPC-21 | A task carries its test cases before it carries any code. Each case has an id, says what it proves, and names the level it is proven at. | constitutional | `gate:task-cases` |
| SPC-22 | Cases are written by whoever plans the task and **approved by a person** before implementation starts. A case a person added is worth the same as one that was proposed; a case nobody answered is not a case yet. | constitutional | `gate:cases-approved` |
| SPC-23 | The example board is `board/template.html` with a payload in it. The two never differ outside that payload — a second copy of the page drifts, and the copy that drifts is always the one shipped as documentation. | constitutional | `gate:example-page-current` |

**Layout:**

```text
{{PROJECT}}-spec/
├── 07-board.md            this file: what a task is, and the rules about it
└── board/
    ├── tasks.json         the state — the only file a change is written to
    ├── template.html      the page — a list on the left, the task on the right
    ├── example.json       a filled board, for reading
    ├── example.html       that board, rendered — the template with data in it
    └── reports/           where a finished task's report lands
```

`example.*` is documentation: a board with six tasks in every state, so the
shape above can be read rather than imagined. Delete it, or keep it — nothing
reads it but a person.

**What a task looks like:**

```jsonc
{
  "id": "T-4",
  "title": "Cancel an order",
  "surface": "Orders table",           // null when the task has no screen
  "system": "internal",                // the design system, when there is one
  "covers": ["AC-1", "AC-2", "AC-4"],  // the ids this task satisfies
  "backend": "the transition, its guard, the endpoint, the released shipments",
  "frontend": "the row action, its permission gate, the confirm with the count",
  "status": "done",                    // open · doing · done · blocked
  "report": "reports/T-4/report.html", // written by the delivery harness
  "blockedBy": null,

  // Written before any code, read by a person, and only then implemented.
  "cases": [
    { "id": "TC-1", "proves": "AC-1", "level": "unit",
      "text": "a paid order transitions to cancelled and releases its shipments",
      "state": "approved" },
    { "id": "TC-2", "proves": "AC-2", "level": "unit",
      "text": "cancelling an already cancelled order changes nothing and says why",
      "state": "approved" },
    { "id": "TC-3", "proves": "AC-1", "level": "e2e",
      "text": "the endpoint returns the released shipments in the response",
      "state": "approved" },
    { "id": "TC-4", "proves": null, "level": "integration",
      "text": "two cancellations racing leave one cancellation and one refusal",
      "state": "approved", "addedBy": "joaogabriel" }
  ],
  "casesApprovedBy": "joaogabriel",
  "casesApprovedAt": "2026-08-20"
}
```

**Why the example is generated and not written.** `SPC-23` exists because two
copies of one page drift in a predictable direction: the page somebody opens
every day gets fixed, and the one shipped as an example keeps the bug — until a
reader builds something from a board the product no longer has. So the example
is the template with a payload swapped in, and the gate compares everything
outside that payload.

**Why the state is a file and not the page.** `SPC-16` exists because a page
that is edited by hand is a page whose history is a diff of markup. The page
carries a copy of the state so it opens with no server — the same arrangement
the delivery report uses — and that copy is a **render**: written from
`tasks.json` in the same step, never the other way around.

**Why `covers` is the field that matters.** It is the only thing that makes "is
this implemented?" answerable without opening the code. A request arrives as a
sentence, a test proves something specific, and the id is what survives the trip
between them:

```text
"deixa o operador cancelar um pedido"
    → capability: Cancel an order  →  AC-1 … AC-4
    → task T-4 covers AC-1, AC-2, AC-4 · task T-5 covers AC-3
    → each test cites the id it proves
    → each report lists the id beside that test
```

**Both directions are checked**, and each catches a different mistake: an id no
task names is a decision nobody scheduled — usually an unhappy path — and a task
naming no id is either work nobody asked for or an id that was never written
down.

**Why the cases come first.** `SPC-21` is the one thing in this file a
non-engineer can check. A spec says what should happen; a case says what will be
run to find out — and the gap between those two is where a task quietly ships
half of what was asked for:

```text
AC-2  an already cancelled order is refused, and the refusal says so

TC-2  cancelling an already cancelled order changes nothing and says why   unit
TC-5  the refusal reaches the operator as inline text, not a toast         component
```

`AC-2` alone leaves the second row to whoever implements it, at the end of the
day. Written as cases, both are on the page before there is any code to defend.

**Reading them is the point, and so is adding to them.** `SPC-22` makes the
approval a real step: a person reads the list, deletes what is not worth a test,
corrects what was misunderstood, and — the valuable half — **adds the cases only
they know about**, like `TC-4` above. The race nobody modelled, the customer who
has two organizations, the import that ran twice last March.

A case someone added is not a lesser case. Once approved, the list is one list,
and every entry on it has to be proved:

```text
proposed   the agent wrote it, nobody has read it → the task cannot start
approved   a person read it, or wrote it themselves → it must end up in a test
```

A task can sit `blocked` with its cases unread — it blocks for reasons that
happen earlier, like a spec that turned out to be silent. What it cannot do is
reach `doing`.

**The report is the status.** `SPC-18` makes `done` mean one thing. A task
without an attached report may be finished, and nobody can tell it apart from
one whose gates were never run.

## {{PROJECT}}

<!-- turystack:unfilled -->
> ⛔ **Not filled in.** {{PROJECT}} has no board. Work will be tracked from
> memory, and "what is left" will be answered differently each time it is
> asked.
>
> Derive it from the sections above — `turystack-harness` › `05-board.md` owns
> that step — and write it to `board/tasks.json`. Record here anything
> {{PROJECT}} decides about its own board that the shape above does not: how
> tasks are ordered, who may change a status, where the reports are kept.

## Never do

- Recording a change by editing the page (`SPC-16`).
- Editing `example.html` by hand instead of regenerating it from the template
  (`SPC-23`).
- A task that names no id (`SPC-17`).
- An id that no task names, because it was an unhappy path (`SPC-17`).
- `done` with no report attached (`SPC-18`).
- Writing the cases after the code, from what the code happens to do
  (`SPC-21`).
- Starting a task whose cases nobody has read (`SPC-22`).
- Dropping a case a person added because it turned out to be inconvenient to
  prove (`SPC-22`).
- Rewriting a delivered task's ids so a later spec change comes out green — the
  report proved what it proved on the day it ran.
- Keeping the board in a tracker the repository cannot check.
