# Setup instructions for an AI agent

You are being asked to bootstrap this machine onto the `ai-bits` skills repo
(`https://github.com/ChrisHMullins/ai-bits`). Follow these steps exactly —
don't skip or reorder them.

1. Determine the local agent skills directory. For Claude Code this is
   `~/.claude/skills/` (create it if missing). If you are a different agent,
   use its equivalent personal-skills directory.
2. Check whether the repo is already cloned on this machine before cloning
   anything: read `~/.claude/ai-bits-repo-path` if it exists, then probe
   common locations (`~/Repos/ai-bits`, `~/repos/ai-bits`, `~/ai-bits`,
   `~/Documents/Repos/<username>/ai-bits`; on Windows the `%USERPROFILE%`
   equivalents of the same). If a valid clone is found, run
   `git pull --ff-only` in it and skip to step 4. If the pull fails
   (diverged history, dirty tree), stop and show the user — don't force
   anything.
3. If no clone exists, propose a local path and confirm it with the user
   before cloning — match their evident repo-folder convention if one exists
   (e.g. `~/Repos/ai-bits`), falling back to `~/ai-bits` only if there's no
   convention to follow. Once they've confirmed:
   ```
   git clone https://github.com/ChrisHMullins/ai-bits.git <path>
   ```
4. Compare `<path>/skills/` against the skills directory from step 1 before
   copying:
   - A repo skill with no same-named local folder will be newly installed.
   - Where a same-named local skill exists, diff the two. If the local copy
     differs, show the user the diff before overwriting — the local version
     may be *ahead* of the repo (unpushed edits), in which case offer to
     push it up via `sync-skills` after bootstrap instead of clobbering it.

   Tell the user what will be installed or updated and get their go-ahead
   before writing into the skills directory.
5. Copy every top-level folder under `<path>/skills/` into the skills
   directory from step 1, per the confirmation above.
6. Record the resolved repo path in `~/.claude/ai-bits-repo-path` (plain
   text, one line; on Windows the same file under `%USERPROFILE%\.claude\`)
   — the `sync-skills` skill reads this to skip re-discovery on future runs.
7. From this point on, use the `sync-skills` skill (now installed in the
   skills directory) for all future syncing — pulling updates from the repo,
   or pushing local skill edits back up. Do not hand-roll `cp`/`git`
   commands for routine syncs after this initial bootstrap.
8. Confirm to the user which skills were installed or updated and that
   `sync-skills` is now available for future pulls/pushes.

## Rules

- Never force-push, rewrite history, or delete anything in the repo during
  this process.
- If the skills directory or repo path already has content, don't silently
  overwrite unrelated files — only touch the skill folders described above.
