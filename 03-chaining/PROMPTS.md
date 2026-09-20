# PROMPTS.md: Living Prompt Pack

> Module 3 · Prompt Chaining. Re-architect the build with prompt chains; capture the reusable ones here.

## How to use this pack

_Each prompt is a reusable step. Chain them: the output of one becomes the input to the next._

## Prompt chain: The Guided Path — Week-One Activation Chain

### Step 1: Expand, build new screens in a strict sequence
```
You are extending my existing prototype "The Retention Engine" (TanStack Start, file-based

routing, dark dense B2B-SaaS console theme, no backend — all state is local/localStorage).

I'm turning the four-step activation CHECKLIST on /activation into a real GUIDED ONBOARDING FLOW.

Before writing any code, think step by step and show me your plan first:

Map the flow: list every screen in order and the single job each one does.

For each screen, state how the user arrives, what advances them, and where "back" and

"skip/dismiss" go.

State exactly what local state changes on each step and how it persists.

Then, after the plan, build it.

Screens to create (this is the guided week-one path):

Kickoff — a welcome step that frames the four steps and a primary "Start" action.

Connect data source — pick a source from a small hard-coded list.

Invite a teammate — an email invite form.

Run your first retention cohort — THE HERO / "aha" step: a short configure view, then a

result view. Completing this marks the account as having reached first value.

Set weekly digest — choose a day/time for a digest.

First value reached — a completion screen summarising what they did.

How the screens connect:

Entry: the "Walk the guided path" button on the Evidence (/) screen and the checklist on

/activation both lead into this flow.

The flow is a sequential stepper: Kickoff → Connect → Invite → Run cohort → Digest → Completion.

Each completed step ticks its row in the /activation checklist and persists, so the checklist

always reflects real progress and the flow can be resumed mid-way.

Completing the "Run cohort" step sets the account to activated and updates the Cohort Readout

(/cohorts) and the kill-switch card on /.

The Completion screen links onward to /cohorts.

Visual anchor: adopt the LAYOUT, spacing, and step/onboarding STRUCTURE from the attached screenshot — but translate it into MY existing dark console visual

language (my color tokens, type scale, nav rail, status dots). Match the structure, not the colors.

Keep everything local and seeded. No backend, no real integrations — simulate.
```

### Step 2: Behavior, hard-code the states
```
Now hard-code the behavioral states for the guided onboarding flow you just built. Use these EXACT

strings — do not paraphrase. Simulate all latency with setTimeout; nothing hits a network.

CONNECT DATA SOURCE

Loading: "Connecting to your data source…" subtext: "This usually takes a few seconds."

Success: "Data connected. We found 3 sources you can pull from."

Error: "We couldn't connect to that source. Check your access and try again." button: "Try again"

INVITE A TEAMMATE

Empty: "No teammates invited yet. Retention improves fastest when the whole team can see it."

Invalid email: "That doesn't look like a valid email address."

Success: "Invite sent to {email}. We'll nudge them if they don't join in 3 days."

Duplicate: "You've already invited {email}."

RUN YOUR FIRST RETENTION COHORT (the aha step)

Blocked (no data connected): "Connect a data source first — a cohort needs data to run."

with a link back to the Connect step.

Loading: "Building your first retention cohort…" subtext: "Crunching 12 weeks of account activity."

Success: "Your first cohort is ready. Guided accounts retain 17 pts better at 90 days."

(this marks the account activated and updates /cohorts + the kill-switch)

Error: "Something went wrong building your cohort. Your data is safe — try running it again."

SET WEEKLY DIGEST

Success: "Weekly digest scheduled for Monday 9:00am. First one arrives next week."
FAILURE PATH — SKIP / DISMISS (the path the hypothesis must be able to test)

If the user skips the Run-cohort step: show a persistent banner —

"You skipped the step that drives first value. Accounts that skip it activate at 22%."

with a "Resume the guided path" action. Leave the step unchecked and count the account as

unguided / not activated in /cohorts and the kill-switch.

If the user dismisses the whole flow: confirm first —

"Leave the guided path? You can pick up where you left off anytime."

buttons: "Keep going" / "Leave for now".

RESUME

Returning to /activation mid-flow: "You're {n} of 4 steps in. Pick up where you left off."

with the progress reflected in the checklist.

COHORT READOUT edge case

Low sample size: "Not enough data yet — a cohort needs at least 20 accounts to be meaningful."
Wire each state to the real interaction. Don't invent extra screens — only add these states to the flow that already exists.
```

### Step 3: Refine, one surgical polish
```
First, AUDIT ONLY — do not change any code yet.

Review the guided onboarding flow (Kickoff → Connect → Invite → Run cohort → Digest → Completion)

and list every place the {{SURGICAL_TARGET}} is inconsistent: step numbering, completed vs current

vs upcoming styling, whether it appears on every step, how it wraps on mobile, and whether it uses

my existing dark-console color tokens and status-dot pattern. Present the list and wait.

Then make ONE surgical change:

Replace the per-step progress indicators with a single shared, reusable ProgressStepper component

used on EVERY step of the flow. It shows "Step X of 4" with clearly distinct completed / current /

upcoming states, built from my existing color tokens and status-dot pattern.

Don't change anything else — no other components, no copy, no routes, no state logic, no theme.

Only the progress stepper.
```

## Reusable techniques learned

- _____
- _____

## What broke (and the fix)

_Where a single mega-prompt failed and chaining fixed it._

_____
