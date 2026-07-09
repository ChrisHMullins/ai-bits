# Setup instructions for an AI agent

You are being asked to bootstrap this machine onto the `ai-bits` skills repo
(`https://github.com/ChrisHMullins/ai-bits`). The sync logic lives in one
place — `skills/sync-skills/SKILL.md` in this repo — and this bootstrap
exists only to get that skill installed. Follow it rather than improvising
your own copy/git steps.

1. Determine the local agent skills directory. For Claude Code this is
   `~/.claude/skills/` (create it if missing). If you are a different agent,
   use its equivalent personal-skills directory.
2. Locate or clone the repo by following "Step 1 — locate the local clone"
   in `skills/sync-skills/SKILL.md` — read that file straight from GitHub if
   no local clone exists yet. It covers the probe locations, proposing a
   clone path that matches the user's repo-folder convention (confirm with
   them before cloning), and recording the resolved path in
   `~/.claude/ai-bits-repo-path`.
3. Copy `<repo>/skills/sync-skills` into the skills directory from step 1.
   Verify the result sits at `<skills-dir>/sync-skills/SKILL.md`, not nested
   one level deeper (`cp -r` nests when the destination folder already
   exists).
4. Install the remaining skills by following the **Pull** flow in the
   now-installed `sync-skills` skill — it handles diffing against any
   existing local skills, per-skill confirmation, and reporting.
5. Confirm to the user which skills were installed or updated, and that
   `sync-skills` is the tool for all future syncing — pulling updates or
   pushing local skill edits back up, never hand-rolled `cp`/`git` commands.

## Rules

- Never force-push, rewrite history, or delete anything in the repo during
  this process.
- If the skills directory or repo path already has content, don't silently
  overwrite unrelated files — only touch the skill folders described above.
