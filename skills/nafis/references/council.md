# Council contract

Shared by the router and the three members. The router reads it at the start of every
run; each member reads it before its cards. It holds the ground rules, the words the
council thinks with, the evidence rules, the severity scale, how cards are used, the
route names, and the two message formats between the router and the members.

## Ground rules

- **Diagnose; the fix step edits.** Members read, run, render and query. They change no
  code, data, config or anything a customer receives. A check that needs something
  written first (a test, a fixture, worst-case demo data, a prototype) becomes an
  evidence gap that names its route. The router changes the product only in the fix
  step of a review; while it plays a role inline, that role's rules bind it.
- **Rules order.** The repo's own agent-rules file, named in the profile, wins over any
  card, criterion or Move. The profile's house rules govern every data source.
- **Cards are the knowledge.** Questions, criteria, severity and every Move come from
  the cards and this file. Card sources are never fetched; a `From:` line is
  provenance only.
- **Helpers are optional.** Nafis has no required dependencies. A helper skill is used
  only when it is resolved as available for this run (members: the dispatch's
  `helpers` line; router: `routes.md`). Without one, do the step yourself with the
  cards: gather the evidence, apply the Move, or ask the question directly.
- **Plain search.** Search the repo with the session's own tools (grep, glob, file
  reads, git log, diff and blame); Nafis needs no index or graph tooling of its own.
  Tool limits in the profile's house rules apply.
- **Read widely; keep real data out of files.** Reading has no default limits: use the
  profile's sources and any other source that helps (support history, conversations,
  analytics, logs, database reads). When a source refuses access, report the refusal
  and continue with the other evidence; never route around it. Real customer data may
  appear in a chat report only as the house rules allow, and never in a file: not in
  briefs, reviews, code, fixtures or commits. Describe real extremes by their shape
  (length, script, count) with synthetic values.
- **Captures stay in scratch.** Screenshots and other captures go to
  `${TMPDIR:-/tmp}/nafis/<profile>/`, outside every repo and outside `~/.council`.
  Reports cite them by their conditions (screen, width, locale, theme, state).

## Words

- **End user, caller, operator.** The three people a change can reach. The end user
  feels outcomes (replies, notifications, prices, latency). The caller (a frontend,
  another service, an AI tool) feels the interface. The operator (on-call developer,
  support) feels errors, logs and recovery. In backend work "the user" means all three.
- **Target.** `ui` or `backend`, taken from the profile's path → target map;
  `ui+backend` loads both rulebooks.
- **Promises.** What the feature's `brief.md` committed to: Cooper's job, no-gos and
  metric; Tesler's step count and defaults; Norman's states and feedback. A review
  judges the work against them.
- **Finding.** One problem, with evidence, a severity, a Move and a route.
- **Evidence.** Something a reader can re-check: a screenshot with its conditions, a
  `file:line`, or data (the source, the query or filter, the count).
- **Move.** The card's fix direction made concrete for this element. A Move is enough
  to act on without any helper skill.
- **Route.** The kind of fix or step a finding needs (Routes below); `user` marks a
  product decision.
- **Cut list.** Cooper's elements that serve nobody's goal. The user decides each one.
- **Content ranges.** The real extremes a surface must carry (longest names, largest
  collections, empty accounts), found by Cooper in real data and recorded as shapes.
  Norman uses them for worst-case checks.
- **Evidence gap.** What was not checked, why, and what would check it. Unchecked is
  never reported as passed.

## Evidence rules

- Every finding carries evidence. The router drops a finding without it.
- At most five findings per member: the five most severe. When more exist, add one line
  with their count and highest severity.
- Every finding names the exact element, line, record or message and the person it
  hurts. Advice that would fit any product is no finding.
- A missing thing is a finding only once checked: cite the code path or the render
  where it is absent.

## Severity

The labels are exactly `shit`, `bad` and `nice`.

| Label | `ui`: for the person | `backend` | Verdict |
|---|---|---|---|
| `shit` | cannot finish the task, loses data, or does the wrong thing (money, messages to customers) | data lost or corrupted; money wrong; a wrong message reaches a customer; a failure is silent; a contract callers rely on breaks | ⛔ stop |
| `bad` | finishes, but struggles, makes mistakes, or writes to support | callers must know a hidden rule; one rule lives in N places; an unclear error costs developer and support time | ⚠️ fix |
| `nice` | polish only | naming; small cleanup that costs nobody anything today | ✅ pass |

A card that marks a specific smell as `shit` overrides the default. Judge by the
outcome for the person, the caller or the operator, not by how the code looks.

## Cards

```
### <Leading word> · <ui | backend | both>
**Ask:** the critical question
**Smell:** how the problem shows up in a design, plan or diff
**Move:** the fix direction
**From:** provenance, for maintainers
```

The leading word is a law, principle or pattern name; it recalls the whole idea.
Each role prompt (`agents/<member>.md`) says which card files its target loads: `ui`
loads `ui` and `both` cards, `backend` loads `backend` and `both`, `ui+backend` loads
all of that member's.

- **Plan mode:** put each relevant **Ask** to the input and the brief. An Ask that
  neither the evidence nor the input answers becomes a question for the user, or an
  open item in the brief.
- **Review mode:** hunt each **Smell** in the evidence (screens, diff, data, logs). A
  hit with evidence becomes a finding; its **Move**, made concrete, is the fix.

## Routes

Route names used in findings and plan steps. `routes.md` maps them to optional helper
skills; every route also has a fallback that needs no helper.

| Route | Kind | Covers |
|---|---|---|
| `interview` | plan | questioning the user when a brief is thin; recording decisions |
| `terms` | plan, fix | domain terms, glossary, names |
| `shape` | plan | a design direction for a `ui` brief |
| `spec` | plan | turning a brief into a spec |
| `questionnaire` | plan | questions for a decision someone else owns |
| `multi-session` | plan | work that spans several sessions |
| `prototype` | plan, evidence | a throwaway build that tests a flow (writes; router-run) |
| `critique` | evidence | a heuristic UX review of a screen |
| `audit` | evidence | accessibility, performance and responsive checks |
| `worst-case` | evidence | worst-case data rendered or tested (writes; router-run) |
| `mobile` | evidence | native feel on phones |
| `motion` | evidence | motion and feedback quality |
| `standards` | evidence | the repo's coding standards |
| `diagnose` | evidence | tracing a failure to its cause |
| `distill` | fix | cutting and simplifying |
| `onboard` | fix | first run, empty states, help at the moment of action |
| `harden` | fix | states, errors, edge cases, forgiving input |
| `clarify` | fix | copy, labels, messages, error texts |
| `layout` | fix | grouping, hierarchy, spacing, type |
| `polish` | fix | final polish of `nice` items |
| `architecture` | fix | module depth, locality, seams |
| `test-first` | fix | a fix behind a failing test: validation, errors, idempotency, worst-case input |
| `user` | decision | a product decision the user makes |

## Dispatch (router → member)

The router starts each member with one block (`SKILL.md` → Starting a council member).
Paths in the skill are relative to the folder on the `skill` line.

```
NAFIS DISPATCH
member: cooper | tesler | norman
mode: plan | review | quick
target: ui | backend | ui+backend
skill: <absolute path of the Nafis skill folder>
profile: <absolute path of profile.md>
feature: <absolute path of the feature folder> | none
input: <TZ text or path | diff base..head and changed paths | URLs | screenshot paths | message templates>
in working tree: yes | no
helpers: <route: skill, ...> | none
cut list: <from Cooper> | none
content ranges: <from Cooper> | none
shape output: <path> | none
user notes: <anything the user said that narrows the run> | none
```

`quick` is Norman alone on copy, colour or one control: only the changed element, its
states and the sweep.

## Reports (member → router)

Members reply with exactly one report and nothing after it. Sections with nothing to
say read `none`.

### Review report

```
## <Member> · <review | quick> · <target>
Verdict: ⛔ | ⚠️ | ✅
Checked: <screens with width · locale · theme · state; files; sources and queries>
Promises: job <kept | broken>, no-gos <kept | broken>, metric <measurable | not>   (Cooper, when a brief exists)

Criteria
1. <criterion>: yes | no | unchecked · <evidence reference>
2. …
5. …

Findings
F1 · <shit | bad | nice> · <card leading word>
  Where: <file:line | screen · width · locale · theme · state | source + query>
  Evidence: <what was observed, re-checkable>
  Hurts: <who, and how>
  Move: <the concrete fix>
  Route: <route | user>
F2 …   (at most five)

Cut list   (Cooper)
- <element>: <why it serves no one's goal>

Content ranges   (Cooper)
- <field or collection>: <shape of the real extreme>

Evidence gaps
- <what is unchecked> · <why> · <what would check it: a route or a source>
```

The verdict is the worst severity among the findings: any `shit` → ⛔, else any
`bad` → ⚠️, else ✅.

### Plan report

```
## <Member> · plan · <target>
Recommendation: build | narrow | stop · <one line why>   (Cooper)

Answers
1. <plan question>: <answer> · source: <reference> | OPEN
…

Questions for the user
Q1. <question> · proposed: <answer the evidence supports, or none> · matters because: <one line>

Brief section
<this member's block of the brief template, each field filled or OPEN>

Risks
- <shit | bad | nice> · <card leading word> · <the smell found in the plan> · Move: <direction>

Content ranges   (Cooper)
- <field or collection>: <shape of the real extreme>

Evidence gaps
- <what is unchecked> · <why> · <what would check it>
```
