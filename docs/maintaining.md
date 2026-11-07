# Maintaining the cards

For people who edit Nafis's knowledge. Agents never read this file: it holds the
admission rule, the card format, where each card lives, and the provenance of every
card. It sits outside the skill folder so installs stay light and the core stays free
of source names.

## What earns a card

A card earns its place only when the problem it catches reaches one of three people:
the end user (feels outcomes), the caller (feels the interface) or the operator (feels
errors, logs and recovery). Hygiene that only the code's maintainer feels belongs to
code-design and code-review skills.

Cards carry a way of thinking, not a stack: no framework, database, vendor, product or
project appears in a card's Ask, Smell or Move, so its Move translates into any
codebase. Cards are written in our own words, never copied: the sources are
copyrighted. At runtime the cards are the knowledge; sources are never fetched.

## Card format

```
### <Leading word> · <ui | backend | both>
**Ask:** the critical question (plan mode puts it to the brief)
**Smell:** how the problem shows up in a design, plan or diff (review mode hunts it)
**Move:** the fix direction (made concrete in a finding)
**From:** provenance, using the short names below
```

The leading word is a law, principle or pattern name that recalls the whole idea. A
Smell may mark one specific case as **shit**; that overrides the default severity.

## Where cards live

`skills/nafis/references/cards/`, split by member and by target so a run loads only
what its target needs: `ui` loads `ui` + `both`, `backend` loads `backend` + `both`.
Tesler's reliability cards sit in their own two files; a `ui` run loads
`tesler-reliability-both.md` only when the flow writes, waits, retries, shows times,
money or long lists, or ships behind a staged rollout. Each role prompt in
`skills/nafis/agents/` names the files it loads.

| File | Member | Tag | Cards |
|---|---|---|---|
| `cooper-ui.md` | Cooper | `ui` | 2 |
| `cooper-both.md` | Cooper | `both` | 24 |
| `tesler-ui.md` | Tesler | `ui` | 5 |
| `tesler-both.md` | Tesler | `both` | 15 |
| `tesler-backend.md` | Tesler | `backend` | 5 |
| `tesler-reliability-both.md` | Tesler | `both` | 9 |
| `tesler-reliability-backend.md` | Tesler | `backend` | 15 |
| `norman-ui.md` | Norman | `ui` | 15 |
| `norman-both.md` | Norman | `both` | 17 |
| `norman-backend.md` | Norman | `backend` | 7 |
| | | **Total** | **114** |

To add or change a card: write it in the format above, tag it, put it in the file for
its member and tag under the section it belongs to, and update the counts here and in
`README.md`. When a card refers to another by its leading word ("see Circuit breaker"),
keep that name stable.

## History

- 2026-11-07: five reading passes produced 105 drafts, merged to 95 by removing
  overlaps (Tesler's Law, Postel, Occam/YAGNI, Doherty/Feedback, mental/conceptual
  model, working memory/recognition). One conflict was resolved: variant formats of a
  known value are accepted; unknown input fields are rejected with a reason; clients
  ignore unknown fields in responses.
- 2026-11-08: 26 cards added (conversation, localization, AI and automation, change
  safety); six maintainer-only code smells removed under the admission rule
  (pass-through, special-general mixture, feature envy, primitive obsession, switch
  statements, hard to name); Load shedding merged into Bulkhead. Total 114.
- 2026-11-08: split per member and target for Nafis; card text unchanged.

## Provenance

Read once and distilled; agents never fetch these. Short names match the cards'
**From:** lines.

### Cooper

Read once on 2026-11-07 and distilled; agents never fetch these.
- Shape Up: https://basecamp.com/shapeup
- hacker-laws: https://github.com/dwmkerr/hacker-laws
- Alan Cooper, *About Face* and *The Inmates Are Running the Asylum*, through secondary
  sources (the primary pages did not load): Wikipedia (Alan Cooper; Persona), IxDF
  personas article, Coding Horror "Defending Perpetual Intermediacy", course notes on
  excise and goal-directed design
- Jobs to be Done: https://www.christenseninstitute.org/theory/jobs-to-be-done/ and
  HBS Working Knowledge milkshake articles
- Laws of UX: https://lawsofux.com
- Product runbook: the product-thinking ideas of a tool-specific skill, generalized
- Amazon shareholder letters: 2015 (Type 1 / Type 2 decisions,
  https://www.sec.gov/Archives/edgar/data/1018724/000119312516530910/d168744dex991.htm)
  and 2016 (decision speed, https://www.aboutamazon.com/news/company-news/2016-letter-to-shareholders)
- HAI: Guidelines for Human-AI Interaction (Amershi et al., CHI 2019),
  https://www.microsoft.com/en-us/research/publication/guidelines-for-human-ai-interaction/
- PAIR: People + AI Guidebook, https://pair.withgoogle.com/guidebook
- CDG: conversation design guidelines, https://developers.google.com/assistant/conversation-design/welcome
- NN/g: AI, chatbot and notification articles, https://www.nngroup.com

### Tesler

Read once on 2026-11-07 and distilled; agents never fetch these.
- Laws of UX: https://lawsofux.com
- John Ousterhout, *A Philosophy of Software Design*, through summaries of its red
  flags and principles (https://lethain.com/notes-philosophy-software-design/,
  https://yingang.github.io/aposd-zh/en/summary.html and others)
- Refactoring Guru, code smells: https://refactoring.guru/refactoring/smells
- hacker-laws: https://github.com/dwmkerr/hacker-laws
- Nielsen's usability heuristics and NN/g articles: https://www.nngroup.com
- CDG: conversation design guidelines, https://developers.google.com/assistant/conversation-design/welcome
- HAI: Guidelines for Human-AI Interaction (Amershi et al., CHI 2019) and its design patterns,
  https://www.microsoft.com/en-us/haxtoolkit/design-patterns/
- PAIR: People + AI Guidebook, https://pair.withgoogle.com/guidebook

### Tesler: reliability and data

Read once on 2026-11-07 and distilled; agents never fetch these. Short names in
**From:** lines:
- Cloud Design Patterns: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- microservices.io: https://microservices.io/patterns/
- Builders' Library: https://aws.amazon.com/builders-library/ (timeouts and retries,
  safe retries, queue backlogs, fallback, load shedding, static stability)
- Idempotency post: https://stripe.com/blog/idempotency
- Idempotency keys essay: https://brandur.org/idempotency-keys
- Twelve-Factor: https://12factor.net
- Don't Do This: https://wiki.postgresql.org/wiki/Don%27t_Do_This
- Use The Index, Luke: https://use-the-index-luke.com
- W3C internationalization (language negotiation, spec best practices, quick tips): https://www.w3.org/International/
- GLOB guide: https://learn.microsoft.com/en-us/windows/apps/design/globalizing/prepare-your-app-for-localization
- Parallel change: https://martinfowler.com/bliki/ParallelChange.html
- Online migrations: https://stripe.com/blog/online-migrations
- Builders' Library (rollback safety): https://aws.amazon.com/builders-library/ensuring-rollback-safety-during-deployments/
- Canary release: https://martinfowler.com/bliki/CanaryRelease.html
- Feature toggles: https://martinfowler.com/articles/feature-toggles.html

### Norman

Read once on 2026-11-07 and distilled; agents never fetch these. Short names in
**From:** lines:
- DOET: Don Norman, *The Design of Everyday Things*, verified through NN/g and IxDF pages
- Nielsen H1–H10 and NN/g: https://www.nngroup.com/articles/ten-usability-heuristics/
  and related NN/g articles (gulfs, slips, mistakes, modes, response times, progress)
- Laws of UX: https://lawsofux.com
- AIP-n: https://google.aip.dev
- ZRG #n: https://opensource.zalando.com/restful-api-guidelines/
- RFC 9457: https://www.rfc-editor.org/rfc/rfc9457
- TW-Errors: https://developers.google.com/tech-writing/error-messages
- CLL: https://stripe.com/blog/canonical-log-lines
- OWASP-EH, OWASP-Log, OWASP-IV: https://cheatsheetseries.owasp.org
- SRE-Mon: https://sre.google/sre-book/monitoring-distributed-systems/
- Falsehoods: https://github.com/kdeldycke/awesome-falsehood and the linked articles
- Grice: the cooperative principle and maxims, https://en.wikipedia.org/wiki/Cooperative_principle
- CDG: conversation design guidelines, https://developers.google.com/assistant/conversation-design/welcome
- W3C: text size in translation, quick tips, spec best practices, https://www.w3.org/International/
- CLDR plural rules: https://www.unicode.org/cldr/charts/latest/supplemental/language_plural_rules.html
- L10n dev guide: https://mozilla-l10n.github.io/documentation/localization/dev_best_practices.html
- GLOB guide: https://learn.microsoft.com/en-us/windows/apps/design/globalizing/prepare-your-app-for-localization
- HAI: Guidelines for Human-AI Interaction (Amershi et al., CHI 2019) and design patterns
- PAIR: People + AI Guidebook, https://pair.withgoogle.com/guidebook
