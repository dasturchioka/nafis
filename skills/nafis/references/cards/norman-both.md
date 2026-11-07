# Norman cards: both

Loaded on every Norman run, `ui` and `backend` alike.

## See it, predict it

### Mental model / Conceptual model · both
**Ask:** What does the person expect to happen when they act, and can a newcomer infer what this affects from what the interface itself shows, without reading code or asking?
**Smell:** A word or state means something else internally: "delete" only hides, "saved" is still queued, "sent" comes before delivery. Effects with no visible trace: a plain edit that also messages customers, a setting that silently changes another screen, an update call whose response never mentions its side effects. Internal terms (table names, raw codes, ids) in user copy.
**Move:** Match names, structure and sequence to the user's model, learned from research. Make every effect visible: name side effects in the action ("Save and notify customer"), echo them in confirmations and responses, and keep plain updates free of side effects. Keep backend semantics equal to what the interface promises.
**From:** DOET (conceptual model, system image); Laws of UX (Mental Model); Nielsen H2; AIP-134

### Mode · both
**Ask:** Does the same input ever produce a different result depending on a state the person cannot see right now?
**Smell:** A bot that reads the same message differently depending on a hidden conversation step; a test/live or draft/live switch buried in settings while both modes look identical; sandbox and live credentials that return indistinguishable responses; a Reply and Reply-all style pair of near-identical actions.
**Move:** Where a slip costs money, data or embarrassment, use separate controls instead of a mode; otherwise show the active mode with at least two cues wherever it applies, and echo it in every confirmation and response.
**From:** NN/g modes; DOET (slips)

## Feedback and state

### Feedback / Doherty Threshold · both
**Ask:** Does every action get feedback that says it was received, what is happening and how it ended, with an indicator matched to the wait?
**Smell:** No pressed or busy state, so people submit twice; a bare spinner on a 30-second task; static "Loading…"; a bot that stays silent while it works; a success message when the work was only queued. An interactive endpoint with no latency budget, or slow work run synchronously.
**Move:** Acknowledge within 0.1 s and aim for a useful response within 400 ms; past 1 s show busy; for 2–10 s use a looping indicator that names what is loading; from 10 s show percent-done or a "3 of 50" counter plus cancel. Finish slow work asynchronously and end with an explicit outcome that tells accepted, done and failed apart.
**From:** Nielsen H1; NN/g response-time limits and progress indicators; Laws of UX (Doherty Threshold)

### State · both
**Ask:** Can the person or caller read, without interpretation, what state each thing is in now (gulf of evaluation), and is that state one value that only the system moves?
**Smell:** Several booleans that can contradict each other (active and deleted); state writable through a generic edit; "Processed" that could mean queued or finished; a toggle readable only by colour; items stuck in "pending" with no age or reason.
**Move:** Keep one output-only state with in-progress names (CREATING, RUNNING) and terminal past-tense names (SUCCEEDED, FAILED); move it only through named actions that refuse invalid transitions with a precondition error; show state in words, never by colour alone.
**From:** NN/g gulfs; AIP-216; AIP-203

### Job status · both
**Ask:** While long work runs, can the end user, the caller and the operator each tell how far it got, whether it is stuck and why it failed, without asking support?
**Smell:** A status that only ever says "processing"; no start time or last-progress time, so stuck and slow look the same; a failure reason that exists only in a server log; a new item that looks ready before it is; a bulk job with a single verdict for 1,000 items.
**Move:** Report progress in countable units ("312 of 1,000 rows"), start and last-progress times, and a final reason the reader can act on; show the target in reads with a not-ready state while work runs; record per-item failures in the job's metadata, each in the standard error shape.
**From:** AIP-151; AIP-133; NN/g progress indicators; ZRG #152

## Constraints, slips and errors

### Constraint · both
**Ask:** Does the design make wrong input impossible, or flag it while the person is still entering it, rather than reporting it after commit?
**Smell:** Free text where the choices are finite; a return date accepted before the departure date; a length limit revealed only on submit; checks that run only in the client; a server that silently ignores misspelled or unknown input fields.
**Move:** Offer only valid choices and disable impossible ones with a stated reason; show the remaining room as a limit approaches; enforce the same allowlist rules (type, range, length, cross-field) on the server at every trust boundary; reject unknown input fields with a clear error.
**From:** DOET (constraints); Nielsen H5; NN/g slips, mistakes; OWASP-IV; ZRG #109

### Slips and mistakes · both
**Ask:** For each costly or hard-to-reverse action, can the person see its exact scope before committing, and recover afterwards?
**Smell:** "Are you sure? Yes/No" on every action, so people click through by habit; bulk actions that show no count; an irreversible send or delete with no undo; a caller who must run the real change to learn whether it is allowed.
**Move:** Prefer undo with a short grace window over confirmation; reserve confirmation for destructive actions and put the action and scope on the button ("Delete 24 photos"); preview slow or wide changes; offer a validate-only call that runs the same permission and validation checks with no side effects.
**From:** DOET (slips vs mistakes); NN/g slips, mistakes; Nielsen H3, H5; AIP-163

### Error message · both
**Ask:** Does each error say what went wrong, show the value and the rule it broke, and tell this reader what to do next, in words they use?
**Smell:** "Invalid input", "Something went wrong", or "Permission denied" with no hint of who has access; codes or jargon shown to end users; blame ("You entered…"), jokes or long apologies; an error placed far from its field or signalled by colour only; the same failure worded differently in two places.
**Move:** Give cause, offending value, limit and fix, e.g. "Attachments total 14 MB; the limit is 10 MB. Remove a file or share a link." Add a valid example; keep the tone neutral and positive; place the error next to its field with text or an icon; use one message per problem everywhere.
**From:** Nielsen H9; TW-Errors; RFC 9457 (detail)

### Silent failure · both
**Ask:** When something fails (in a request, a background job, a fallback or a catch block), does someone who can act find out, with the root cause intact?
**Smell:** Catch-and-continue; a fallback value served with no signal; a generic "server error" that drops the original cause; partial success reported as success; a job that fails into a log nobody reads; a bot that simply never answers.
**Move:** Route every failure to a party who can act (user message, caller error, operator log or alert) and chain the original cause; give outside readers a safe message plus a correlation id, and give inside readers full detail under the same id; raise errors where they occur.
**From:** TW-Errors (error handling); OWASP-EH; Nielsen H9

## Worst-case inputs

### Falsehoods: names and text · both
**Ask:** What happens with a one-letter name, a 100-character name, a non-Latin script, an apostrophe or hyphen, an emoji, or a combining accent?
**Smell:** Mandatory first and last name fields; letters-only patterns; length caps sized for Western names; byte-based truncation that splits a character; case conversion assumed to keep length (ß → SS); un-normalised comparison; profanity filters that block real names; a name used as an identity key or assumed never to change.
**Move:** Use one free-form full-name field, plus an optional "what should we call you"; accept any script with limits counted in characters; normalise (NFC) before comparing or enforcing uniqueness; truncate on whole characters; key identity on ids; render the worst strings in the UI to check wrapping and ellipsis.
**From:** Falsehoods (names, plain text); AIP-210; OWASP-IV

### Falsehoods: time · both
**Ask:** Does it still work on the night the clocks change, for a person in another time zone, and for an event booked a year ahead?
**Smell:** Durations computed as "days × 24 h"; an offset stored as if it were a zone; zone abbreviations accepted as input ("CST" names several zones); a local time that never occurs or occurs twice; a zone guessed from the phone prefix; a trusted client clock.
**Move:** Compute "today", "tomorrow" and reminders in the person's zone; define what happens in the skipped hour and the repeated hour; keep zone rules updatable; show every time with its zone or relative to now. Storage rules live in Tesler: Instants.
**From:** Falsehoods (time zones; date-time and zones; phone numbers); ZRG #169

### Falsehoods: contact data and money · both
**Ask:** Will a real customer's phone number, email, address or price pass validation, survive storage unchanged, and stay tied to the right person?
**Smell:** Phone numbers stored as numbers, losing the leading zero, "+" and extension; country inferred from the calling code (+1 and +7 each span several countries); every number assumed SMS-capable and owned forever, although numbers get recycled; email patterns that reject valid addresses or strip plus-tags; a mandatory postcode or house number; two decimals and a leading symbol assumed for every currency.
**Move:** Keep the phone number as entered plus its international form, and re-verify ownership over time; check email lightly, then prove it by sending a message; take addresses as free-form lines with an optional postcode; allow several contacts per person; format money per currency (minor units, symbol position, separators) and per locale.
**From:** Falsehoods (phone numbers, email, addresses, prices); OWASP-IV; ZRG #173

## Conversations

### Grice's maxims · both
**Ask:** Read as a reply from a cooperative partner, does this message give as much as the moment needs and no more, claim only what the system can back, stay on what was asked, read one way only, and imply nothing false?
**Smell:** Too little: the one detail needed to act (time, amount, next step) is missing. Too much: background and edge cases before the answer. Unbacked: a confident figure or promise the system has no data for. Off topic: a promotion or pleasantry opening an answer, or "see our help page" in place of the answer. Unclear: jargon, an ambiguous reference, steps out of order. False implicature: an offered slot, product or option that is not actually available, since people take anything offered as offered in good faith.
**Move:** Test every template and generated reply against the four maxims and against what it implies: give the specific answer with only the caveats accuracy needs and put the rest behind a follow-up option; say plainly when data is missing; keep promotion out of answers; offer only alternatives the system has checked; reword anything a reader could take two ways.
**From:** Grice; CDG (learn about conversation); NN/g less chat, more answer; NN/g site chatbots

### Repair · both
**Ask:** When the system did not understand, heard nothing, or cannot do the request, does each next turn add new help, and does the last one hand the person to a working route with everything already said?
**Smell:** The same prompt repeated word for word; "I didn't understand" with no hint of what would work; a misread reply that restarts a long flow; endless retries, or a way out that appears only after many failures; a confident wrong answer instead of an admission; "I can't do that" buried under unrequested advice; "try again later" when later will not help; after a transfer, the human asks the original question again.
**Move:** Treat the input as meant cooperatively and escalate: a short re-ask with the question condensed, then new help (examples, options, the likely reading), then after two failed attempts a graceful exit that names another way to finish. Say "can't" in the first sentence with the reason and the alternative. Keep a human or another route reachable at any point; the handoff itself follows Fallback ladder (Tesler) and Handoff.
**From:** CDG (errors); NN/g chatbots; NN/g customer-service chat; NN/g less chat, more answer

## Localization

### Plural categories · both
**Ask:** Does every message that contains a count choose its wording through the target language's plural categories, rather than an English one-versus-many switch?
**Smell:** Code that picks a singular or a plural string by checking whether the count equals one; "item(s)" or "minute(s)"; separate translation keys for singular and plural; "You have 0 new messages"; decimal counts and ordinals ("1st", "2nd") worded by hand.
**Move:** Keep one message per count with a branch per category (zero, one, two, few, many, other) filled per language from the shared plural-rule data; "other" is always required and is the fallback. Expect the spread: Japanese, Chinese and Korean use only "other"; English uses one and other; Russian uses one, few, many and other, with "one" covering 1, 21 and 101 but not 11; French puts 0 in "one"; Arabic uses all six; 1.5 falls in a different category per language. Write a separate zero message where it reads better ("No new messages"), test with 0, 1, 2, 5, 11, 21, 101 and 1.5, and fall back to "Label: 3" where a format cannot express plurals.
**From:** CLDR plural rules; CLDR plural-rule guidance; L10n dev guide; GLOB guide (make localizable)

### Composite messages · both
**Ask:** Can a translator produce a grammatical sentence from each string without seeing the code, with every inserted value placed, inflected and formatted for the target language?
**Smell:** A sentence glued from separately translated pieces ("You have " + n + " new " + noun); one string reused where meanings differ (a noun and a verb, one on/off pair for different features); "The {0} could not be saved" filled with nouns whose gender changes the article; placeholders with no note on what fills them; links, markup, separators or a date pattern fixed inside a string; numbers, dates, money and percentages formatted by hand; a value in another script dropped into right-to-left text without isolation.
**Move:** Translate whole sentences with named or numbered placeholders that translators may reorder; give each context its own string id and a note describing every placeholder; write one full sentence per noun where agreement differs; keep markup and links outside the text; format numbers, dates, times, currency, units and percentages for the reader's locale at display time (digits, separators, spacing and field order all vary); isolate the direction of each inserted value in mixed-direction text.
**From:** W3C quick tips; W3C spec best practices (§6.6, §10.4); L10n dev guide; GLOB guide (make localizable)

## AI and automation

### Handoff · both
**Ask:** At each moment, can the customer, the staff member and the operator tell whether the AI or a human is handling this, what the AI has already done, and who acts next?
**Smell:** A handoff nobody announces: the customer keeps writing to a bot no one watches, or a human steps in and the bot keeps answering too; "connecting you to an agent" with no time expectation; staff who cannot tell which messages the AI sent; an agent that books, sends or edits with no trail of its actions; an escalation queue with no reason shown; an approval prompt that says "Proceed?" without the exact action the AI will take. A customer waiting on a handoff nobody picked up is **shit** (see Silent failure).
**Move:** Label AI-authored content for customers and staff alike. Announce every handoff in both directions with who has it now, why, and when to expect a reply, early enough for the person to react, plus what they can do meanwhile. Keep a readable log of each action the AI took on someone's behalf, with undo where possible, and put the exact action, amount and recipient on any approval (see Slips and mistakes).
**From:** HAI pattern 10-C (hand-off pitfalls); PAIR (Errors + graceful failure: return control); NN/g (5 dimensions of site chatbots; AI agent studies)
