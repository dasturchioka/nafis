# Plan

Steps for `plan <feature>` and single-lens plan runs. You arrive here with
`council.md` and the profile read. The output is the feature's `brief.md`: Cooper's
purpose, Tesler's flow, Norman's states, each field answered or open with an owner.
Steps 2 to 4 run only for the members on the roster.

## 1. Open or continue the feature

- Read the whole input: a TZ, an idea, a brief, a file or an issue.
- Pick the slug (`SKILL.md` → Features). When `features/<slug>/brief.md` exists,
  continue it: plan only the fields still OPEN. Otherwise create the folder with
  `brief.md` and `reviews.md` from the templates.
- Take the target from the paths the work will touch. Before code exists, take it from
  the surfaces the input names (`SKILL.md` → Target).
- Take the roster from the task size: new work → all three; an improvement to an
  existing surface → Tesler + Norman, with Cooper's block marked `not run: improvement
  of an existing surface`. A single-lens command fixes the roster.

Done when: the feature folder exists and you have stated its target and roster.

## 2. Cooper: purpose, the gate

1. Start Cooper (`mode: plan`; `SKILL.md` → Starting a council member) with the
   input and the feature.
2. Put his questions to the user:
   - `backend` targets → through the `interview` route, which also records decisions
     and terms;
   - `ui` targets → in one numbered message, each question with Cooper's proposed
     answer as its default. When the user cannot answer the job, user or metric
     questions, run `interview` on them instead of guessing.
   In the same message say what comes next: shape (when available), then Tesler and
   Norman; the user may say "skip shape".
3. Write Cooper's block of `brief.md` from his brief section and the answers, and put
   the kill metric on the brief's top line.
4. When Cooper recommends `stop` (an existing surface already serves the job, the
   problem does not recur, or no metric can be named), give the user his reasons and
   continue only on their word.

Done when: every field of Cooper's block is answered, OPEN with who decides, or
declined by the user and recorded as declined.

## 3. Shape (`ui` targets)

Unless the user said "skip shape", resolve the `shape` route (`routes.md`) and run it
with `brief.md` as input; Cooper's block already answers its purpose, people and
outcome round. Link its output under the brief's Links. Without a helper, skip this
step.

## 4. Tesler and Norman: flow and states

1. Start both (`mode: plan`), with the feature, Cooper's content ranges and the
   shape output.
2. Merge their questions, removing duplicates. Where their proposals conflict, apply
   the council's conflict rule (severity first, then rank) and show the user both
   sides when the choice is a product decision.
3. Ask the user once, with the proposed answers as defaults.
4. Write their blocks of `brief.md`.

Done when: every field of both blocks is answered, OPEN with who decides, or `n/a`
with a reason.

## 5. Close the brief

1. Copy each member's plan risks into the brief's Risks table with their severity.
2. Set the status to `planned` and write the next step under Links:
   - `ui` → build from the brief (and the shape output), then `/nafis review`;
   - `backend` → the `spec` route; `questionnaire` for a decision someone else owns;
     `multi-session` for work that spans sessions. Without a helper, name the step.
3. Tell the user: the brief's path, its open items with owners, the kill-metric line,
   and the next step.

Done when: `brief.md` has no blank field, the kill-metric line is on top (or recorded
as declined), and the user has the path, the open items and the next step.
