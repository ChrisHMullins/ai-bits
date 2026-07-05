---
name: md-to-plan
description: Turn the current markdown file (notes, ideas, a feature sketch) into a structured implementation plan, edited in place, with clarifying questions and recommended defaults embedded in each section. Use when the user says "turn this into a plan", "planify this doc", "make an implementation plan from this file", or invokes /md-to-plan.
---

# md-to-plan

Rewrite a markdown file **in place** so it becomes an implementation plan the
user can review and annotate asynchronously.

## Target file

1. If an argument was given (`/md-to-plan docs/foo.md`), that's the target.
2. Otherwise use the markdown file currently open in the IDE.
3. If neither exists, ask the user which file to use.

Read the whole file before editing. Preserve the author's intent, ideas, and
any decisions already made — you are restructuring and elaborating, not
replacing their thinking.

## Output structure

Rewrite the document into these sections (keep the original title; adapt
section names if the doc already has a natural structure):

1. **Summary** — what we're building and why, 2–4 sentences distilled from
   the original notes.
2. **Scope** — in scope / out of scope bullets. Anything ambiguous in the
   original goes to a clarifying question, not a silent assumption.
3. **Implementation steps** — ordered, concrete steps referencing real files
   and modules in the repo (go look at the code; don't invent paths). Each
   step small enough to verify independently.
4. **Testing** — REQUIRED, never omit. Concrete test cases to write: which
   test file(s), what behavior each case pins down, and how to run them.
   New behavior gets new tests; changed behavior gets updated tests.
5. **Risks / open questions** — anything that could invalidate the plan.

## Clarifying questions — required in every section

Each section (including Testing) ends with a questions block. Every question
has 2–4 concrete options as checkboxes; exactly one option per question is
marked **(recommended)** and the recommendation should be genuinely defensible,
not just the first idea.

Questions are numbered **once, globally, across the entire document** —
do not restart at 1 in each section's block. Track a running counter as you
write top to bottom (Summary's questions first, then Scope's, etc.), so every
question in the doc has a unique number the user can reference in
conversation (e.g. "go with the recommended option for Q5"). Format:

```markdown
> **❓ Questions**
>
> **Q5.** How should takes be stored when a page is rerolled?
>    - [ ] Append to the existing takes array **(recommended)** — matches how storage.py already versions pages
>    - [ ] Overwrite the latest take — simpler, but loses history
>    - [ ] New file per take — avoids large JSON blobs, but complicates loading
```

Rules for questions:
- Only ask things the doc/code genuinely leaves open — no filler questions.
  If a section has no real open question, ask the single most load-bearing
  assumption you made ("I assumed X — correct?") with alternatives.
- Options must be concrete enough to act on, each with a short trade-off note.
- The checkboxes are for the user to tick later; do NOT block on answers.
  Write the plan assuming the recommended options, and say so in the Summary.
- Numbers must stay unique and stable within a document: if you later revise
  the plan and add questions, append new ones with the next free number
  rather than renumbering existing questions out from under the user.

## Rules

- Edit the file in place. No new files, no sidecar docs.
- Ground every implementation step in the actual codebase — read the relevant
  code first so file paths, function names, and test commands are real.
- Do not start implementing anything. The deliverable is the plan document.
- After writing, tell the user: the plan assumes the recommended options, and
  they should tick/adjust the checkboxes, then hand the doc back for
  implementation.
