---
name: prep-pr
description: Get the current branch ready for a pull request — sanity-check the diff, optionally run quality gates, draft a title/summary/test plan from the real changes, and open the PR after confirming with the user. Use when the user says "prep this PR", "get this ready for review", "open a PR for this", "let's ship this branch", or invokes /prep-pr.
---

# prep-pr

Take a feature branch from "code is written" to "PR is open," without
skipping the checks that make a PR easy to review.

## Step 1 — orient

Run in parallel:
- `git status` (uncommitted/untracked files)
- `git branch --show-current` and confirm it isn't `main`/`master`
- Whether the branch tracks a remote and is ahead/behind it
- `git log --oneline <base>..HEAD` and `git diff <base>...HEAD` to see every
  commit and change that will end up in the PR — not just the latest commit

If on `main`/`master` with local changes, stop and ask whether to create a
feature branch first — do not open a PR from the trunk branch.

If there are uncommitted changes, ask whether to commit them (and how) before
continuing; don't silently fold uncommitted work into the PR draft.

## Step 2 — quality gate (offer, don't force)

Ask the user if they want a quality pass before opening the PR:
- `/code-review` for correctness/simplification findings on the diff
- `/update-docs` if the change plausibly touches documented behavior
- Running the project's test suite / build, if one is discoverable

Do not run these unprompted — surface them as an option, since the user may
have already done this or want to skip it for a draft PR. If they decline or
don't respond to the offer, move on.

## Step 3 — draft the PR

Using the full commit range from Step 1 (not just the last commit):

1. **Title** — under 70 characters, describes the change, not the ticket
   number alone.
2. **Summary** — 1-3 bullets on what changed and why, synthesized from the
   actual commits/diff, not copy-pasted commit messages.
3. **Test plan** — a markdown checklist of concrete verification steps: what
   was tested manually, what automated tests cover it, what's left to verify.
   If nothing was tested yet, say so plainly rather than inventing a checklist.

Show this draft to the user before doing anything else. Do not push or open
the PR yet.

## Step 4 — confirm, push, open

Once the user approves the draft (or edits it):

1. Push the branch if it isn't already up to date with the remote
   (`git push -u origin <branch>` for a new branch).
2. Open the PR with `gh pr create --title "..." --body "$(cat <<'EOF' ... EOF)"`,
   using the confirmed title/body.
3. Return the PR URL.

Never force-push, never push to `main`/`master`, and never open the PR
without the user having seen and approved the draft body first.

## Rules

- Ground the summary and test plan in the real diff/commits — don't
  editorialize about the change beyond what's actually there.
- If the branch has no commits ahead of its base, say so and stop; there's
  nothing to open a PR for.
- If a PR already exists for this branch (`gh pr view` succeeds), tell the
  user and ask whether they want to update it instead of opening a new one.
