# Open a profile

Steps for `profile new`. The user opens the profile; you discover, draft and ask. The
profile holds everything the core must not know about a project, and the council
reads no project fact from anywhere else.

## 1. Name and paths

- Ask for the profile's name; propose the repo or folder name. It must be new under
  `~/.council/`.
- Propose the paths it covers: the working directory, plus the repo root when they
  differ. The user adds others (sibling repos, a workspace folder around the repo).
- When an existing profile already covers the working directory, say which one and ask
  whether to extend it instead.

Done when: the user has confirmed a free name and the list of paths.

## 2. Discover, then ask

Discover first, read-only, so the user confirms instead of typing:
- the repo's agent-rules file, product context (for example `PRODUCT.md`,
  `DESIGN.md`, a README) and glossary;
- the directory layout, for a first path → target map;
- locales (translation catalogs), themes and breakpoints in the code;
- channels: web screens, phone layouts, chat bots and their messages, email templates;
- data sources reachable in this session (connected tools, database and log access,
  analytics);
- helper skills: compare the session's available skills with `routes.md` → Suggestions.

Then ask the user, in at most three rounds of a few questions each, for what only they
know:
- the users and roles, each with their situation, goal and how often they come;
- which sources the council may read, and each source's house rules: what may be quoted
  in chat, where real data must never go, what to do when access is refused;
- how to see the product running: the URL and the names of the test-account keys
  (never their values);
- the inputs people really type that must be accepted (formats of phone numbers, dates,
  amounts, names);
- whether to restrict helper skills to an allowlist, or leave the section out so Nafis
  uses any suggested helper the session has.

Done when: every section of the profile template has a value or the user's explicit
`none`.

## 3. Draft, confirm, write

1. Fill `templates/profile.md` and show the user the whole draft.
2. On their approval, write `~/.council/<name>/profile.md` and create
   `~/.council/<name>/features/`. The profile holds pointers and key names only: no
   secrets, no customer data.
3. Resolve the profile for the working directory (`SKILL.md` → Start, step 3); it must
   resolve to the new one.

Done when: the file exists, the user approved its content, and the working directory
resolves to it.
