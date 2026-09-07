# Glossary — one term, one meaning

## Shape

The shortest section and the one that saves the most rework. Every other
section, every schema field and every UI label draws from it.

| ID | Law | Class | Gate |
|---|---|---|---|
| SPC-11 | A term has exactly one meaning in {{PROJECT}}, recorded here, and the code uses that word for that thing. | constitutional | `manual` |
| SPC-12 | A term records what it is **not**, whenever a near-synonym exists. | constitutional | `manual` |

**What a filled glossary looks like:**

```markdown
| Term | Means | Not |
|---|---|---|
| Order | a customer's purchase, from placed to delivered | not a Shipment, not an Invoice |
| Shipment | one dispatch of items belonging to an order | not the order; an order may have several |
| Cancel | stop an order before delivery | not Return, which happens after delivery |
| Return | send back a delivered order | not Cancel |
| Organization | the tenant that owns the data | not Workspace, which is a subdivision |
| Workspace | a subdivision inside an organization | not Organization |
```

**The Not column carries most of the value.** "Order means a purchase" prevents
nothing. "Order is not a Shipment; an order may have several" prevents the model
where someone adds `trackingCode` to `Order` because in that conversation there
was only one shipment.

**Why this is architecture-adjacent.** The glossary is what makes `ARC-CTR-1`
mean something: a contract declared once is only single-source if everyone
agrees what the field is called and what it holds. Two names for one concept
produce two schemas, and neither looks wrong on its own.

It is also the copy source for `{{PROJECT}}-uiux`: labels and empty states use
the glossary's word, not a synonym that reads better on one screen.

**When to add a term.** When it first appears in a section — not when someone
gets confused. The row costs a minute; discovering the ambiguity inside a schema
costs a migration.

## {{PROJECT}}

<!-- turystack:unfilled -->
> ⛔ **Not filled in.** {{PROJECT}} has no glossary. This is the section to fill
> first: six to ten terms, with the Not column. Every other section and every
> schema field depends on it.
>
> Replace this block with the table.

## Never do

- Two words for one concept, live at the same time (`SPC-11`).
- A definition that is a synonym of itself ("an Order is an order").
- Skipping the **Not** column when a near-synonym exists (`SPC-12`).
- Naming a schema field or a UI label with a word that is not in here.
