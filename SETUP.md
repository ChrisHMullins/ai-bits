# Setup instructions for an AI agent

You are being asked to bootstrap this machine onto the `ai-bits` skills repo
(`https://github.com/ChrisHMullins/ai-bits`). Follow these steps exactly —
don't skip or reorder them.

1. Determine the local agent skills directory. For Claude Code this is
   `~/.claude/skills/` (create it if missing). If you are a different agent,
   use its equivalent personal-skills directory.
2. Clone the repo to a sensible local path (e.g. `~/ai-bits`, or match this
   user's existing repo-folder convention if one is evident, such as
   `~/Documents/Repos/<username>/ai-bits`):
   ```
   git clone https://github.com/ChrisHMullins/ai-bits.git <path>
   ```
3. Copy every top-level folder under `<path>/skills/` into the skills
   directory from step 1, overwriting if a same-named folder already exists
   there.
4. Record the resolved repo path in `~/.claude/ai-bits-repo-path` (plain
   text, one line) — the `sync-skills` skill reads this to skip
   re-discovery on future runs.
5. From this point on, use the `sync-skills` skill (now installed in the
   skills directory from step 3) for all future syncing — pulling updates
   from the repo, or pushing local skill edits back up. Do not hand-roll
   `cp`/`git` commands for routine syncs after this initial bootstrap.
6. Tell the user which skills were installed and that `sync-skills` is now
   available for future pulls/pushes.

## Rules

- Never force-push, rewrite history, or delete anything in the repo during
  this process.
- If the skills directory or repo path already has content, don't silently
  overwrite unrelated files — only touch the skill folders described above.
