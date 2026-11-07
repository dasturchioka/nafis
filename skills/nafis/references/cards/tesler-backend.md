# Tesler cards: backend

Loaded for `backend` targets, together with `tesler-both.md` and both reliability files.

## Boundaries callers feel

### Deep module / Shallow module · backend
**Ask:** How much does a caller have to learn to use this, compared with how much it does for them?
**Smell:** A new unit's signature nearly restates its body. A feature works only after the caller composes several small wrappers. Many tiny units each bring their own interface and setup ("classitis").
**Move:** Merge shallow pieces until the interface is far smaller than the behaviour behind it. Judge by the ratio of interface to functionality, not by line count: a long unit with a simple signature and a readable body is healthy.
**From:** Ousterhout (deep modules, shallow module, classitis); Refactoring Guru (lazy class, long method)

### Leaky abstraction / Liskov substitution · backend
**Ask:** Which behaviour of the hidden layer still reaches callers, and does the interface admit it?
**Smell:** An interface looks local and instant but sits on something remote, slow or limited. An implementation rejects or ignores some operations it inherited ("not supported"). Callers check which implementation they hold. An abstraction hides details callers need to use it correctly (a false abstraction).
**Move:** Make the leak part of the contract (cost, limits, partial outcomes) rather than pretending it isn't there. Split the interface so each implementation honours all of it, or replace inheritance with delegation. Hide only what callers truly do not need.
**From:** hacker-laws (Law of Leaky Abstractions; SOLID: Liskov substitution); Refactoring Guru (refused bequest); Ousterhout (false abstraction)

### Information leakage / DRY · backend
**Ask:** Which design decision does this change encode, and how many modules now know it?
**Smell:** Two modules each read or write the same format, key layout, status list or threshold. A rule is copied into a second place "to avoid a dependency". An interface doc explains internals that callers then start to rely on.
**Move:** Give each decision one owner and have the other modules ask it. Keep interface docs to what callers need. DRY concerns knowledge, not text: two look-alike fragments that encode different rules may stay separate.
**From:** Ousterhout (information leakage; repetition; implementation documentation contaminates interface); hacker-laws (DRY); Refactoring Guru (duplicate code)

### Temporal decomposition / Conjoined methods · backend
**Ask:** Can each piece be understood and changed without reading its neighbour, or was the code split by run order rather than by what each piece knows?
**Smell:** Stage modules (read, validate, transform, save) each carry knowledge of the same format. A function is correct only if another one ran first. Two pieces in different places make sense only when read together. Call order is a hidden rule.
**Move:** Group code that shares knowledge, even when it runs at different times. Join conjoined pieces, or make the order part of the interface: step one returns what step two requires. Split a unit only when the part comes away cleanly.
**From:** Ousterhout (temporal decomposition; conjoined methods; better together or apart; nonobvious code)

## AI and automation

### Model budget · backend
**Ask:** How much time and spend per task is this model call allowed, what enforces the limit, and is the AI path still faster than doing the job by hand?
**Smell:** A model call on an interactive path with no timeout; an agent loop with no cap on steps, time or spend; cost per conversation unknown, or growing every turn because the whole history is resent; a parse failure that regenerates the entire answer; one tenant's volume draining a shared quota; an AI flow slower than the manual one (in one test an agent took 11 minutes on a booking form a person filled in about 2).
**Move:** Set per-feature budgets for time to first useful output, total time, spend per task and maximum steps; enforce them in code and step down the Fallback ladder when one is hit. Acknowledge at once and show progress on long runs (see Feedback / Doherty Threshold). Meter spend per tenant and feature, alarm on outliers, and compare the AI path's time-to-done with the manual path before calling it a win.
**From:** NN/g (agent mode impressions); HAI pattern 10-C (fallbacks for low-latency use); PAIR (User needs: when speed and low cost outweigh AI)
