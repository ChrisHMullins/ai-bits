---
name: needs-user
description: Produce the closed list of decisions and actions that need the owner, and nothing else — no status report, no narrative of work done. Use when the owner says "what do you need from me", "what's on my plate", "hand this off", "what do I need to decide", "what's waiting on me", or invokes /needs-user. Also offer it when a turn ends with decisions that are genuinely the owner's.
---

# needs-user

The owner's attention is the scarce resource. This skill produces **the list of
things only they can do** — closed, ranked, each with a recommendation — and
deliberately omits everything else.

A report is organised around what the agent did. A handoff is organised around
what the owner must do. **This is the second thing.** If an item does not
require them, it does not appear, however interesting it is.

## Scope

The grammar has no reserved words: **bare means the session, a number means
that many turns, anything else is a subject filter.**

- **`/needs-user` (bare) — session-scoped. The default.** Everything still open
  for the owner across the whole session, not just the last turn. Items go stale
  and get buried when each turn only reports its own news; session scope is what
  makes the list closeable.
- **`/needs-user <N>` — the last N turns.** `1` is the most recent turn, `3` the
  last three.
- **`/needs-user <subject>` — narrowed.** Free text limits the list to that
  blocking item or topic: *"sometimes we gotta be razor focused on a blocking
  item, not the entire list."*

Whenever the list is narrowed by either means, say so — a short list must not be
mistaken for an empty queue.

## Selection — the hard part

Most of the value is here, not in the formatting. Before writing an item, put it
through this filter. Only the last category reaches the owner.

- **Missing rule** — a question with the same answer every time. Don't ask it.
  Write the rule, and if it needs blessing, list it *once* as a policy item, not
  repeatedly as instances.
- **Missing evidence** — a question a machine could answer by going and
  looking, running the test, reading the file. Don't ask it. Go and look.
- **Unearned permission** — asking before an action that is reversible and
  already in scope. Don't ask it. Do it and report it.
- **Genuinely theirs** — taste, risk appetite, product direction, anything
  needing their accounts, credentials, hardware, or money, and any decision not
  derivable from the code. **These, and only these.**

One more rule decides *how* an item is written:

- **Prefer policy altitude.** If one answer would settle every future instance,
  ask the policy and use the case at hand as the worked example — a policy
  answer compounds, an instance answer is spent on delivery. State how many
  cases stand behind the recommendation so they know whether they are setting a
  rule from one data point or ten. Never manufacture generality: a question
  genuinely about this file this week stays specific.

## Completeness

The list must be **closed, not illustrative**. Invoked after a long stretch of
work, the list is re-derived from memory, and re-derivation silently drops
things and invents others. So:

- Every item must trace to work actually done or genuinely blocked. No
  speculative asks.
- End with an explicit closing line — *"Nothing else is waiting on you"* — so a
  short list reads as exhaustive rather than as a sample.
- If nothing needs them, say so in the first line and stop. Zero is a valid and
  common answer.

## Item shape

**The headline is the recommendation.** Phrase it as the imperative action being
recommended — *"Auto-delete build branches once merged"*, not *"Decide whether
merged build branches auto-delete"*. A neutral "decide whether…" hands the
analysis back to the owner, which is the agent's job; committing to a position
and letting them veto is far cheaper for them than choosing from scratch. It
also removes a line: once the headline says what to do, a separate `Recommend:`
is redundant.

Each item carries, in this order:

1. **The recommended action — imperative, bold.** What they should do.
2. **Blocking or not, and what it blocks** — same line, after an em dash.
3. **Why** — one indented bullet. When the choice is genuinely two-sided, the
   alternative goes here.
4. **If you don't answer** — one indented bullet naming the reversible default,
   where one exists. This turns a blocking ask into a non-blocking one, and is
   usually the most useful line in the item.

One bold per item; the indentation does the visual separation, so nothing
shouts. Rank blocking items first, then by what they unblock.

**A deliberate limit of this shape:** it cannot present a neutral fork — the
headline always picks a side. That is correct for a handoff. A neutral fork is
a *question*, and questions belong in a doc where they can carry options,
evidence and a recommendation, not in a list of things to do.

## Response format

```
**N decisions waiting on you.**   ← or: **Nothing is waiting on you.**

1. **<Recommended action, imperative>** — blocking: <what it blocks>
   - Why: <one line>
   - If you don't answer: <the reversible default>

2. **<Recommended action, imperative>** — not blocking
   - Why: <one line>

Nothing else is waiting on you.
```

No preamble. No summary of work done. No closing pleasantry.

## What this never includes

- What was built, fixed, found, or verified. That is a report — a different
  thing, and it belongs in the ordinary response or a doc.
- Anything the agent can do itself, including things it would rather not.
- Progress narrative, test counts, commit hashes, file paths as reassurance.
- Items kept on the list because they are interesting rather than because they
  are blocked on the owner.

## Obligations belong on the list

An item not on the list should not be waiting on the owner. That is what
*closed* is for, and it is most of the artifact's value: they can read the
list, act or not, and trust that nothing was hiding in the prose.

So **avoid creating obligations in prose.** Two things have to stay distinct:

- **State they may want to know** — an unpushed commit, a service still
  running, a branch left in place. Say it plainly, attach no expectation, move
  on.
- **An action awaiting them** — goes on the list. Never in a closing sentence.

The test for which one you are holding: **will some mechanism re-surface it?**
An unpushed commit reappears the next time the sync runs; a stopped service
reappears the next time something needs it. If something will bring it back,
it is state, and reporting it costs the owner nothing. If nothing will, it is
an action wearing state's clothing, and it belongs on the list.

**The tell is obligation-flavoured phrasing outside the list**: *"waiting on
your next push"*, *"say the word and I'll…"*, *"when you get a chance"*,
*"let me know if you want…"*. Each one quietly opens a ticket in someone's
head. Rewrite as state, or move it to the list:

- Wrong: *"Committed, not pushed — waiting on your next public push."*
- Right: *"Committed locally. The next sync will offer it."*
- Wrong: *"Say the word and it's a one-line fix."*
- Right: just do it, and report it as done — or if it genuinely needs them, it
  is a list item, not an aside.

This binds hardest immediately after the list. Declaring "nothing else is
waiting on you" and then adding a chore in the next breath is the exact leak
this skill exists to close.

### Two limits, because this is a norm and not a law

**Urgency overrides it.** If something is dangerous, destructive, or
time-critical, say so immediately and plainly, list or no list. A discipline
that muffles a real alarm has failed far worse than one that leaks a chore.
Obligation-flavoured language is the correct register there.

**Ordinary conversation is not a handoff.** Working a problem through,
weighing a design, asking which of two approaches the owner prefers — those
are normal collaborative moves, not leaks. This governs the close of a work
handoff, not every sentence in a session.

**Never swallow the signal — this is a relief valve, not a loophole.** If
something needs saying and no named limit covers it, *say it*, and note that
you are stepping outside the pattern to do so. The failure this guards against
is **suppression**, not non-compliance: an absolute with nowhere to vent makes
the inconvenient observation quietly disappear, and a lost signal is a worse
outcome than a stretched norm. The valve exists so the thing gets expressed,
not so the rule gets bypassed — if the pattern cannot carry what needs
carrying, that is information about the pattern, and it should reach the owner
rather than being absorbed in silence.

### On the closing line

*"Nothing else is waiting on you"* is a report of what was found, not proof
that nothing was missed. The Completeness section above admits the list is
re-derived and that re-derivation drops things; both are true at once. Say the
line plainly anyway — a list that hedges is not worth reading — but if the
owner thinks something is missing, **assume they are right and go look**,
rather than defending the list. Confidence in the format is not evidence about
its contents.

## Examples

**Good**

```
**2 decisions waiting on you.**

1. **Auto-delete build branches once merged** — blocking: every repo after
   its first successful build
   - Why: `git branch -d` refuses unless the work is already in the target
     branch, so it cannot lose code. The alternative, leaving them, costs
     one repo per success.
   - If you don't answer: the reachable pool shrinks as builds succeed.

2. **Rotate the leaked API key** — not blocking, highest consequence
   on this list
   - Why: it is already in git history, so removing the file does not
     help; only revocation does. Needs your provider account, so nothing
     automated can close it.

Nothing else is waiting on you.
```

**Bad** — this is a report wearing a handoff's clothes:

```
I fixed the head-of-line blocking bug and added 21 tests, all passing.
The scheduler now picks 3 jobs a run instead of 1. I also found the test
suite was deleting a production lock file...  One thing is yours: should
merged branches auto-delete?
```

The ask is real but buried at position four, behind work the owner did not need
to evaluate. Same content, wrong artifact.
