# @turystack/spec-template

Template for a project's own **spec skill** — what the system does: definition, actors and capabilities, domains with states and transitions, cross-domain rules, flows and glossary. Materialized once per project as `<project>-spec` and owned by that project from then on.

## Installation

```bash
pnpm add -D @turystack/spec-template
turystack skills --claude --skills spec --project acme
```

That materializes `.claude/skills/acme-spec/`, substituting the project's name.
It is **never overwritten** by a later install: the shape came from here, the
content belongs to the project.

## Contents

- [Overview](00-overview.md)
- [Definition — what the project is](01-definition.md)
- [Domains — what exists and how it changes](02-domains.md)
- [Rules — the ones that cross domains](03-rules.md)
- [Flows — end to end, across domains](04-flows.md)
- [Glossary — one term, one meaning](05-glossary.md)
- [Filling — how this skill grows](06-filling.md)
- [Board — the work this spec turns into](07-board.md)
- [Skill manifest](SKILL.md)

## How a section works

Each one has a **Shape** half (from this template, kept) and a **project** half
(starts with `<!-- turystack:unfilled -->`). The marker is machine-readable:
`turystack-proof` reports an unfilled section that a task depends on as a missing
input, so "the spec is silent" is a gate result rather than an opinion.

## Documentation

**https://tury.dev/libs/spec-template**
