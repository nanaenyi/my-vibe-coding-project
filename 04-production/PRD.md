# Living PRD

> Module 4 · Production Specs. Refactor for readability; extract a living PRD that stays true as the build evolves.

## Problem

_What user problem does this solve? Tie to the validated hypothesis._

New B2B SaaS accounts stall before they ever reach value, and a meaningful share churn inside the first quarter. The prototype's baseline (illustrative, see honesty note) is **30% 90-day churn**, **22% week-one activation**, and **1.4 seats active per account** — adoption rarely spreads past the original buyer. Churned accounts describe the same failure in their own words: they signed up, looked around, and never understood what the product was supposed to do for their team.

This PRD is anchored to a single hypothesis the prototype exists to test:

> **We believe** a guided week-one activation path that surfaces the core "aha" action **will cause** more new accounts to reach first value and stay past 90 days.
> **Win condition:** week-one activation rises above **40%** and 90-day churn drops.
> **Kill switch:** if guiding the first action doesn't move activation, onboarding isn't the real problem — pivot.

**Honesty note — validation status.** The hypothesis is *operationalized*, not yet *validated*. Every metric in the prototype is seeded, in-memory mock data, including the current guided activation figure (**38%**, shown against the 40% threshold). No real accounts have been measured. "Validated hypothesis" here means the prototype makes the test *runnable and legible* — it defines the intervention, the win condition, and the kill switch — so the Problem is tied to a hypothesis that is ready to be tested with real data, not one that real data has yet confirmed.

## Users & jobs

- **Primary user:** The original buyer/admin at a B2B SaaS customer, in their first seven days after signup.
- **Job to be done:** I want to quickly reach a first meaningful result and understand what the product does for us, so I can justify rolling it out to my team instead of abandoning it.

## Scope

- **In:** - Four screens: **Evidence (`/`)**, **Guided Week-One Path (`/activation`)**, **Cohort Readout (`/cohorts`)**, **Account Detail (`/accounts/$id`)**.
- The guided four-step week-one path: connect data → invite a teammate → **run first retention cohort (the "aha")** → set a weekly digest.
- Kill-switch logic driven by a single shared threshold, visible on the home, cohort, and activation screens.
- Guided-vs-unguided cohort comparison across four weeks (activation, 90-day retention, seats/account, sample size) with an honesty note on significance.
- Account-level timeline drill-down, reachable from the cohort list and from home-screen quote cards.
- The failure path: skip / dismiss controls so a non-activating account can be demonstrated.
- **Out (explicitly):** - Real backend, authentication, or multi-tenancy — **all state is local (in-memory / localStorage), seeded with mock rows.**
- Real data-source connection, real teammate email invites, real digest delivery — **all simulated.**
- Real cohort computation and a real statistical-significance engine — the readout shows seeded results and an honesty *note*, not a computed p-value.
- Analytics instrumentation to a real event store (events below are local state transitions today).
- Mobile-optimized layouts, billing, notifications, and any brand/logo assets.

## Requirements

| # | Requirement | Priority | Acceptance criteria |
|---|---|---|---|
| 1 | Evidence screen states hypothesis, three baseline metrics, and two verbatim churn quotes | Must | Home renders hypothesis text, the 30% / 22% / 1.4 baselines, and ≥2 quote cards; each quote card links to the matching Account Detail. |
| 2 | Guided week-one path with four ordered steps | Must | 'activation' shows the four steps in order; each can be opened, completed, and its state persists across reload |

## Data & events

_What gets stored, what gets tracked._

**Entities (currently seeded local objects, not a database)**

- **Account:** id, signup date, session history, seats active, activation status, cohort assignment (guided / unguided), risk flag, associated churn quote.
- **Activation path state:** per-step status (not started / complete / skipped), current step, first-value-reached flag.
- **Cohort metrics:** per-cohort activation rate, 90-day retention, seats/account, sample size; derived kill-switch state.

**Events a production build would instrument (today these are in-memory state transitions that go nowhere)**

- `path_started`
- `step_completed` (with `step` = connect_data | invite_teammate | run_cohort | set_digest)
- `aha_reached` (fires on cohort run; the activation event)
- `path_skipped`, `path_dismissed`
- `data_connected`, `invite_sent`, `digest_set`
- `account_activated`, `account_churned`

**Honesty note.** No events are emitted to an analytics pipeline; the kill-switch and cohort figures are computed from seeded values, not from captured event streams.

## Open questions

1. **Causation vs correlation** — does reaching the aha action *cause* retention, or do already-committed accounts simply complete it? The current design can't separate the two.
2. **Is 40% the right win threshold**, and over what exact week-one window is activation measured?
3. **Which step is the real driver** — is the retention cohort genuinely the aha, or is it "connect data" (the prerequisite) or "invite a teammate" (seat expansion)?
4. **Sample size and significance** — what minimum n and test duration make the guided-vs-unguided comparison trustworthy? The honesty note flags this but doesn't answer it.
5. **What is "first value" in production**, and can it be detected from real product telemetry the same way the prototype marks it?
6. **Feasibility of real data connection** — the whole path assumes an account can connect a data source in week one; how realistic is that, and what's the fallback if they can't?
7. **Do invited teammates matter to retention**, or is seats/account a vanity signal?
8. **Segment differences** — does the intervention work across account sizes/verticals, or only for some?
