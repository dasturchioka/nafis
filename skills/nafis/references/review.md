# Review

Steps for `review [target]` and single-lens review runs. You arrive here with
`council.md` and the profile read.

## 1. Pin the target

Read what is to be judged:
- nothing named → the current branch's committed and uncommitted changes against its
  merge-base with the default branch;
- a PR (number or link), a branch or a commit range → its diff;
- paths → those files as they stand;
- a URL or a screen name → that screen in the running product;
- screenshots, message templates, chat transcripts → those, as given.

Note whether the code under review is in this working tree: only then can the fix step
change it.

Done when: you can list every changed path and every screen or message in scope.

## 2. Target and roster

Take the target from the paths and the roster from the task size (`SKILL.md` → Target,
Roster by task size). A single-lens command fixes the roster. When the roster is
nobody, say so in one line and stop.

## 3. Feature and promises

- With all three members, the run needs a feature. Match an existing one by the slug
  the user gave, the branch or PR name, or a brief whose job covers the changed
  surfaces; ask once if two fit. With no match, open one: create the folder, copy
  `brief.md` from the template with the status `opened at review, not planned`, and
  create `reviews.md` from its template.
- With fewer members, use a matching feature when there is one; otherwise run without.
- A feature's `brief.md` holds its promises; the members read it through the dispatch.

## 4. Cooper, the gate

When Cooper is on the roster, start him alone first (`mode: review`; `SKILL.md` →
Starting a council member).
- Verdict ⛔: stop before the others. Show his findings and ask the user to narrow,
  stop, or continue anyway; continue only on their word.
- Otherwise carry his cut list and content ranges into the next dispatches.

Done when: Cooper's report is in, and the gate is passed or the user has the question.

## 5. Tesler and Norman

Start the rest of the roster (`mode: review`, or `quick` for Norman alone on copy,
colour or one control), with Cooper's cut list and content ranges.

## 6. Merge into one verdict

Take every report through these steps in order:
1. **Check.** Drop findings without evidence and note each drop. Keep at most five per
   member, the most severe.
2. **Merge duplicates.** Findings on the same element with the same problem become one,
   at the highest severity, crediting every member that found it.
3. **Settle conflicts.** When two Moves pull the same element in opposite directions,
   keep the higher severity; on a tie keep the higher rank (Cooper > Tesler > Norman).
   Note the overruled finding in one line.
4. **Park.** Findings on elements Cooper put on the cut list wait for the user's cut
   decision.
5. **Order and verdict.** Order by severity (`shit`, `bad`, `nice`), then by rank. The
   verdict is the worst kept severity: any `shit` → ⛔ stop, else any `bad` → ⚠️ fix,
   else ✅ pass.

Done when: every finding is kept, merged, overruled, parked or dropped, and each one
not kept has its reason.

## 7. Report

Send the user one message:

```
Nafis review · <target> · <roster> · <date>
Kill metric: <threshold> by <date>                  (feature runs)
Verdict: ⛔ stop | ⚠️ fix | ✅ pass
Promises: job <kept|broken>, no-gos <kept|broken>, metric <measurable|not>   (when a brief exists)

For you to decide
1. [shit · <Member> · <card>] <finding>. Evidence: <ref>. Move: <move>
Cut list (Cooper)
- <element>: <why>
Fixing now
3. [bad · <Member> · <card>] <finding>. Evidence: <ref>. Route: <route> (<helper or "direct">)
Polish, optional
5. [nice · <Member> · <card>] <finding>
Evidence gaps: <gap>, <what would check it>
Merged or overruled: <one line each>
```

Real customer data appears in this message only as the house rules allow.

## 8. Record

On a feature run, append one entry to `reviews.md` in its template's format, with the
decision column at `open`. Feature-less runs record nothing.

## 9. Act

1. **Ask once.** In one message, ask the user to decide: each ⛔ finding, each cut-list
   item, any evidence gap that needs a writing route (`worst-case`, `prototype`), and
   whether the `nice` items should go through `polish` (`ui` targets).
2. **Fix the ⚠️ findings** that are not parked, without waiting for those answers:
   - code in this working tree → resolve each finding's route (`routes.md`). Group the
     findings per helper so each helper runs once with all of its findings; apply the
     Move yourself where no helper is available. Follow the repo rules file.
   - code elsewhere (someone else's PR) → give the user the ⚠️ findings with their
     Moves as review comments.
3. **Re-check each fix** against its own evidence: the same screen and conditions, the
   same `file:line`, the same query. A finding still failing gets one more pass, then
   goes to the user with what was tried.
4. **Apply the user's answers** through the matching routes: a cut goes to `distill`;
   parked findings on kept elements are fixed like ⚠️ ones; parked findings on cut
   elements close with the cut.
5. **Record** each finding's decision in `reviews.md`: `fixed via <helper or direct>,
   re-checked`, `user: <decision>`, `parked: <open question>`, or
   `handed over: <reason>`.

Done when: every finding has a decision, recorded in `reviews.md` on a feature run or
stated in your final message otherwise.
