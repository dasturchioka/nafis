# Norman cards: ui

Loaded for `ui` targets, together with `norman-both.md`, and for a `backend` target whose change produces user-facing output (messages, notifications, error texts).

## See it, predict it

### Signifier · ui
**Ask:** From the screen or message alone, can a first-time user tell what is actionable and what each action will do?
**Smell:** Tappable text that looks like body copy; headings or badges styled like buttons; icon-only controls; a placeholder used as the label, which vanishes on typing; chat buttons labelled with a noun ("Orders"); a hint telling users where to tap, which shows the cue is missing or false.
**Move:** Give every interactive element one conventional cue, applied the same way everywhere, and a verb label that names its result ("Cancel order"); remove interactive styling from static content; pair any non-standard icon with text.
**From:** DOET (affordance, signifier); NN/g clickable elements; Nielsen H4

### Mapping · ui
**Ask:** Do a control's label, position and direction predict its effect, so the person can plan the action that reaches the goal (gulf of execution)?
**Smell:** A switch whose label states the current state while users read it as the action; "Hide X" set to Off (a double negative); a control placed far from what it changes; one button doing different things on different screens.
**Move:** Put controls next to the thing they change, word settings positively so On means the thing happens, label actions by their outcome, keep one label → one effect everywhere, and make a control behave like the physical metaphor it borrows.
**From:** DOET (mapping); NN/g gulfs; Nielsen H2, H4

### Jakob's Law · ui
**Ask:** Does this behave the way the products our users spend most of their time in already behave?
**Smell:** A custom pattern replaces a familiar one: unusual navigation, a novel icon or gesture for a common action, a non-standard form flow. A redesign replaces a learned flow overnight.
**Move:** Follow established conventions so that effort goes into the task. When changing a learned pattern, offer a transition period in which users can switch back to the familiar version.
**From:** Laws of UX

### Fitts's Law · ui
**Ask:** Is each frequent or important target large, close to where attention already is, and spaced from its neighbours?
**Smell:** Tap targets are small, or the primary action sits far from the content it acts on. Destructive and safe actions are packed together, or controls sit in hard-to-reach corners on touch screens.
**Move:** Size targets generously, place them near the user's focus and within easy reach, and add spacing so that a near miss lands somewhere harmless.
**From:** Laws of UX

## Feedback and state

### Peak-End Rule · ui
**Ask:** What are the most intense moment and the final moment of this journey, and is each one designed on purpose?
**Smell:** An error, payment failure or long wait at the peak gets a generic message. The flow ends on a blank screen, a raw status or an upsell, and success goes unacknowledged.
**Move:** Design the peak and the ending deliberately: graceful recovery where stress is highest, and a clear completion with the next step. Bad moments are remembered more strongly than good ones.
**From:** Laws of UX

## Grouping and emphasis

### Law of Proximity, Common Region & Uniform Connectedness · ui
**Ask:** Do spacing, containers and connectors group exactly the things that belong together?
**Smell:** A label sits closer to the wrong field, or spacing is equal within and between groups. A button floats away from the item it acts on. Nested borders box everything, or sequential steps have no visual link.
**Move:** Use tight space within a group and wide space between groups. Give a real unit a shared background or border, and connect sequence with a line or arrow, so the layout states the structure before the text is read.
**From:** Laws of UX

### Law of Similarity & Prägnanz · ui
**Ask:** Do elements that look alike work alike, and does each one read as a single, simple form?
**Smell:** One colour or icon means two things, or a destructive action is styled like a routine one. Look-alike elements behave differently. Icons or charts are busy enough to need decoding.
**Move:** Give each role one consistent look (colour, shape, size). Reduce icons and visuals to the plainest shape that still carries the meaning.
**From:** Laws of UX

### Von Restorff Effect / Selective Attention · ui
**Ask:** Is the one thing the user must notice or do also the one thing that stands out, in the path of their task?
**Smell:** Several highlights, badges or primary buttons compete, and a key warning blends in. Important content looks or sits like an ad (banner blindness). A significant change happens quietly away from the user's focus (change blindness). Emphasis relies on colour alone.
**Move:** Make one element per view distinct and keep emphasis rare. Put critical information in the task path, apart from promotional content, and signal each significant change with one clear cue near where the user is looking. Pair colour with shape, text or weight; offer reduced motion when motion carries importance.
**From:** Laws of UX (Von Restorff Effect; Selective Attention)

### Serial Position Effect · ui
**Ask:** Are the most important items at the start and end of this list, menu or message?
**Smell:** A key action or instruction is buried mid-list or mid-navigation. A bot message puts its essential line in the middle paragraph.
**Move:** Put the most important items first and last and the least important in the middle. In messages, lead with the answer and close with the next step.
**From:** Laws of UX

### Aesthetic-Usability Effect · ui
**Ask:** Does this succeed on the task, or does it only look like it does?
**Smell:** Usability is judged from a polished mockup or from satisfaction ratings alone, and participants praise a flow they failed to finish. Rough visuals undercut trust in a flow that works.
**Move:** Judge usability by task completion and errors, not impressions. Treat visual polish as what earns trust and tolerance once the flow already works.
**From:** Laws of UX

## Conversations

### Confirmation in conversation · ui
**Ask:** Does the system show what it understood without stopping the person, ask for an explicit yes only where a misreading is costly or the action is hard to undo, and accept a one-step correction?
**Smell:** A clear "yes" echoed back as a question; a message sent on the person's behalf, a payment or a deletion carried out with no read-back of the exact text or amount; "Saved" or "Done" that never says what and where, so people doubt it happened; a "no" that cancels the whole flow, so fixing one value means starting over.
**Move:** Confirm implicitly by default: carry the understood values into the next turn ("Two people, Friday at 7. Inside or on the terrace?") and report finished actions with what and where. Ask explicitly only before hard-to-undo actions and for costly values (names, addresses, text sent for the person), showing exactly what will happen. Read "no, Saturday" as a change to that one value, keep the rest, and let any earlier answer be edited without a restart.
**From:** CDG (confirmations; questions); NN/g chatbots

## Localization

### Text expansion · ui
**Ask:** Does every label, button, message and stored text field still fit when the translation is up to three times longer, set in a taller script, or one long compound word that cannot wrap?
**Smell:** Buttons, tabs, badges and column headers sized to the source string; truncation that cuts meaning with no way to see the full text; abbreviations invented to fit; text inside images; fixed line heights; storage or message length limits set from the source language; forced uppercase or forced word breaks; a flat 30% allowance applied to short labels.
**Move:** Let containers grow and wrap, budgeting most for the shortest strings: a source of up to 10 characters can reach 200–300% of its length, 11–20 → 180–200%, 21–30 → 160–180%, 31–50 → 140–160%, long text about 130%; scripts such as Thai can need about 1.5× the height, and East Asian text can take more width with fewer characters. Pseudo-localize (strings about 40% longer, with visible end markers) before translating to expose clipping and hard-coded text; keep text out of images; let translators own capitalization.
**From:** W3C text size; L10n dev guide; GLOB guide (make localizable)

## AI and automation

### Capability expectations · ui
**Ask:** From the product itself, can a person tell what this AI does well, what it cannot do and how often it errs, before they rely on it?
**Smell:** A blank prompt or "How can I help?" with no sign of the jobs it handles; "AI magic" claims; a small "AI can make mistakes" footer as the only expectation cue; first-person, human-like phrasing or a persona implying understanding it lacks; claims to do what it cannot (checking stock it cannot see); customers unable to tell whether staff or a bot is answering.
**Move:** Open with the specific jobs it does well as tappable examples or buttons, since many people struggle to phrase a request, and disclose persistently that it is an AI. State each important limit as a plain sentence paired with what to do, next to the input; raise a specific alert at the moment conditions are known to degrade (unsupported language, unreadable photo). Match human-like cues to real ability, and re-introduce the feature when its capabilities change.
**From:** HAI G1, G2 (pattern 2-D), G18; PAIR (Mental models); NN/g (site chatbots; explainable AI in chat; AI paradigm: articulation barrier)

### Calibrated trust · ui
**Ask:** For an output someone will act on, can they see why it was produced and how far to rely on it, at the moment of the decision?
**Smell:** Every answer in the same confident tone; "best option" or a ranking with no criteria; a precise percentage nobody can act on (85.8% vs 87%); sources listed at the end, detached from claims or not supporting them; a step-by-step "reasoning" panel presented as the true account of the decision; no explanation exactly where the stakes are high.
**Move:** Aim for calibrated trust, not maximum trust: give a short partial reason naming the key inputs and data used ("based on your last three orders"), with more on request. Place each source beside the claim it supports and link to the passage. Show uncertainty where it changes the decision: as two or three levels, each with a stated action, or as a few alternatives when they are close, in wording that stays short of certainty. In high-stakes outputs, always pair "what" with "why".
**From:** HAI G2, G11; PAIR (Explainability + trust); NN/g (AI hallucinations; explainable AI in chat; chatbots discourage error checking)

### Efficient dismissal and correction · ui
**Ask:** When the AI is wrong or unwanted, can the person ignore, fix or undo it with less effort than doing the job alone, and do they know what the correction changes next time?
**Smell:** A suggestion that blocks the path or must be closed before work continues; a proactive pop-in mid-task; output that can only be regenerated, not edited in place; an automatic change to the person's data or message (re-categorised, auto-tagged, auto-replied) with no notice or undo; thumbs up/down that changes nothing visible and promises nothing; the same mistake right after a correction; no way to switch the feature off or reset what it learned.
**Move:** Let the person carry on past AI output without acting on it; make output editable in place; announce each automatic change with its own undo ("Moved to Billing · Undo"), clear about which item it touches. Acknowledge feedback with what changes and when (this item now, similar items after the next update), and keep a one-item fix apart from global settings. When users correct often, offer an off switch, because correcting now costs more than doing; let them see and reset what it learned about them.
**From:** HAI G3, G8, G9 (pattern 9-C), G15, G16, G17; PAIR (Feedback + control)
