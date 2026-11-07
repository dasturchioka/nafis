# Tesler reliability cards: backend

Failure, data and release complexity behind the interface. Loaded for `backend` targets.

## Duplicates, retries and failing dependencies

### Idempotent consumer · backend
**Ask:** When this message, webhook or job is delivered a second time, as it will be, what changes in the data and what does the customer receive?
**Smell:** A handler that applies a delta or side effect per delivery (balance minus amount, counter plus one, notification sent); the "processed" marker written after the effect or in another store; a dedup key taken from the delivery attempt or receive time; one marker per message shared by several subscribers; markers that expire before a redelivery or dead-letter replay can arrive.
**Move:** Prefer naturally idempotent writes (upsert on a business key, a full new state instead of a delta). Otherwise insert a stable producer-assigned key under a uniqueness constraint in the same transaction as the effect; for effects outside that transaction, mark in-progress, call with a key derived from your record, then mark done; keep markers longer than the longest redelivery path and count duplicates as a metric.
**From:** Cloud Design Patterns, microservices.io, Idempotency keys essay

### Backoff with jitter · backend
**Ask:** For each remote call: what is the timeout, which single layer retries, on which errors, how many times, and what does the dependency see when every caller retries at once?
**Smell:** A call with no timeout or a library default; retry loops at several layers (three tries at each of five layers is 243 calls against the failing dependency); unbounded or fixed-delay loops without jitter; retries of validation errors or of writes without an idempotency key; a downstream throttle hidden behind a silent retry or a generic error.
**Move:** Give every remote call a connect and a request timeout taken from the dependency's tail latency (for example p99.9 for a 0.1% false-timeout rate, padded for network distance). Retry at one layer only, with capped attempts and total time, capped exponential backoff plus jitter, retry-after hints honoured and a retry budget; jitter scheduled jobs too, and alarm on retry rate.
**From:** Builders' Library, Cloud Design Patterns, Idempotency post

### Circuit breaker · backend
**Ask:** When a dependency stays down for ten minutes, does the system fail fast into a degraded mode that already runs every day, or do requests pile up waiting and then switch to a path nobody has exercised?
**Smell:** Threads and connections held until a long timeout while a dependency is down; one breaker shared by independent tenants or partitions; retries that ignore an open breaker; a fallback that only runs during outages (cache down, so every server hits the origin directly); recovery that needs resources created or reconfigured mid-incident.
**Move:** Trip per resource on failure rate within a window, fail fast while open, admit a few probe calls before closing, publish state changes and allow manual override (or use a retry budget, which has fewer modes to test). Degrade to data pushed ahead of need or the last known good state, run both paths continuously so failover is proven, and pre-provision capacity.
**From:** Cloud Design Patterns, Builders' Library

## Multi-step changes and consistency

### Transactional outbox · backend
**Ask:** If the process dies between committing the change and publishing the event, sending the message or enqueuing the job, does the rest of the system still hear about it, and only if the change committed?
**Smell:** Commit the row, then publish or send in the same handler (dual write); publishing or enqueuing inside the transaction before commit, so a rollback leaves a ghost event; a post-commit "fire and forget" whose failure is only logged.
**Move:** Write the outgoing message or job into an outbox in the same transaction as the business change; a relay (polling publisher or log tailing) publishes after commit and marks it sent. Expect the relay to duplicate, so consumers are idempotent; keep per-entity order and alarm on the age of unsent rows.
**From:** microservices.io, Idempotency keys essay, Cloud Design Patterns

### Saga · backend
**Ask:** When step three of four fails after earlier steps committed elsewhere (payment taken, stock held, customer told), what brings the whole operation to a valid end state, and who is told if that fails too?
**Smell:** A multi-step flow with no persisted progress; "rollback" that restores an old snapshot over concurrent changes; compensations that are not idempotent; an irreversible step (capture, customer message, external booking) placed before validations finish; a failed compensation that only logs.
**Move:** Persist each step's recovery point and its undo in that step's own commit; order steps compensable, then the pivot (point of no return), then retryable-only, with irreversible steps after every check. Retry transient faults before compensating, make compensations idempotent and resumable, and route stuck or ambiguous high-impact cases to an operator with a correlation id.
**From:** Cloud Design Patterns, microservices.io, Idempotency keys essay

## Queues, overload and isolation

### Queue-based load leveling · backend
**Ask:** If producers outrun consumers for an hour, how long until fresh work is fast again, and who notices before customers do?
**Smell:** A queue added "for resilience" with no age-of-oldest-message alarm; consumers that scale out without a cap on their combined downstream rate (overload moved, not removed); strict oldest-first draining so a stale backlog delays every fresh item; work that outlives its lease and is redelivered while still running.
**Move:** Alarm on the age of the oldest first-attempt message, not only depth, and budget recovery (an hour-long outage costs about another hour at double capacity). Bound consumer concurrency to what the downstream tolerates; sideline aged messages to a backlog processed after live work; drop work a later full sync supersedes; extend leases with heartbeats while working; keep polling headroom.
**From:** Cloud Design Patterns, Builders' Library

### Dead-letter · backend
**Ask:** What happens to a message or job that fails on every attempt, and who finds out?
**Smell:** Unlimited redelivery that burns capacity; a handler that catches every error and acknowledges anyway (silent loss); one failing message blocking the rest of its ordered group; a dead-letter store with no alarm, owner or replay path.
**Move:** Cap delivery attempts, then move the message with its failure reason, attempt count and correlation id to a dead-letter store; treat any arrival there as a bug and alarm on it. Provide a documented, idempotent replay after the fix, and decide explicitly whether an ordered group pauses or skips past a poison message.
**From:** Cloud Design Patterns, Builders' Library

### Sequential convoy · backend
**Ask:** Which events must apply in order, what key defines that order, and what happens when one arrives late or twice?
**Smell:** Parallel consumers on events for the same entity (an update processed before its create, a status moving backwards); ordering assumed from a channel that does not promise it; one global consumer used to get order, capping throughput; a key the producer sets inconsistently.
**Move:** Order per entity key and run different keys in parallel; carry a sequence number or version so consumers reject stale or out-of-order updates; where possible make updates order-independent or carry full state. Pick a key fine enough for parallelism yet covering every message that must stay in order.
**From:** Cloud Design Patterns

### Bulkhead / Load shedding · backend
**Ask:** When one tenant, dependency or non-critical feature misbehaves, or traffic passes capacity, what else stops working, and does the work already accepted still finish in time?
**Smell:** One connection, worker or queue pool shared by every dependency and tenant; a slow optional feature (reports, bulk notifications) competing with the money or messaging path; one customer's bulk import starving everyone; unbounded in-memory queues; overload answered by slowing every request until nearly all fail, or with a generic error.
**Move:** Give each dependency and criticality class its own pool or concurrency cap, and per-tenant limits with burst allowance. Past capacity, reject early and cheaply with a retry-after hint and the limit's name, bound queue wait, drop requests past their deadline, and switch off non-essential features first. Watch each partition's health on its own.
**From:** Cloud Design Patterns, Builders' Library

## Data correctness

### Null semantics · backend
**Ask:** What does this negative filter or comparison return when a value, or the set it is compared against, contains null?
**Smell:** "x not in (subquery)" where the subquery can yield a null, so the filter silently returns no rows at all; "not equal" filters that quietly drop rows where the column is null; nullable columns where absence is not a real business state.
**Move:** Express exclusion as "not exists" (an anti-join) or remove nulls from the set explicitly; decide per column whether unknown is a real state and declare not-null where it is not; give every negative filter a test row containing a null.
**From:** Don't Do This

### Index path · backend
**Ask:** For every query a user or a hot job waits on, is there an index that narrows by all its conditions, and was it checked at production volume?
**Smell:** A new filter or sort column with no index; the column wrapped in a function, cast or arithmetic (lower-casing, truncating a date) so the index cannot be used; an index that matches only part of the filter and scans the rest; one query per row of a list (query count grows with rows); timing measured only on small development data.
**Move:** Index the access conditions (equalities first, then the range) and filter on the bare column or index the same expression; fetch related rows with one join or one batched lookup and keep queries per request constant. Test with realistic data volume and judge the trend, not one timing.
**From:** Use The Index, Luke

## Processes and environments

### Disposability · backend
**Ask:** If this process is killed mid-request or mid-job (deploy, scale-in, crash), is the work resumed, retried safely or lost?
**Smell:** In-memory timers, queues or schedules for future work (a reminder in two hours); sessions, uploads or rate-limit counters kept in one instance's memory or disk; a job acknowledged before its work commits; no graceful stop; a liveness check that calls the database, so one database blip restarts every instance.
**Move:** Keep durable state and pending work in a backing service, acknowledge after commit and make jobs re-entrant; on stop, refuse new work, finish or return in-flight jobs and release locks, and treat sudden death as normal. Start fast and gate traffic on readiness; keep liveness shallow and readiness limited to required dependencies, each with its own timeout.
**From:** Twelve-Factor, Cloud Design Patterns

### Config · backend
**Ask:** Can each environment be configured, and a limit tuned during an incident, without a code change, and does every environment run the same kinds of backing services?
**Smell:** Credentials, endpoints, limits or thresholds hard-coded or branched on an environment name; limits that need a deploy to change mid-incident; tests or local runs on a lightweight substitute for the real database, queue or cache; migrations and one-off fixes run by hand from a developer machine with different code or config.
**Move:** Keep per-deploy settings in the environment, one granular value each, with no credentials in code (could the code be published today?), so a backing service can be swapped by config alone; make operational limits changeable at runtime. Run the same type and version of each backing service everywhere, and ship one-off admin tasks with the release, run in the same environment and config.
**From:** Twelve-Factor, Cloud Design Patterns

## Changing a live system

### Expand–contract · backend
**Ask:** Does this schema or contract change ship as steps that each run beside the previous version and roll back on their own, and what evidence clears the old shape for removal?
**Smell:** One release renames, retypes or moves a field and updates every reader and writer at once; only new records get the new shape because the backfill is "later"; reads move to the new shape with no side-by-side comparison; the old shape is dropped in the release that adds the new one; or the contract step has no owner or date, so both shapes live on and drift apart.
**Move:** Ship expand (new shape added, old still authoritative), then write both (in one transaction when they share a store), then a throttled, resumable, idempotent backfill off the hot path. Read both on live traffic and alarm on every mismatch; switch reads at zero mismatches, then writes; make any leftover use of the old shape fail loudly, and contract only when measured use is zero. Caller-facing deprecation: Norman: Compatibility.
**From:** Parallel change; Online migrations

### Rollback safety · backend
**Ask:** While old and new versions run side by side, and after a rollback, can the old version read every record, message and state value the new one has already written?
**Smell:** The release that first emits a new format, field, state value, message shape or timing assumption (a heartbeat or polling interval) is also the release that first learns to read it; rollback means "redeploy the previous build" and has never been rehearsed; a rollout counts as done when most instances are healthy, hiding stragglers. A rollback that strands data the old version cannot read is **shit**.
**Move:** Split the change into prepare (readers accept the new form, writers still emit the old) and activate (writers switch), with a bake period between and every instance confirmed on prepare first; store a format version with the data and keep unknown fields on write-back. Rehearse deploy-to-half, deploy-to-all and full rollback under realistic traffic, background jobs included.
**From:** Builders' Library (rollback safety); Parallel change
