---
name: sync-skills
description: Use when the user wants to sync Agent Skills between this machine and the ai-bits GitHub repo (https://github.com/ChrisHMullins/ai-bits) — "sync skills", "pull down my skills", "push my skills", "update skills from the repo", or right after editing/creating a local skill that should follow the user to other machines. Also invoked as /sync-skills. Works on macOS, Linux, and Windows.
---

# sync-skills

Keep the local agent skills directory (e.g. `~/.claude/skills/`) in sync with
the `ai-bits` GitHub repo, so skills follow the user across machines.

Repo: `https://github.com/ChrisHMullins/ai-bits` — skills live under `skills/<name>/SKILL.md`
in that repo, same layout as the local skills directory.

## Step 1 — locate the local clone

Resolve the repo path in this order, and remember whichever one works by
writing it to `~/.claude/ai-bits-repo-path` (plain text, one line) so future
runs skip straight to it:

1. Read `~/.claude/ai-bits-repo-path` if it exists and the directory is still
   a valid git clone of the repo.
2. Otherwise check common locations for this OS:
   - macOS/Linux: `~/Repos/ai-bits`, `~/repos/ai-bits`, `~/ai-bits`, `~/Documents/Repos/<username>/ai-bits`
   - Windows: `%USERPROFILE%\Repos\ai-bits`, `%USERPROFILE%\Documents\Repos\<username>\ai-bits`, `%USERPROFILE%\ai-bits`
3. If still not found, ask the user where to clone it (default: `~/ai-bits`
   or the OS equivalent), then:
   ```
   git clone https://github.com/ChrisHMullins/ai-bits.git <path>
   ```
4. Write the resolved path to `~/.claude/ai-bits-repo-path`.

## Step 2 — determine direction

Default to **pull** unless the user said "push" / "upload" / "sync up" or
similar. If ambiguous, ask.

## Pull (repo → local skills dir)

1. `git -C <repo> pull --ff-only` (if this fails — diverged history, dirty
   tree — stop and show the user, don't force anything).
2. For every top-level folder under `<repo>/skills/` that contains a
   `SKILL.md`, copy it into the local skills directory, overwriting the
   existing folder if present. `cp -r src dest` nests (`dest/src`) when
   `dest` already exists as a directory — remove the destination first, or
   copy contents rather than the folder itself:
   - macOS/Linux: `rm -rf <skills-dir>/<name> && cp -r <repo>/skills/<name> <skills-dir>/<name>`
   - Windows (PowerShell): `Remove-Item -Recurse -Force <skills-dir>\<name> -ErrorAction SilentlyContinue; Copy-Item -Recurse -Force <repo>\skills\<name> <skills-dir>\<name>`
   - After copying, verify: the target `SKILL.md` must sit directly at
     `<skills-dir>/<name>/SKILL.md`, not one level deeper
     (`<skills-dir>/<name>/<name>/SKILL.md`).
3. Report which skills were added or updated (diff folder names before/after,
   don't just say "done").
4. Never delete a local skill that isn't in the repo — that may be a
   work-in-progress or intentionally local skill. Just note the mismatch.

## Push (local skills dir → repo)

This is also the flow to use right after making a change to a skill: edit the
skill locally, then push it — don't hand-roll `cp`/`git` steps ad hoc.

1. For every top-level folder under the local skills directory that contains
   a `SKILL.md`, copy it into `<repo>/skills/<name>`, overwriting. Same
   nesting hazard as the pull direction applies in reverse — remove
   `<repo>/skills/<name>` first, or copy contents rather than the folder
   itself, and verify `<repo>/skills/<name>/SKILL.md` isn't nested one level
   deeper afterward.
2. `git -C <repo> status --short` to see what actually changed. If nothing
   changed, say so and stop.
3. If a skill folder is new (wasn't in the repo before), add a row to the
   `## Skills` table in `<repo>/README.md`: `| [\`name\`](skills/name/SKILL.md) | <description from frontmatter, trimmed to one clause> |`.
4. `git -C <repo> diff` (not just `--short`) and read it. The repo is
   **public** — flag anything that looks like a secret, internal path,
   private project name, or anything else the user might not want visible
   on GitHub, before going further.
5. Show the user the changed/new skill names and the diff summary, then ask
   for confirmation before committing and pushing — never push silently.
6. On confirmation: `git -C <repo> add -A`, commit with a message describing
   *what changed in the skill* (e.g. `md-to-plan: number questions uniquely
   across whole document`), not a generic `Sync skills: <names>` — then
   `git -C <repo> push`.
7. Commit messages never include attribution lines (e.g. `Co-Authored-By:`)
   — this matches the user's global git preference and applies here too.

## Rules

- Never force-push, rebase, or rewrite history in the repo.
- Never delete skills on either side — sync only adds/updates.
- Plain directory copies only, never symlinks (Windows symlinks need
  elevated permissions/dev mode; copies work identically on every OS).
- If `<repo>` has uncommitted changes unrelated to skills when pulling, stop
  and show the user rather than stashing or discarding anything.
