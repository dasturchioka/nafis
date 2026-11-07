# Cooper cards: both

Loaded on every Cooper run, `ui` and `backend` alike. Cooper has no `backend`-only cards.

## Who, and which goal

### Job to be done · both
**Ask:** In what situation does this person need it, what progress are they trying to make, and what do they use for that job today (a workaround, a person, or nothing)?
**Smell:** The spec names a feature or a customer segment ("for shop owners", "add a calendar") but no situation, or its requirements come from asking users which attributes they want and averaging the answers, which gives a product that fits no one. Backend: an endpoint or job justified by "clients may need it", with no situation where someone calls it.
**Move:** Write one line, "When <situation>, <who> wants <progress>; today they <workaround>", based on observed behaviour (support threads, logs, transcripts, timing of use) rather than stated wishes. The workaround, including doing nothing, is the competitor the design must beat.
**From:** Christensen JTBD; HBS milkshake study; Product runbook

### Elastic user · both
**Ask:** Who exactly is the primary user of this surface, and when their needs conflict with another group's, whose need wins?
**Smell:** "The user" is a novice on one screen and a power user on the next, stretching to fit whoever is arguing. A backend change says "clients" without separating the end user who feels the outcome, the caller who codes against it and the operator who handles its failures.
**Move:** Name one primary person per surface, with their goal, skill and context taken from research. Serve the others without compromising that person. For backend, write one line each for the end user, the caller and the operator. Handle rare edge cases correctly, but don't let them shape the main design.
**From:** Inmates; About Face; Persona research

### Goals, not tasks · both
**Ask:** What end state does the person want, and would each step in this design still exist if the system could do it for them?
**Smell:** The flow copies today's manual procedure onto a screen (the paper form, the spreadsheet columns, the internal approval chain). Acceptance criteria list clicks and fields, not outcomes. An integration exposes internal stages (draft, attach, submit) when the caller only wants "invoice sent".
**Move:** State the end goal and the personal goals behind it (not feeling stupid, not making mistakes), then design backwards from the end state. Tasks are interchangeable means. Goals stay stable while tools change, so anchor acceptance criteria to the goal.
**From:** About Face; Inmates; Goal-Directed Design

### Paradox of the Active User · both
**Ask:** Does this work for someone who skips every instruction and starts acting at once?
**Smell:** Success depends on reading a manual, a tour, a long intro message or a help page first. The first real attempt can do damage, or its mistakes are explained only in documentation.
**Move:** Put guidance at the moment and place of the action (inline hints, examples in empty states), and make the first attempt safe to get wrong.
**From:** Laws of UX

### Excise · both
**Ask:** Which steps, fields or confirmations here serve our needs (reporting, internal process, team boundaries) rather than the person's goal?
**Smell:** Required fields that nothing downstream reads, setup before the first useful result, and confirmations that protect the team rather than the user. Backend: an operator must do a manual step after every run, or a caller must follow an internal procedure to reach one outcome.
**Move:** Map each step to the user's goal. Remove or automate the rest, or record it in the spec as a deliberate product cost with its reason and owner.
**From:** About Face (eliminating excise)

## Problem before solution

### Narrow the problem · both
**Ask:** What actually went wrong, when, and for whom? Or is this request a solution in disguise?
**Smell:** A customer's proposed fix becomes the requirement in the TZ ("add permission rules", "calendar view", "a setting for X"), with no concrete moment when the current flow failed.
**Move:** Ask *when* they needed it, then replay that moment. Write the problem as one specific story in which today's flow fails, and judge each solution by whether that story now ends well. In the source case, a six-week permissions request shrank to a one-day warning on the action that caused the harm.
**From:** Shape Up (Set Boundaries; Write the Pitch)

### Recurrence · both
**Ask:** Does this problem keep coming back across many users, or is it one loud request (or one reaction to a launch)?
**Smell:** The TZ traces back to a single message or one large client. Complaints after a launch turn into commitments within days. The feedback channel is mistaken for the whole user base, although a vocal ~1% produce most of what gets posted.
**Move:** Treat each request as a raw idea: acknowledge it without committing, check how often it recurs in support history and usage data, and shape it before betting time on it. After a launch, let the first reaction settle, then recheck who the change serves.
**From:** Shape Up (Bets, Not Backlogs; Move On); hacker-laws (90–9–1 Principle)

### Cognitive Bias · both
**Ask:** Which belief about the user does this rest on, and what result would show that belief is wrong?
**Smell:** The rationale is "users want X" or "obviously" with no source. Only confirming evidence is cited, the imagined user resembles the team, and the success metric can only show success.
**Move:** List each assumption with its evidence or a cheap test, check it against users unlike the team, and pick a metric that could disprove the plan.
**From:** Laws of UX

### Grab-bag · both
**Ask:** Which single problem does this project solve, and what does "done" look like?
**Smell:** "Redesign settings", "v2", "rewrite the integration" or "refactor the pipeline" with no use case driving it. A second version that adds every deferred wish at once (Second-system effect). A large new system specified in full on paper (Gall's Law).
**Move:** Split the work into named problems, each with its own appetite and finish line. Grow from a simple version that already works, and extend it one proven step at a time.
**From:** Shape Up (Set Boundaries); hacker-laws (Gall's Law; Second-System Effect)

### Reuse before adding · both
**Ask:** Does an existing surface already serve this job, and if a new one is proposed, does it exist for the user or for our org chart?
**Smell:** A new screen, endpoint, command, notification type or setting that overlaps an existing one. A structure that follows team or service boundaries (separate settings per internal team, events per internal service) instead of the user's job (Conway's Law).
**Move:** Start by listing the current surfaces that touch the job. If one already serves it, stop and point to it; otherwise extend the closest one. A new surface needs a stated reason why the existing one cannot carry the job.
**From:** Product runbook; hacker-laws (Conway's Law)

## Appetite and boundaries

### Appetite · both
**Ask:** How much time is this problem worth, decided before the solution, and does the proposed design fit within it?
**Smell:** An estimate that grows out of the design instead of a budget that limits it. "The best solution" debated with no constraint. Open-ended work that fills all the time available (Parkinson's Law) and still runs late after padding (Hofstadter's Law).
**Move:** Fix the time and let the scope vary: a small batch is 1–2 weeks, a big batch is 6 weeks, for one designer and one or two programmers. If the design does not fit, narrow the problem or split off a part that fits. When time runs out, reshape the work rather than extend it by default; adding people to a late project rarely saves time (Brooks' Law).
**From:** Shape Up (Set Boundaries; The Betting Table); hacker-laws (Parkinson's, Hofstadter's, Brooks' Laws)

### No-gos and rabbit holes · both
**Ask:** Does the plan say what is out of scope, and which hard decisions has it settled before the build starts?
**Smell:** No "not doing" list. Questions such as "what happens to existing records", "how do completed items show" or "what if the provider is down" are left for the builder to settle under deadline. The plan assumes a design or technique exists that nobody has actually tried.
**Move:** Walk through one use case step by step and list the unknowns. Patch each one (often by keeping today's behaviour for that part), cut it, or fence it off as a named no-go. Ask a technical expert "can this fit the appetite?", not "is this possible?".
**From:** Shape Up (Risks and Rabbit Holes; Write the Pitch)

### Scope hammering · both
**Ask:** Is each piece a must-have for the core job, and is the result better than what users live with today, rather than merely short of the ideal?
**Smell:** Scope grows with every "wouldn't it be better if" and each newly found edge case. Nice-to-haves sit unmarked beside must-haves. Features for imagined future needs get built now. The work is compared with a perfect design, so it never ships.
**Move:** For each addition, ask: can we ship without it? Is it a new problem or one users already live with? How likely is the case, which users hit it, and how bad is it when it happens? Mark the rest as nice-to-have and cut it first. Measure progress against users' current workaround, not against the ideal.
**From:** Shape Up (Decide When to Stop; Map the Scopes); hacker-laws (YAGNI)

### Pareto Principle · both
**Ask:** Which few flows, users or cases produce most of the outcome, and is this effort spent on them?
**Smell:** Effort is spread evenly across all cases: rare branches, settings or endpoints get polish while the main path stays rough. Priorities are set without usage or outcome data.
**Move:** Rank cases by their share of users and value (often close to 80/20). Make the vital few excellent first, and give the long tail a plain, safe path.
**From:** Laws of UX

## Defaults, contracts and trust

### Default is the decision · both
**Ask:** What happens to someone who never touches this, and does that match today's behaviour?
**Smell:** A new behaviour switched on by default for existing customers. A config flag whose default is the new path. A default chosen for the implementer's convenience. A bot that starts sending a new kind of message to every chat. An unannounced default that changes what customers receive or pay is **shit**.
**Move:** Name each default in the spec as a product decision, with an owner and a reason. Keep today's behaviour as the default, make new or risky behaviour opt-in, and widen it only on evidence.
**From:** Product runbook

### Hyrum's Law · both
**Ask:** Who already depends on the current behaviour, including behaviour nobody promised, and what breaks for them?
**Smell:** The diff changes a field's meaning, message wording, ordering, timing, rounding, a default, an error code or a button's location, defended with "no one uses that". A new interface returns internals "for convenience". A guard or step is removed and no one knows why it existed (Chesterton's Fence). A fix to one path quietly adds load to another (Hutber's Law). A silently broken contract is **shit**.
**Move:** Treat every shipped surface as a contract: state what is promised and keep everything else unobservable. List the observable behaviours a change touches and who relies on them, then keep them, version them, or migrate those callers before shipping. Find out why something exists before removing it.
**From:** Product runbook; hacker-laws (Hyrum's Law; Chesterton's Fence; Hutber's Law)

### One-way door · both
**Ask:** Which decisions in this plan cannot be undone once shipped, and is each one's process sized to that: deliberate for the one-way doors, fast for the two-way ones?
**Smell:** One process for everything: a reversible label, threshold or layout waits weeks for sign-off, while dropping old data, messaging every customer, changing a price or publishing a contract rides in the same quick change. "We can always roll back" is said of a step that deletes, sends, charges or publishes. A one-way step taken lightly that loses data or breaks a contract is **shit**.
**Move:** Mark each decision one-way or two-way in the plan, and first try to turn one-way doors into two-way ones (keep the old data, defer the irreversible step until evidence is in, expose a slice first). Review what stays one-way with the people who will live with it; let a small group decide two-way doors on about 70% of the information they want, and name the signal that would reverse a wrong call.
**From:** Shareholder letter 2015; Shareholder letter 2016

### Trust and durability · both
**Ask:** Will people trust this enough to rely on it, and will it still matter once tools and models get better?
**Smell:** An automated answer or action that is right most of the time and silently wrong the rest, with no way for the user to tell. Effort spent hand-patching today's tool limits, which a better general method will soon make obsolete (Bitter Lesson). A wrong automated message sent to a customer is **shit**.
**Move:** Ship automation where its mistakes are visible and reversible, and where the user opts in; a feature users do not trust is worse than no feature. Invest in what gains value as capability grows (data, workflow, feedback loops), and keep stopgaps cheap to remove.
**From:** Product runbook; hacker-laws (The Bitter Lesson)

## Success and exit

### One metric · both
**Ask:** Which single number shows that the job got done, what threshold counts as success, and was it fixed before building?
**Smell:** Success means "shipped". Metrics are chosen after launch to fit the result. A dashboard counts activity (messages sent, screens viewed) with no link to the job. A target can be met without helping anyone (Goodhart's Law). A task flow has no time-to-done target, so it expands to fill the user's patience (Parkinson's Law).
**Move:** Before building, record one outcome metric tied to the job, with its baseline, success threshold and check date, plus a guardrail the team may not trade away. For task flows, time-to-done against the user's expectation is often the right metric. When a result looks too good, check the measurement before celebrating (Twyman's Law).
**From:** Product runbook; hacker-laws (Goodhart's Law; Twyman's Law); Laws of UX (Parkinson's Law)

### Kill metric · both
**Ask:** What number, by what date, means we remove this, and who checks it?
**Smell:** There is no exit criterion, so every launched feature, setting and background job lives forever and adds support and maintenance load. "Let's see how it goes."
**Move:** When the bet is made, record the kill threshold and review date next to the success metric. Removing an underused surface is a planned outcome, carried out as a contract change with notice to those who depend on it.
**From:** Product runbook

## Conversations

### Message budget · both
**Ask:** Is each proactive message (notification, reminder, status update, email) worth interrupting this recipient for right now, and can they stop one kind without losing the rest?
**Smell:** One message per event, so a batch job lands as a burst; alerts about activity that does not involve the recipient; consent asked before the person has seen any value, or without saying what will arrive; an interrupting channel used for routine news; one all-or-nothing off switch, or one buried in settings; heavy sending defended by "they can turn it off", although most people never change defaults.
**Move:** Send only what concerns this recipient and needs their attention or action; bundle a burst into one summary with a count (the source's line: more than five at once becomes one); ask for consent after a moment of value and name what will arrive; use the least intrusive form that works (indicator, passive notice, action-required) and keep interrupting channels for events that need a response; offer per-type opt-out where the messages appear, plus a frequency or importance limit.
**From:** NN/g push notifications; NN/g indicators, validations and notifications; CDG (notifications)

## AI and automation

### Automation boundary · both
**Ask:** For each thing the model decides, has the design chosen whether a fixed rule handles it, the model acts alone, the model proposes and a person approves, or a human owns it, and was that tier picked by what a wrong result costs?
**Smell:** One autonomy level for everything: the model that drafts a summary also sends customer replies, changes prices or deletes records. A model where the answer can be written as a rule or must be exact (totals, permissions, prices, statuses). "The AI handles it" with no named person for what it cannot do. A model that messages customers, moves money or deletes data with no approval step is **shit**.
**Move:** Record the tier per output. Use a rule wherever the answer can be specified; let the model act alone only where its errors are cheap, visible and reversible; have it propose where stakes are high, responsibility is personal or the preference is hard to put into words; place a person's approval right before each irreversible step. Widen its autonomy tier by tier on measured accuracy (see Trust and durability).
**From:** PAIR (User needs + defining success); HAI G10; NN/g (site chatbots; AI agent studies)

### Error costs · both
**Ask:** What does a false positive cost here compared with a false negative, who pays each one, and was the decision threshold set from those costs?
**Smell:** Success stated as one accuracy figure with no split by error kind; both errors weighed equally by default; a confidence cut-off left at a library default or a developer's guess; quality judged on hand-picked demo inputs rather than real, messy ones (other languages and scripts, blurry photos, slang); a metric the model can inflate by acting more (replies sent, suggestions shown).
**Move:** Write the four outcomes in the user's words ("flagged a real order as spam" vs "let spam through"), price each, and lean toward precision or recall on purpose. Before launch, record a quality threshold with the action it triggers (for example: more than 20% of suggestions rejected means review the model). Measure on a sample of production inputs split by user group, launch where coverage is proven, and recheck on day 100 as well as day 1 (see One metric, Kill metric).
**From:** PAIR (User needs + defining success; Data collection; Errors + graceful failure)

### Adapt cautiously · both
**Ask:** When the model, its instructions or what it learned from users changes, what shows that answers people rely on stayed as good, and who is told?
**Smell:** Prompt or instruction text edited in production with no test cases; the model or its version swapped "because the new one is better" with no side-by-side on real cases; personalisation that reshuffles results or wording the user had learned; capabilities added or removed with no notice. Hyrum's Law with no visible diff: one instruction edit rewrites every future message at once.
**Move:** Treat a model, prompt or tuning change as a contract change (see Hyrum's Law). Keep a regression set of real cases with known good answers and past failures, compare old and new on it before switching, roll out behind a switch with fast rollback, and watch the Error costs threshold after release. Keep areas the user has learned stable while adapting the rest, and tell users when capabilities are added or change.
**From:** HAI G14, G18; PAIR (Mental models: re-boarding; User needs: monitor over time)
