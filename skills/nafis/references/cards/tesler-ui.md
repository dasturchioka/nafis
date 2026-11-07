# Tesler cards: ui

Loaded for `ui` targets, together with `tesler-both.md`.

## The person's load

### Hick's Law · ui
**Ask:** At this decision point, how many options must the user weigh, and is the likely one already chosen or highlighted?
**Smell:** A menu, form or bot message offers many equal-weight choices at a moment that needs speed. There is no recommended or default option, and every feature is exposed on first use.
**Move:** Trim or defer options at time-critical steps and mark the recommended path. Split complex choices into steps and reveal advanced options gradually, while keeping labels concrete instead of abstract.
**From:** Laws of UX

### Cognitive Load · ui
**Ask:** Which elements here help the user reach the goal, and which only spend their attention?
**Smell:** Decoration, duplicate labels, jargon, promos or unrelated options sit beside a task. Modals, forced re-logins or upsells interrupt mid-task. The screen or message is denser than the goal needs.
**Move:** Cut the extraneous load (anything that does not serve the current goal), hold interruptions until the task ends, and keep the intrinsic task material clear and in view.
**From:** Laws of UX (Cognitive Load; Flow)

### Goal-Gradient Effect · ui
**Ask:** In multi-step work, can the user see how far along they are and how little is left?
**Smell:** Setup, onboarding or long forms show no step count or progress. Progress starts at zero even though some work is already done, or the hardest stretch comes last.
**Move:** Show clear progress and count completed groundwork as a head start. Keep the final steps short so that momentum builds toward the finish.
**From:** Laws of UX

## Conversations

### Turn-taking · ui
**Ask:** Does each system turn end with exactly one question, and does the flow take everything a reply contains, so the person never answers the same thing twice?
**Smell:** Two questions in one message, or text after the question so nobody knows whose turn it is; a flow that reads one field per turn and asks "What size?" after the person wrote "size 42 running shoes"; a clarifying question on a request that was already clear; "thanks", "that's all" or "done" answered with a not-understood message.
**Move:** Close each turn with one question and stop; ask narrow questions for first asks, unfamiliar topics and after an error, and wide ones only where every likely answer is handled; fill every field a reply supplies, in any order; clarify only when the request would otherwise get a wrong answer; read closing phrases as the end of the task.
**From:** CDG (learn about conversation; questions); NN/g less chat, more answer

### Quick replies · ui
**Ask:** Where the likely answers are known, can the person answer with one tap, and can they still type anything the buttons do not cover?
**Smell:** A date, size or plan the system could offer typed by hand; buttons as the only input, so anything off-script is impossible; buttons that cover one corner of the possible answers or repeat the items of the list above them; a message whose text also spells out every button; a tapped reply the system would not understand if the same words were typed; no "none of these" after a short list.
**Move:** Offer the popular answers as short buttons (at most about eight, about 25 characters each, one per category for a broad question) beside a free-text field that stays open; treat a tap exactly like the same words typed, so both paths reach the same intent; end a list with a way out; keep detail and examples in the buttons rather than in the message.
**From:** NN/g chatbots; CDG (chips; suggestions)
