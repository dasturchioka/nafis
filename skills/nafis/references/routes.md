# Routes: helpers and fallbacks

For the router. Read it when you resolve a route: when you list a run's evidence helpers
for the dispatch, and when you reach a plan step or a fix. Route names and what each
covers: `council.md` → Routes.

Every helper here is an optional suggestion. Nafis runs fully with none of them: the
cards carry the knowledge, and each route has a fallback you carry out yourself.

## Resolve a route

1. **Candidates.** When the profile has a Helper skills section, it is the whole
   allowlist: the route's candidates are its entry there, and a route it leaves out has
   none. Without that section, the candidates are this file's suggestions for the
   target kind, in order.
2. **Available.** Keep the first candidate whose skill name is in the session's
   available-skill list. An entry such as `impeccable shape` means the skill
   `impeccable` with the argument `shape`.
3. **Run it.** Invoke the skill with its argument plus a brief: for a fix, the finding's
   text, evidence, Move and files; for a plan step, the brief path and the open items.
   When the skill refuses model invocation, give the user the exact command and the
   brief, and carry on with the rest.
4. **Fallback.** No available candidate:

| Kind | Fallback |
|---|---|
| evidence | The member gathers the evidence itself (code, renders, data) and applies its cards. |
| evidence that writes: `worst-case`, `prototype` | Do it yourself on the user's yes: render the content ranges with synthetic values, or write the throwaway test or prototype, outside the shipped code where you can. |
| plan: `interview` | Ask the questions yourself, in one message, with the members' proposed answers as defaults. |
| plan: `shape` | Skip it; Tesler and Norman plan from Cooper's section. |
| plan: `terms`, `spec`, `questionnaire`, `multi-session` | Do the small part yourself (add the term to the brief, list the decision owner) and name the larger step as the next step in the report. |
| fix | Apply the finding's Move directly when the code is in this working tree, following the repo rules file. Otherwise report the finding with its Move to the user. |

## Suggestions

Optional integrations. A dash means no suggestion; the fallback applies.

| Route | `ui` | `backend` |
|---|---|---|
| `interview` | `grilling` | `grill-with-docs` |
| `terms` | `domain-modeling` | `domain-modeling` |
| `shape` | `impeccable shape` | - |
| `spec` | `to-spec` | `to-spec` |
| `questionnaire` | `to-questionnaire` | `to-questionnaire` |
| `multi-session` | `wayfinder` | `wayfinder` |
| `prototype` | `prototype` | `prototype` |
| `critique` | `impeccable critique` | - |
| `audit` | `impeccable audit` | - |
| `worst-case` | `break-ui` | `tdd` |
| `mobile` | `mobile-native` | - |
| `motion` | `review-animations` | - |
| `standards` | `code-review` | `code-review` |
| `diagnose` | `diagnosing-bugs` | `diagnosing-bugs` |
| `distill` | `impeccable distill` | - |
| `onboard` | `impeccable onboard` | - |
| `harden` | `impeccable harden` | - |
| `clarify` | `impeccable clarify` | - |
| `layout` | `impeccable layout`; `impeccable typeset` for type findings | - |
| `polish` | `emil-design-eng` | - |
| `architecture` | - | `codebase-design`, `improve-codebase-architecture` |
| `test-first` | `tdd` | `tdd` |

The suggestions come from three skill sets, each optional: impeccable (UI design),
Emil Kowalski's skills (`break-ui`, `mobile-native`, `review-animations`,
`emil-design-eng`) and Matt Pocock's skills (the rest).

## Helpers in the dispatch

Before dispatching, resolve the read-only evidence routes (`critique`, `audit`,
`mobile`, `motion`, `standards`, `diagnose`) for the run's target and list the
available ones on the dispatch's `helpers` line (`critique: impeccable critique, …`),
or `none`. Members use only the read-only parts of what that line names; a helper step
that writes becomes an evidence gap.

`worst-case` and `prototype` write, so they never go on that line. A member that needs
one names it as an evidence gap; offer it in the review's decision message, run it on
the user's yes (helper or fallback above), then start the member that named the gap
again, in review mode, with the new evidence as its input.
