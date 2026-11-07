---
name: <profile-name>
paths:
  - <absolute path the profile covers>
---

# <Project> · Nafis profile

Opened <YYYY-MM-DD>. Why: <the problems that made the user open it>.

Everything project-specific lives here; the council reads no project fact from
anywhere else. The repo rules file below wins over any card.

## People

<!-- One line per role: who, their situation, their goal, how often they come, skill level. -->
- <role>: <situation>; <goal>; <daily | weekly | first run>; <skill level>

Backend readers:
- Callers: <frontends, services, AI tools that call this code>
- Operators: <who is on call, who handles support>

## Languages and input

- Languages: <list>
- Regions and formats: <dates, numbers, currencies, time zones>
- Input that must be accepted: <how people really type phone numbers, dates, amounts, names>

## Channels and sweep

- Channels: <web | phone | chat (messages and buttons) | email | other>
- Sweep: widths <list in px>; themes <list>; locales <list>
- See it running: <URL per environment>; test account: <key names only, never values>

## Repo rules and context

- Rules file: <path> (always wins)
- Glossary: <path | none>
- Product context: <paths such as PRODUCT.md, DESIGN.md | none> (pointers, not copies)

## Path → target

First matching row wins; keep a catch-all last.

| Pattern | Target |
|---|---|
| `<glob>` | ui |
| `**` | backend |

## Data sources

| Source | How to reach it | What it shows |
|---|---|---|
| <name> | <tool, command or connection, by key name> | <support history, conversations, analytics, logs, database> |

House rules:
- Refused access: report it and continue with the other evidence; never route around it.
- Real customer data: <may | may not> be quoted in chat reports; it never goes into
  notes, briefs, reviews, code, fixtures or commits.
- Worst-case test data copies the shape of real extremes with synthetic values.
- <any rule specific to a source>

## Helper skills

<!--
Optional; delete this section to let Nafis use any suggested helper the session has
(routes.md → Suggestions). When present, it is the whole allowlist: a route left out
here has no helper and uses its fallback. Value: an installed skill, with an argument
when it takes one (for example `<skill> <argument>`).
-->

| Route | `ui` | `backend` |
|---|---|---|
| <route> | <skill> | <skill> |
