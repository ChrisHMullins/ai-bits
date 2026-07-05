---
name: clickable-prototypes
description: Build a throwaway, clickable prototype of a plan or feature idea purely for a design/UX check — not production code. Use when the user says "make a clickable prototype", "mock this up so I can click through it", "let's prototype this flow", or invokes /clickable-prototypes. Works across platforms (web, iOS/mobile, desktop) — defaults to the currently open plan/doc, or infers the target from recent conversation context if none is open.
---

# clickable-prototypes

Produce something the user can click through to sanity-check design and UX —
before any real implementation happens. The deliverable is a disposable
prototype, not a step toward shipping code. This applies to any platform:
web, iOS/mobile, desktop — not just browser apps.

## Step 1 — figure out what to prototype

1. If a plan document is currently open in the IDE (or was the subject of the
   current conversation), that's the target — read it in full.
2. Otherwise, infer the most recently discussed feature/flow/screen from
   conversation context.
3. If neither gives a clear target, ask the user what to prototype.

Identify the concrete screens/flows/states that matter for a UX review —
not every edge case, just what a reviewer needs to click through to judge
the design (happy path + the 1-2 states that are actually in question, e.g.
empty state, error state, a branching choice the plan is unsure about).

Also note the target platform — check the repo for signals (`.xcodeproj`/
`.xcworkspace`/`Package.swift` for iOS/Swift, `package.json` for web, etc.)
or ask if it's unclear. This determines what "the real app stack" means in
Step 2.

## Step 2 — offer the build approach as a choice

Don't silently default to one approach — ask the user which they want,
recommending one based on what the review is actually for:

- **Hosted mockup (e.g. the Artifact tool for web-viewable prototypes)** —
  fast, self-contained, shareable by link, and completely separate from the
  real codebase. Works for any platform as a UX/layout approximation (an
  HTML mock can stand in for an iOS screen's layout and flow), but it won't
  feel truly native and can't be dropped into the real app later. Load the
  `artifact-design` skill before writing an HTML mockup so it looks like a
  real product, not a wireframe. Recommend this by default — it's faster and
  sufficient for most layout/flow/copy reviews.
- **Real screens in the actual app stack**, isolated and throwaway (e.g.
  SwiftUI views run via Xcode Previews or the simulator for iOS, real
  components in the actual frontend framework for web) — true native look,
  feel, and interaction, and it catches platform-specific quirks a hosted
  mockup can't fake. Slower to build and needs a working dev environment to
  run. Recommend this when the review is specifically about native feel,
  platform-specific gestures/animations/components, or fitting into an
  existing design system's real components.

Present both options and let the user pick before building anything. If they
don't have a preference, go with the recommended default for the situation
and say why.

If building in the real app stack, keep it disposable: a throwaway
view/screen/route/branch, not integrated into real app state, data,
navigation, or shipped targets.

Never wire the prototype into real backend calls, real persistence, or
production code paths, regardless of which approach is chosen. All data is
mocked/hardcoded; all state lives in memory for the session.

## Step 3 — build it

- Cover the flows identified in Step 1 with realistic mock data — enough
  variety that the screens don't look like a single lucky-path screenshot.
- Make it genuinely clickable: buttons navigate, forms have basic client-side
  validation/feedback, toggles toggle. The point is to evaluate interaction
  and layout, not just look at static screens.
- Skip real business logic, auth, error handling for cases outside the
  reviewed flows, accessibility polish beyond what the design calls for, and
  responsive edge cases — unless the review is specifically about one of
  those.
- Keep scope tight to what's needed for the design/UX call. Do not expand
  into a fuller build without the user asking.

## Step 4 — hand it off

- Hosted mockup: deliver via the Artifact tool so the user can open it and
  click through immediately.
- Real app stack: get it running where the user can actually click through
  it (simulator, Preview canvas, dev server) and tell them exactly how to
  open/launch it — don't just say "it's done," hand them the concrete step
  to see it.
- Tell the user, briefly: what's mocked vs real, which flows are covered,
  and what was deliberately left out of scope.
- Do not treat this as a first draft of the real implementation — if the
  user likes it and wants to build the real thing, that's a new task with
  its own plan, not a graduation of this file.

## Rules

- No real backend/API calls, no writes to real data stores, no changes to
  production code or routes/screens reachable by real users.
- If prototyping inside the real repo (the real-app-stack option), isolate
  the work (new branch, worktree, or clearly-named throwaway files/targets)
  so it can't accidentally get merged or shipped.
- If the plan/feature is ambiguous about a UX decision, don't silently pick
  one — prototype the plausible options as separate reachable states/screens
  so the user can compare, and say so when handing it off.
