---
name: update-docs
description: Scan the project's docs, figure out which ones are stale, and present a ranked, numbered list of suggested updates the user can pick from by entering a number. Read-only until the user picks — never edits a doc before the user selects an option. Use after implementing a plan (including ad hoc changes made along the way), after adding/changing a feature or command, or whenever the user wants a docs health check — "update the docs", "do the docs need changes", "check if the readme is stale", "docs health check", or invokes /update-docs.
---

# update-docs

Find out-of-date documentation and **let the user choose what to fix**, without
just rewriting everything unprompted.

## Hard rule — report first, edit only after a selection

This skill is a review step. Its whole value is that the user sees the
findings and decides what changes; an edit made before that defeats the
purpose, even if the edit is correct.

- Before doing anything else, print exactly:
  "I will look for docs that might need updates and report back without making any edits."
- Do not edit, create, or delete any file until the user has replied to the
  numbered list (Step 4) with a selection.
- Being invoked is **not** a selection — even if the request was literally
  "update the docs". Only a reply to the numbered list counts.
- This holds in auto mode and every other permission mode. Auto mode means
  tool calls don't need approval; it does not mean the user has picked
  which docs to change.
- If you realize you have already edited a file before getting a selection:
  stop, tell the user exactly which files you changed, and ask whether to
  keep or revert them. Do not continue the scan or try to quietly undo it.

## Step 1 — pick a mode

1. **Targeted** (default): triggered right after implementing a plan, or
   after adding/changing a feature/command. Scope the check to what actually
   changed — the plan doc used for the implementation, the diff/commits for
   this session, and any docs (README, CLAUDE.md, docs/**, command/skill
   references) that mention the touched area.
2. **Deep health check**: only when the user explicitly asks for a thorough
   scan/audit of all docs ("do a deep dive", "health check the docs",
   "thoroughly check all docs"). Read every doc in the repo and cross-check
   it against current code, not just recent changes.

If it's ambiguous which mode fits, default to targeted and mention that a
deeper scan is available on request — don't launch a full audit uninvited.

## Step 2 — discover the docs

Find candidate docs: `README*`, `CLAUDE.md`, `docs/**/*.md`, plan/design docs
referenced in the conversation, skill/command reference docs, changelogs. For
targeted mode, narrow to the ones plausibly touching the changed area (grep
for the feature/command name, module, or file paths involved).

## Step 3 — assess staleness

For each candidate doc, compare its claims against current reality:
- Plan docs: does it still match what was actually built? Ad hoc deviations
  made during implementation (extra edge cases handled, a different approach
  than planned, scope cut or added) belong here.
- README / usage docs: do documented commands, flags, file paths, and
  examples still match the code? Anything added/renamed/removed that isn't
  reflected?
- Architecture/reference docs: any described component, module, or flow that
  no longer exists or changed shape?

Use `git log`/`git diff` for the relevant range (recent commits for targeted
mode, broader history for deep mode) and read the actual current code —
don't infer staleness from the doc's age alone.

## Step 4 — present the ranked list

The numbered list is one item **per doc** — the number is what the user
types to select it. Never collapse a doc's changes into one long sentence:
if a doc has more than one stale point, break them into a bulleted sub-list
under that doc's number, one bullet per atomic change, each with its own
short fix. A doc with a single stale point still gets one bullet — the
number line itself only ever holds the doc name and confidence.

**Always append one final numbered item** — "Run a deeper health check
across all docs" — after the findings, even if the list is empty or this was
already a deep-mode scan. It must be a real numbered option the user can
select by typing its number, not something they have to ask for in words.
Example: if targeted mode found 4 items, item 5 is the deep-check option; if
it found 0, item 1 is the deep-check option.

```
1. README.md (high confidence)
   - `/foo` command added but undocumented → add a `/foo` row to the command table with its one-line description
   - old `--legacy` flag still documented but was removed → delete that section

2. docs/architecture.md (high confidence)
   - still describes the old sync queue, removed in this change → replace the queue section with the new direct-write path

3. plan-oauth-refresh.md (medium confidence)
   - implementation added a retry-with-backoff step not in the plan → add a step documenting the retry behavior and why it was needed

4. docs/setup.md (low confidence)
   - mentions a config flag that may be unrelated to this change → verify the flag still exists before touching this

5. Run a deeper health check across all docs, not just what changed here
```

Directly under the list, print exactly:
"No files have been changed. Reply with a number (or a comma-separated list, or "all") to apply those updates, or give different instructions."

Then **end your turn**. Do not apply anything yet. Picking the last option
re-runs from Step 1 in deep mode instead of applying an edit.

## Step 5 — apply on selection

**Gate:** before touching any file, confirm the user's most recent message
is a reply to the numbered list that selects one or more items (a number, a
list, "all", or instructions that clearly pick specific items). If it isn't,
do not edit — go back to waiting.

If the user picks the deep-check option, go back to Step 1 in deep mode and
present a fresh ranked list from that scan — don't apply anything from the
targeted list first unless they also selected other numbers. The hard rule
applies again to the new list.

When the user picks finding(s) by number:
- Make the edit(s) to that doc, matching the suggested fix (adjust if the
  user's reply narrows or changes it).
- Show a short diff/summary of what changed per doc.
- Don't touch unselected items.
- If new information surfaces while editing (e.g. the fix is bigger than the
  one-liner suggested), say so before expanding scope.

## Rules

- Never rewrite a doc wholesale when a targeted edit covers the actual
  staleness — preserve the author's structure and voice.
- Don't invent doc updates for things that didn't actually change — every
  item in the list must trace to a real diff between the doc and the current
  code/plan.
- Confidence ranking reflects certainty that the doc is stale AND that the
  suggested fix is right — not just "this doc looks old."
- If discovery turns up zero stale docs, say so plainly instead of padding
  the list with low-value nitpicks.
