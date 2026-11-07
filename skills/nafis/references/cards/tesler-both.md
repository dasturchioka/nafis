# Tesler cards: both

Loaded on every Tesler run, `ui` and `backend` alike.

## Where the complexity lives

### Tesler's Law / Pull complexity downwards · both
**Ask:** This task has complexity that cannot be removed. Does the system absorb it once, or does every user and caller carry it every time?
**Smell:** People format, convert, look up ids, reconcile states or retry by hand. A screen, bot turn or API asks for a value the system already holds. The backend returns raw codes or states for the client or user to interpret. A new required option, config knob, manual step or "the caller must first…" note covers something the module could compute. "Power users can configure it" stands in for a design.
**Move:** Derive, infer, validate and reconcile inside the system. Expose a knob only when the caller genuinely knows better, and give it a working default. One extra engineering week beats an extra minute for every user, and a module has more users than authors, so prefer a simple interface over a simple implementation.
**From:** Laws of UX; hacker-laws (Tesler's Law); Ousterhout (pull complexity downwards)

### Define errors out of existence · both
**Ask:** Could this operation be defined so that the error or special case cannot happen, instead of every caller and user handling it?
**Smell:** Callers wrap the same call in the same guard. The operation raises "already gone", "not found" or "out of range" when the caller's intent is in fact already satisfied. Special-case branches are spread across callers. A form or bot rejects input the system could safely normalize.
**Move:** Redefine the semantics so the normal path covers the edge case: make sure something is absent instead of failing to delete it, clamp a range, or return an empty result instead of failing. Otherwise handle the case low enough that upper layers never see it, or catch a family of errors in one place. Keep surfacing anything the caller must act on, because masking it creates a silent failure.
**From:** Ousterhout (define errors out of existence; masking, aggregation, designing special cases away)

### Postel's Law · both
**Ask:** Does the system accept the many ways people and clients write the same value, answer in one strict form, and refuse what it would have to guess?
**Smell:** Input is rejected for spaces in a phone number, letter case, stray whitespace or a common alternative date format. Or the opposite: handling silently accepts several shapes, aliases or coerced types, scattered through the code, until the leniency becomes a contract nobody can tighten. Responses vary in shape.
**Move:** Normalize variants of a known value once, at the edge, where the meaning is unambiguous; reject the rest, unknown fields included, with a specific reason (Norman: Constraint). Emit output that strictly follows the contract. Absorbing harmless format differences is the system carrying complexity; guessing what someone meant is not.
**From:** Laws of UX; hacker-laws (Postel's Law, with its reconsideration)

### Overexposure / Interface segregation · both
**Ask:** Can the most common use be done without learning the rare ones?
**Smell:** The everyday call needs options, setup or arguments that exist only for edge cases. A consumer depends on a wide interface but uses two of its operations. The main form or bot flow shows advanced choices to everyone.
**Move:** Make the common case the shortest path, with good defaults. Put rare capability behind a separate entry point, or give each kind of client a narrower interface sized for it.
**From:** Ousterhout (overexposure; make the common case simple); hacker-laws (SOLID: interface segregation)

### Mode flags / Temporary field · both
**Ask:** Does a flag or mode quietly change what this operation means, so that callers must know which combinations are valid?
**Smell:** Boolean or mode parameters choose between different algorithms. Arguments can contradict each other. Some fields mean something only in certain states and are empty otherwise. A toggle on a screen changes how other fields are read.
**Move:** Split the operation into one per intent, or model one type per state, so an invalid combination cannot be expressed. Move fields that only matter in one state into the object for that state.
**From:** Refactoring Guru (long parameter list, temporary field)

## The person's load

### Choice Overload · both
**Ask:** When the set is large (plans, products, templates, integrations), what helps the user narrow it down and compare?
**Smell:** A long, undifferentiated list or grid. Options are shown one at a time, so the user must remember earlier ones. List endpoints have no filter, sort or search, so the screen shows everything.
**Move:** Lead with a small featured set, offer search and filters up front, and place comparable items side by side on the attributes that decide the choice.
**From:** Laws of UX

### Working memory / Recognition over recall · both
**Ask:** What must the person remember from an earlier screen, message or step to complete this one?
**Smell:** The user copies a value, id or price from one place to another, or flips between pages to compare. A multi-step flow drops earlier choices. A bot asks "Which one?" without listing the options. Help exists only in an onboarding tour. An API returns bare ids, so the screen cannot show names.
**Move:** Let the system hold it: carry a summary of prior choices into every step, offer the valid options as buttons, show items to compare together, give help in context, and return names alongside ids. Design as if the person is interrupted after every step; budget about 4–7 chunks, which fade in roughly 20–30 s.
**From:** Laws of UX (Working Memory); Nielsen H6, H10; NN/g

### Miller's Law & Chunking · both
**Ask:** Is long or dense information broken into meaningful, labelled groups that the user can scan?
**Smell:** Walls of text, or settings and pages with no sections. Generated codes, keys or numbers that users must read or type appear as one unbroken string. An item limit is justified only by the "magic seven".
**Move:** Group content into short modules with clear hierarchy and separators, and show long strings in segments. Set limits from the task, not from 7±2, since capacity varies with the person and the context.
**From:** Laws of UX

### Zeigarnik Effect · both
**Ask:** When work is interrupted, can the user return to exactly where they left off and see what is still open?
**Smell:** Leaving a half-done form or setup loses the input, or a draft lives only in one session or device. Unfinished items have no visible way back, and more content exists with no cue.
**Move:** Persist partial progress on the server and surface unfinished tasks with a clear resume point and what remains. Signal clearly when more content is available.
**From:** Laws of UX

## Boundaries callers feel

### Shotgun surgery / Divergent change · both
**Ask:** Does one change land in one place, and does each place change for only one reason?
**Smell:** One business change makes small matching edits across many files (change amplification). One module gets edited for unrelated reasons, such as pricing, display and notifications. A new variant needs a twin in a parallel hierarchy. In UI, one product rule is implemented again on several screens.
**Move:** Gather what changes together into one module, and split modules that change for unrelated reasons, so each decision costs one edit. Split by reason for change, not by size, so the pieces stay deep.
**From:** Refactoring Guru (shotgun surgery, divergent change, parallel inheritance hierarchies); Ousterhout (change amplification); hacker-laws (SOLID: single responsibility)

## Design process

### Occam's Razor / YAGNI · both
**Ask:** What can be removed from this design, plan or diff with the outcome intact, and which part serves a need nobody has today?
**Smell:** A new option, flag, field, step, endpoint, hook or abstract layer with one user or none, added "for flexibility" or "for later". Two ways to do the same thing. Clever generic machinery that will be harder to debug than it was to write (Kernighan's Law).
**Move:** Choose the solution with the fewest parts and assumptions. Remove elements one at a time until any further cut breaks the function, and delete speculative parts until a second real use appears. A general interface is fine; machinery for features nobody asked for is not.
**From:** Laws of UX (Occam's Razor); hacker-laws (YAGNI, KISS, Kernighan's Law); Refactoring Guru (speculative generality)

### Design it twice · both
**Ask:** What radically different design was considered, and why does this one leave callers and users less to carry?
**Smell:** A plan or PR offers a single shape with no alternative, or its "alternatives" are small variations on the same shape.
**Move:** Sketch at least two radically different interfaces or flows. Compare them first on how easy they are for the caller or user, then on simplicity, generality and cost. The chosen design may combine parts of both.
**From:** Ousterhout (design it twice)

### Strategic vs tactical programming · both
**Ask:** Does this change leave the design better than it found it, or does it only make the feature work?
**Smell:** A workaround sits on top of another workaround. A flag or special case routes around a structure instead of fixing it. Cleanup is deferred to a "later" ticket. Increments are features bolted on rather than abstractions.
**Move:** Make each increment the abstraction the system would have had if it had been designed with this need in mind. Spend a small, steady share of every change on structure; a change that does not improve the design usually degrades it.
**From:** Ousterhout (strategic vs tactical programming; increments should be abstractions)

## AI and automation

### Grounded output · both
**Ask:** What may the model see, and what checks its output against the system of record before it changes data, money or what a customer reads?
**Smell:** Free-form output parsed loosely and saved or sent as is; prices, stock, dates, ids or policy taken from the model's text instead of looked up; the model's own "I'm sure" used as a gate (in one study a model flagged its uncertainty on 7 of 153 errors); one model's output fed into another unchecked; a customer's pushback ("are you sure? it was cheaper last week") talks the model into a new price, since models lean toward agreeing with the user; a person approving every output as the only safeguard, re-deriving whole drafts by hand. A made-up price, policy or promise reaching a customer, or one customer's data in another's answer, is **shit**.
**Move:** Give the model only the data this reader may see. Ask for a fixed structure (named fields, closed choices), validate it in code (types, ranges, ids exist, totals recompute) and take every fact from the system of record, so the model chooses and phrases while the record supplies the values. Gate on measured signals (validation passed, a source was found, repeated samples agree) rather than the model's words; on a failed check, repair once, then fall back. Where a person reviews, point them at the parts that failed or vary, so review costs less than doing the job.
**From:** NN/g (AI hallucinations; sycophancy; chatbots discourage error checking); PAIR (Data collection: chained outputs, exposure through outputs; Errors + graceful failure)

### Fallback ladder · both
**Ask:** When the model is unsure, out of scope or unavailable, what happens instead, and does the person still reach their goal without repeating anything?
**Smell:** The model guesses on an ambiguous request instead of asking; a confident answer to an out-of-scope question; no path that works without the model when it or its provider is slow or down; a handoff that drops the conversation, so the customer explains everything again; a request for a person refused or argued with; the same question rephrased again and again with no escalation. A customer left with neither an answer nor a human is **shit**.
**Move:** Define the ladder with thresholds from measured confidence: act, then ask with 3–4 concrete options, then offer a narrower or simpler service, then hand to a human or the manual path. Honour every explicit request for a person and escalate after repeated rephrasing or a failed repair, while keeping escalation rare enough that the AI still earns its place. Pass the human what was asked, what the model tried and why it stopped; keep the non-AI path in daily use so it works on the bad day (see Circuit breaker).
**From:** HAI G10 (patterns 10-A, 10-C); PAIR (Errors + graceful failure; Feedback + control: manual fallback); NN/g (5 dimensions of site chatbots)
