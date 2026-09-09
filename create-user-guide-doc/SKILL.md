---
name: "create-user-guide-doc"
description: "Generate a Google Doc user guide for the current project — covering what it is, who it's for, how to use it, and how it was built (architecture/tech stack) — and create it in Google Drive. Use when the user asks to write/create a user guide, doc, or documentation 'like the iMAG one' as a Google Doc for the current project, or asks for a guide employees/users can read."
---

# create-user-guide-doc

Produces a Google Doc aimed at **end users/employees** of whatever's in the
current project folder — not a dev-facing README. Mirrors the structure used
for the iMAG Page Builder user guide: what it is, how to use it, what to
expect, how it was built, and where to get help.

## 1. Gather project context

Read whatever exists in the current project directory to ground the doc in
fact, not assumption:

- `CLAUDE.md` — architecture, conventions, gotchas
- `as_built.txt` — current-state inventory (scripts, integrations, data
  stores, automations) if the `dcp` skill has been used here
- `README.md`
- Any entry-point scripts, config, or `package.json`/`requirements.txt` etc.
  needed to understand what the thing actually does and what stack it uses

Identify:
- **What the project does**, in plain, non-technical language a general
  employee would understand — not implementation detail.
- **Who would realistically use it** (end users vs. admins vs. developers).
- **The actual entry point** a user interacts with (a URL, a form, a CLI
  command, a Slack command, etc.) — never guess or fabricate one.
- **The real architecture/pipeline** and technologies involved, for the
  "how it was built" section.

If an essential fact is missing from the repo and can't be inferred safely
(e.g. a live form URL, a deployed app URL, who to contact for support), ask
the user for it directly (`AskUserQuestion`) rather than inventing a
placeholder that could mislead a reader — a wrong URL in a doc that goes out
to employees is worse than a short pause to ask.

## 2. Draft the doc

Write an HTML file to the scratchpad directory, then create it as a Google
Doc. Use this section structure, adapting section count/order to what's
actually true of the project — skip sections that don't apply, don't force
content that isn't there:

1. **What This Is** — one or two plain-language paragraphs.
2. **Who This Guide Is For**.
3. **How to Use It** — numbered, concrete steps a non-technical reader can
   follow start to finish, including the real link/command they need.
4. **What to Expect** (output format, turnaround time, what happens behind
   the scenes) — only what's true; don't promise notifications, speed, or
   behavior that isn't actually implemented.
5. **Known Limitations** — be honest about gaps (e.g. "does not send email",
   "still requires a manual publish step") rather than omitting them; this
   saves the support burden of everyone asking "why didn't X happen."
6. **How It Was Built (Architecture & Tech Stack)** — the pipeline end to
   end, each component with a one-line role, then a plain tech-stack bullet
   list. Written for a curious reader, not just an engineer.
7. **Troubleshooting** — an HTML `<table>` of symptom → likely cause/fix,
   drawn from the Known Limitations and How-to-Use steps.
8. **Support / Questions** — who/where to go for help; don't invent a
   contact if none is known — ask the user instead.

Keep tone plain and direct, matching the style of an internal how-to, not
marketing copy. Use `<h1>`/`<h2>`, `<p>`, `<ol>`/`<ul>`, `<b>`/`<i>`, `<a
href>`, `<code>`, and `<table>` — Google Docs' HTML import handles all of
these cleanly.

## 3. Create the Google Doc

Load the tool if not already available: `ToolSearch
select:mcp__claude_ai_Google_Drive__create_file`.

Call it with:
- `title`: a short, specific name, e.g. "`<Project Name> — User Guide`"
- `contentMimeType`: `text/html`
- `textContent`: the full HTML content (Drive auto-converts `text/html` to
  a native Google Doc — do not set `disableConversionToGoogleType`)

Always pass `parentId: 1fN4U-Ebu_m5tt8Mw-335o9_ZjH5lQqDK` — the designated
Google Drive folder for user-guide docs
(https://drive.google.com/drive/folders/1fN4U-Ebu_m5tt8Mw-335o9_ZjH5lQqDK) —
so every guide this skill creates is filed there by default. Only use a
different `parentId` if the user explicitly names another folder for a
particular doc.

## 4. Report back

Give the user the resulting `viewUrl` from the tool response as a clickable
link, plus a one-line summary of what sections it covers. Do not fabricate
a URL — only report the one the tool actually returned.
