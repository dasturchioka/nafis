# Norman cards: backend

Loaded for `backend` targets, together with `norman-both.md`.

## Constraints, slips and errors

### Problem details · backend
**Ask:** Can the caller's code decide what to do with an error (fix the input, wait, or stop) without parsing the human text?
**Smell:** Failures returned as success with a flag inside; bare strings; one generic 400 or 500 for everything; a different error shape per endpoint; a bulk call that returns plain success while some items failed; stack traces, paths or versions in the body.
**Move:** Use one error shape everywhere: the most specific status, a stable machine reason per error kind that never changes, a constant title, a per-occurrence detail aimed at fixing, field violations that point at the field, per-item results for bulk calls, and a wait hint on rate limits; check permission before existence.
**From:** RFC 9457; AIP-193; AIP-194; ZRG #152, #153, #176, #177, #220; OWASP-EH

## A contract the caller reads correctly

### Naming · backend
**Ask:** Can a caller guess a field's meaning, unit and type from its name alone, and does one concept carry one name across the whole API?
**Smell:** `timeout`, `size` or `delay` with no unit; `date` holding an instant; `created`, `created_at` and `creation_time` side by side; vague `data`, `info` or `items`; verbs or prepositions in field names (`reason_for_error`); `name` used for a display label; `status` meaning lifecycle in one place and request outcome in another.
**Move:** Name fields as nouns with the adjective first; add a unit suffix (`_seconds`, `_bytes`, `_km`), `_count` for counts, `_time` for instants and `_date` for calendar dates; use plurals for lists, booleans without `is_`, domain-specific resource names, and one name per concept everywhere.
**From:** AIP-122, AIP-140, AIP-141, AIP-142, AIP-216; ZRG #142

### Type as constraint · backend
**Ask:** Does each field's type rule out wrong values, or must the caller read prose to avoid them?
**Smell:** A nullable boolean with three hidden meanings; null meaning something different from an absent field; empty lists sent as null; a closed set typed as a free string; a duration or timestamp as a bare integer with no unit; phone numbers or ids as integers; length limits unstated or counted in bytes.
**Move:** Treat null and absent alike, send `[]` for an empty list, replace a nullable boolean with a named enum, use standard formats (date-time with an explicit offset; ISO country, language and currency codes), keep ids and phone numbers as strings, state limits in characters, and mark each field required, optional or output-only.
**From:** ZRG #122, #123, #169, #170, #238; AIP-126; AIP-142; AIP-203; AIP-210

### Compatibility · backend
**Ask:** When the contract grows next month, will today's callers keep working, and does the contract tell them in advance what can grow or go away?
**Smell:** A response enum (states, types, channels) declared closed although it will grow; a client switch with no branch for unknown values; a field kept while its meaning, default or format changes; a new required input or stricter validation; a deprecation announced only in chat.
**Move:** Mark growing enums as extensible and require a fallback for unknown values; have readers ignore unknown fields and keep them on write-back; change only by addition (new name beside the old); mark deprecations in the spec and in responses so callers can alert on them.
**From:** AIP-180; AIP-126; AIP-216; ZRG #106, #107, #108, #112, #187, #189

### Pagination contract · backend
**Ask:** Can a caller tell from the response alone whether it now has the whole list?
**Smell:** A list endpoint with no paging that grows past a few hundred items; end of list inferred from a short page; undocumented behaviour for an oversized or zero page size; filters changed between pages without complaint; paging added later, so old callers take page one for everything.
**Move:** Page from day one with a documented default and maximum (clamp values above the maximum, reject negatives); return an opaque token and make an empty next token the only end signal; reject changed parameters mid-sequence; check authorization on every page.
**From:** AIP-158; AIP-180; ZRG #159, #160

## The operator's view

### Canonical log line · backend
**Ask:** Can an operator answer "what happened to this request or job, for whom, how long did it take, and why" from one line, without stitching a trace together?
**Smell:** Free-text prints with values spliced in and no request id; nothing logged when an exception unwinds; field names that drift between releases; secrets, tokens or full customer messages in logs; auth failures, access denials and admin changes not recorded.
**Move:** Emit one structured key=value line per request or job on the always-run exit path, where a logging fault never fails the work: action, outcome and error id, duration, actor and tenant ids, a request id carried to downstream calls, and downstream call counts and time. Keep field names stable, mask secrets, escape line breaks in user input, and record security events with who, what, object and result.
**From:** CLL; OWASP-Log; ZRG #233

### Actionable alert · backend
**Ask:** When this alert fires, is a user being hurt now, and is there something a human must decide to do?
**Smell:** Pages on causes (CPU, one retry, queue depth) instead of symptoms; alerts people routinely ignore; average latency; failed and successful requests timed together; a condition a script could fix paging a human; no alert at all when a job silently stops producing.
**Move:** Page on user-visible symptoms through the four golden signals: latency (tail, success and failure apart), traffic (including a drop to zero), errors (including wrong content) and saturation. Page on a cause only when it is definite and imminent ("disk full in 4 h"); automate rote fixes; review rules that fire less than about once a quarter for removal.
**From:** SRE-Mon
