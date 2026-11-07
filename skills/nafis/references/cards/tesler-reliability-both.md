# Tesler reliability cards: both

Failure and data complexity that reaches people and callers. Loaded for every `backend` target, and for a `ui` target whose flow writes, waits, retries, shows times, money or long lists, or ships behind a staged rollout.

## Duplicates, retries and failing dependencies

### Idempotency key · both
**Ask:** If this write arrives twice (double-click, a client retry after a lost response, a replayed form), does the system perform it once and answer the same both times, or does the caller have to work out what happened?
**Smell:** A create, charge or send operation with no caller-supplied key; dedup by "look up, then insert" without a uniqueness guarantee; a retry answered with "already exists"; a key built by hashing the parameters; a disabled submit button as the only guard.
**Move:** Accept a caller-generated random key scoped to the caller, stored with a request fingerprint and the final response in the same atomic step as the effect. A replay gets the stored response, the same key with different parameters gets a mismatch error, a concurrent same-key request gets "in progress"; keep keys for the retry window (about a day; three days survives a weekend fix), and give forms a hidden key.
**From:** Idempotency post, Idempotency keys essay, Builders' Library, Cloud Design Patterns

## Multi-step changes and consistency

### Lost update · both
**Ask:** When two requests, workers or people change the same record at once, does the second one see the first, or silently overwrite it?
**Smell:** Read the record, modify it in code, write all of it back; "last save wins" in an edit form with no version; a balance or count written as an absolute value computed from a stale read; a half-finished multi-step operation shown to others as final.
**Move:** Check a version on write and reject or merge stale writes, telling the user what changed; mark in-flight work with an explicit pending state (semantic lock) that other flows respect. Note the trade: an increment survives concurrency but not redelivery, an absolute write survives redelivery but not concurrent or reordered writers, so pair each with an idempotency key or a version check.
**From:** Cloud Design Patterns

### Asynchronous request-reply · both
**Ask:** For work that outlasts a normal request, how does the caller learn the final outcome, including failure and cancellation, without guessing or resubmitting?
**Smell:** A long synchronous call that times out at a proxy while the work continues, so the user clicks again; "accepted" with no status location; a status lookup that says "not found" while pending (indistinguishable from a bad id); no terminal failed state; a spinner with no end; results kept forever.
**Move:** Validate first, then accept fast with a status resource and a polling hint; status carries documented terminal and in-progress states, created and updated times, and a structured error on failure. The submit takes an idempotency key so a repeat returns the existing operation; offer cancel, state a result retention window, and show pending, done and failed as distinct UI states.
**From:** Cloud Design Patterns, microservices.io

## Data correctness

### Instants · both
**Ask:** Is every point in time stored as an instant with its zone, and is every time range half-open?
**Smell:** A timestamp without a zone holding "UTC by convention" or server-local time; a date picker sending local midnight with no offset; day or month filters written as an inclusive between (a midnight row lands in two periods) or by wrapping the column in a truncate or format function; rounding to whole seconds, which can store a moment in the future.
**Move:** Store events as zone-aware instants and convert to the viewer's zone only for display. Store a future wall-clock appointment as local time plus a named zone (region/city), because zone rules can change before the date arrives. Keep other zone-less values only for things never used in arithmetic. Filter with start ≤ t < next start, computing the bounds on the parameter side, and truncate rather than round.
**From:** Don't Do This, Use The Index, Luke

### Money · both
**Ask:** Is every amount stored, sent and computed exactly, with its currency beside it?
**Smell:** Binary floating point for amounts, prices or rates; a locale-formatted money type whose display changes with a setting and cannot hold fractions of the minor unit; an amount column with no currency; a form or client that parses a price as a float; totals rounded differently from their stored lines.
**Move:** Use exact decimal (or integer minor units) end to end, with enough scale for unit prices and rates; store the currency next to each amount; fix where and how rounding happens, and build totals from the stored rounded lines so they match what the customer sees.
**From:** Don't Do This

### Keyset pagination · both
**Ask:** Does this list stay complete and fast at page 500, and while rows are added or removed between page loads?
**Smell:** Offset or page-number paging over a live list, so rows repeat or vanish between pages and deep pages slow down; a sort on a non-unique column with no tiebreaker; a sync, export or batch job that walks a table by offset and misses rows.
**Move:** Page by "after the last seen key" with a deterministic order ending in a unique tiebreaker, backed by an index on the same columns in the same direction; hand the caller an opaque cursor, and in the UI prefer "load more" or next/previous over jump-to-page.
**From:** Use The Index, Luke

### Locale per person · both
**Ask:** Is each person's language, and separately their region, decided once, stored with them, and used by every path that writes to them, with a defined fallback when a translation is missing?
**Smell:** Language guessed from address, phone prefix or region (or region from language) on every visit; a saved choice overridden by the device's language setting; reminders, emails and jobs written in the server default or in the actor's language because the recipient's is not at hand; display-formatted values stored ("€2000,00", "03/04/2026"); copy hard-coded in code paths; a missing translation that shows a raw key, a blank or a half-translated screen and alerts nobody; a language list written only in the current language.
**Move:** Store an explicit choice on the person and negotiate from device settings only until one exists; keep language and region apart; resolve every outgoing message in the recipient's language; store locale-neutral values (decimal plus currency code, standard date-times) and format at the edge; keep copy as data in keyed catalogs; return stable error ids with optional text in the request's language; define the fallback chain to one default language and count each fallback; name each language in its own language.
**From:** W3C language negotiation; W3C spec best practices (§10.1, §10.7, §11.1); GLOB guide; W3C quick tips (navigation)

## Changing a live system

### Canary release · both
**Ask:** If this release is wrong, how many customers meet it before a named signal stops it, and does stopping it take one action?
**Smell:** Every user, tenant or chat gets the change at once; the rollout plan has no cohort order, no watch window and no abort threshold written down before release; success means "deploy finished" rather than errors, latency and the business number the change could hurt; the first cohort never exercises the risky path; many versions or cohorts are left running at once.
**Move:** Widen in stages (internal users, a small slice, wider, everyone), each with a watch window of minutes to hours, and write the abort signals before starting: technical health plus one business guardrail, each with a threshold. Roll back by routing the cohort to the old path, automatically on a clear regression, and retire old versions quickly so few run at once.
**From:** Canary release; Feature toggles

### Toggle debt · both
**Ask:** For each toggle this change adds, what kind is it, who owns it, what date or event removes it, and are both of its states tested?
**Smell:** A flag with no owner, reason or removal date; release flags still branching code long after full rollout; the same flag read inline in many places; tests that run only with the new state on; a per-customer entitlement or a permanent kill switch living inside a short-lived release flag; current flag state readable only in a data row or the code.
**Move:** Record kind, owner, reason and removal date with each toggle, open its removal task when it is created, fail a check once a release toggle is past its date, and cap how many are live. Read each decision through one named function, test both the expected production state and the new-toggles-off state, and show operators the live state with its change history.
**From:** Feature toggles
