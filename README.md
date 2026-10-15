# Obzervr Analytics Portal — Product design document

_Oct 15, 2026 · Nahir Saddi_

## How to use this document

This document is the design foundation for the Analytics Portal: what it solves, who it is for, what each feature is worth and how it aligns with the Obzervr Design System. It is filled in gradually; anything we don't know yet stays as an open question, not an assumption.

- **Product source:** prototype [`Obzervr_Analytics_Portal_Live3.html`](Obzervr_Analytics_Portal_Live3.html) (Shape-Up PROD-1882).
- **Design source:** tokens in `obzervr.core` (develop branch) and Karina's Design System deck.
- **Rule:** every feature must answer _"which user question does it answer, and how do we know it helped?"_. If it can't, it is a candidate for removal.
- **Status marker:** _To validate_ = our hypothesis, still to be confirmed with users or data.

## Product summary

The Analytics Portal gives every Obzervr customer answers about their operation (work, quality, assets, safety) straight from their Data Cube, without having to open or understand Power BI. People can ask in plain language, and every number shows how it was calculated.

- **Problem:** the data already exists in the Data Cube, but getting to an answer means knowing Power BI, knowing the model, or asking Analytics for a report.
- **Proposal:** standard dashboards by role + plain-language questions + saved views, all traceable to the source measure.
- **Differentiator:** traceable answers (measure + visible query) and evidence quality metrics ("is the work we call done actually done?"), which Power BI does not give on its own.
- **Context:** Shape-Up pitch PROD-1882. Demo tenant: Agnico Fosterville.
- **Open question:** does the portal replace the dashboards published in Power BI, or live alongside them?

## Users and roles

Each dashboard is designed for one role and one concrete question; that question is what justifies it.

| Role | Main question | Horizon | Dashboard |
|---|---|---|---|
| Planner / supervisor | Are we completing this week's planned work, and where is it stuck? | Week | Execution |
| Reliability engineering | Is the work we call done actually done, and where is the evidence missing? | Month | Quality |
| Maintenance lead | Which assets are costing availability, and why? | Shift / 7 days | Assets |
| HSE / shift supervisor | Are defects being closed, and are safety controls done for real? | Month | Safety |
| Any role | A one-off question no dashboard covers | Any | Ask (AI) |

> **To validate:** who buys and who uses? (site manager vs. supervisor). It changes which feature comes first.
> **To validate:** is it used on a desktop in the office or on a tablet on site?

## Product principles

1. **One question per view.** Each dashboard opens with the question it answers; a chart that doesn't help answer it goes.
2. **Every number is traceable.** Each figure shows its source measure and query. No traceability, no trust.
3. **Real data or nothing.** When not connected, the portal shows no sample data dressed up as real; it shows an empty state that invites you to connect.
4. **Security comes from the data.** What each person sees is set by Row-Level Security in Power BI, not by the portal.
5. **From the system, not custom.** Colour, shape, type and components come from the Obzervr Design System; anything missing is proposed to the system, not invented in the portal.

## Feature map and value

Eleven features; each one's value is measured by the question it answers and by a concrete usage or outcome metric.

| Feature | Question it answers | Value to the customer | How we measure the value | Status |
|---|---|---|---|---|
| Ask (AI) | Any one-off question about the operation | An answer in seconds without knowing Power BI or requesting a report | % of questions answered without rephrasing; Analytics requests avoided | |
| Traceable answer | Where does this number come from? | Trust: every figure can be audited | % of answers where the trace is opened; complaints about wrong numbers | |
| Execution | Are we completing this week's planned work? | Spot delays and bottlenecks before the week closes | Weekly use by planners; change in % complete and overdue | |
| Quality (Work health) | Is the work we call done actually done? | Makes visible the work closed without evidence, which nobody sees today | Fewer empty completions; smaller gap between marked and captured | |
| Assets | Which assets are costing availability? | Prioritises maintenance by real downtime (bad actors) | Availability vs 85% target; downtime hours of the top 5 | |
| Safety | Are defects closed and are controls real? | Visible risk: old defects and controls signed off empty | High-priority defects open >90 days; % of empty controls | |
| My views | Get back to my question without rebuilding it | Each user builds their own board with what matters to them | Saved views per user; views reopened per week | |
| Live connection + RLS | Is this my data, and only what I'm allowed to see? | Real Data Cube data with the security Power BI already has | Successful connections; token or permission errors | |
| Filters and refresh indicator | Which site and period is this, and how fresh? | Avoids decisions on stale data or the wrong site | Questions caused by stale data | |
| Multi-site view | How does this site compare with the others? | Same template compared across a group's tenants | Use by group-level users | |
| Honest empty states | Why am I seeing nothing? | Shows no fake data; guides you to connect | Drop-off in the first session | |

> **Open question:** which feature is the reason to buy, and which drive retention? That sets the roadmap order.

## Feature detail

### Ask (AI)
- **Today:** question bar with suggestions ("Are we going to hit this week's schedule?", "How much is planned vs breakdown?"). Optional AI mode with its own settings.
- **Value:** lowers the entry barrier; nobody needs to know the data model.
- **To define:** what happens when a question doesn't map to a measure; who pays the model cost; usage limits.

### Traceable answer
- **Today:** every answer shows the semantic-model measure and the query that ran ("How it was calculated").
- **Value:** the differentiator against a generic chatbot: the number can be audited.
- **To define:** wording for non-technical users; today the trace shows DAX.

### Execution
- **Today:** completion KPIs (WTD, %, overdue, rescheduled), work orders by type (Planned / Unplanned / Breakdown), completion by team, progress by status, plan vs actual for exceptions only, work order details.
- **To define:** weekly target per site (fixed at 80% today).

### Quality (Work health)
- **Today:** completion rate, field capture score, feedback and exception capture, 4-month trend, "marked complete vs captured" by template, teams that close empty work, decline reasons.
- **Value:** the most differentiated view; it measures the quality of the evidence, not just volume.
- **To define:** threshold for "empty" agreed with customers.

### Assets
- **Today:** fleet availability vs 85%, reading trend with control limits, actual vs estimated hours, bad actors by reason code, plant hierarchy roll-up.
- **To define:** where the 85% comes from (suggested by the site).

### Safety
- **Today:** open high-priority defects, days to resolve, defects by age, open >90 days, safety controls completed and signed off empty, hazards.
- **To define:** which templates count as safety controls (picked by hand today).

### My views
- **Today:** save a question or view and return from the sidebar; views belong to a connection.
- **To define:** whether views are shared with the team.

### Live connection + RLS
- **Today:** Microsoft sign-in (MSAL), queries through Power BI `executeQueries`, RLS applied from the token.
- **To define:** the experience when the token expires or the user has no access.

### Filters, refresh and multi-site
- **Today:** site and period selector, last-refresh indicator, group scorecard and the same template compared across tenants.
- **To define:** whether comparing sites is visible to every role.

## Visual design and the Obzervr Design System

The portal now uses the Obzervr Design System tokens (`Live3_DS` version); the source is `obzervr-design-tokens.scss` in `obzervr.core`, develop branch, not the deck. The prototype's old variables still exist but point to the `--obz-*` tokens.

### Colour

| Role | Before (prototype) | Now (token) |
|---|---|---|
| Primary brand (navy) | `#012435` | `--obz-brand-primary` `#012435` (already matched) |
| Secondary brand (gold) | `#FFD073` | `--obz-brand-secondary` `#ffd073` (already matched) |
| Secondary navy | `#013d5a` | Removed: the system has one navy |
| Main chart series | teal `#128295` | `--obz-accent-blue` `#0d6cb3` (info) |
| Good / success | `#1a7a3c` | `--obz-accent-green` `#2e7d32` |
| Attention / warning | `#c9860a` | `--obz-accent-orange` `#fca60e` |
| Bad / error | `#b3261e` | `--obz-accent-red` `#c41b23` |
| Backgrounds and borders | blue-tinted greys (`#f0f2f5`, `#ccd8e0`…) | Neutral surfaces `#ffffff` / `#f4f4f4` / `#e2e2e2` / `#bbbbbb` |
| Text | `#012435`, `#4a6070` | Alpha emphasis: 87% / 60% / 38% |
| Text on navy (sidebar) | hand-picked greys | `--obz-text-on-dark-*` and `--obz-border-on-dark` tokens |

### Shape (corner radius)

The prototype had 9 different radii (13, 11, 9, 7, 6, 5, 4, 3px…); it now uses the system's 5 steps.

| Token | Value | Where the portal uses it |
|---|---|---|
| `--obz-radius-sm` | 4px | Inputs, filters, selects, chart bars |
| `--obz-radius-md` | 8px | Cards, panels, status badges, dialogs |
| `--obz-radius-lg` | 16px | Suggestion chips, hero, pills |
| `--obz-radius-full` | 9999px | AI mode toggle, status dots |

### Typography
- **Family:** Roboto (was Segoe UI); mono JetBrains Mono.
- **Weights:** 500 maximum. The prototype used 700 and 800; the system has neither.
- **Scale:** sizes moved onto the system scale (10 / 12 / 13 / 14 / 16 / 20 / 24px). Page titles = H2 20/500; card titles = Subtitle 1 14/500; small text = Caption 12/400.
- **Exception:** the large number in the Safety banner stays at 32px because the system has no display size (see the proposal for Karina).

### Spacing, elevation and motion
- **Spacing:** all CSS padding, margin and gap on the 4px grid.
- **Elevation:** cards use `--obz-elevation-resting`, dialogs `--obz-elevation-overlay`. The hero gradient is gone: the system does not use gradients.
- **Motion:** 120ms for hover and state, 240ms for transitions, with the standard and decelerate curves.

### Components
- **Status badges** = the subtle variant of `obz-label`: 10–18% accent background, caption 12/400 text, md radius. Warning uses black text, as the component does.
- **Focus:** 2px outline in brand gold, like the rest of the system.
- **Pending:** buttons, inputs, table and navigation are still custom HTML. When the portal moves to Angular they should become the library's `obz-*` components.

### Decisions to confirm with product
- The main series moved from teal to blue. The system has a teal (`#139db0`), but it fails text contrast; the blue passes.
- At weight 500 the KPIs look lighter than before. That is what the system specifies, but it is worth seeing on screen.

## Proposal for Karina: Foundations / Data visualisation

The Design System covers the operational apps well, but has no rules for charts or KPI numbers. The portal is the first product that needs them, so it proposes adding them to the system instead of solving them only here. Everything proposed uses existing tokens; there are no new colours.

### What the system is missing today
- A series palette for charts (which colour is series 1, 2, 3).
- Semantic data colours: good / attention / bad / neutral, applied to bars, gauges and heatmaps.
- Mark shape: bar radius, gauge and donut ends.
- Track (bar background) and gridline colours.
- A display size for KPI numbers: today the largest is H1 24px.
- A warning ink for text: accent-orange fails 4.5:1 on white.

### Proposed tokens

| Proposed token | Value (existing token) | Use |
|---|---|---|
| `--dv-series-1` | `--obz-accent-blue` | Main series |
| `--dv-series-2` | `--obz-brand-primary` | Comparison series (e.g. planned) |
| `--dv-series-3` | `--obz-accent-teal` | Third series |
| `--dv-good` | `--obz-accent-green` | Within target |
| `--dv-warn` | `--obz-accent-orange` | Near the limit |
| `--dv-bad` | `--obz-accent-red` | Off target |
| `--dv-neutral` | `--obz-surface-4` | No status, "other" |
| `--dv-track` | `--obz-surface-2` | Bar background |
| `--dv-grid` | `--obz-surface-3` | Gridlines |
| `--dv-mark-radius` | `--obz-radius-sm` (4px) | Bars and heatmap cells |
| `--obz-warning-ink` | `color-mix(accent-orange 55%, black)` | Warning text; the same technique `obz-label` uses for green |
| Display (unnamed yet) | 32px / 500 | Highlighted KPI numbers |

### Message to send Karina

> Karina, I aligned the Analytics Portal with the Design System using the tokens from `obzervr-design-tokens.scss` (develop). Colour, radius, typography, spacing, elevation and motion now match 1:1, and the status badges follow the subtle variant of `obz-label`.
>
> I found a gap: the system has no foundations for data visualisation. For the portal I defined series colour roles, data semantics (good / attention / bad / neutral), track and grid, and bar radius, all built only from existing tokens. I also needed a display size for KPIs and a warning ink for text, because accent-orange doesn't pass 4.5:1.
>
> Would you be open to adding this as Foundations / Data visualisation in Storybook? I can put together the proposal and the stories if that helps. Two decisions I'd like to check with you: using accent-blue (rather than teal) as the main series, and whether the KPI display should be a new type level.

- [ ] Send the message to Karina
- [ ] Agree whether to build it as a Storybook story or a Notion page first

## Accessibility

The target is WCAG 2.1 AA, the same standard as the Design System; almost all of it comes from the tokens, except what is specific to charts.

- **Text contrast (4.5:1):** solved by the emphasis tokens. Warning uses `--obz-warning-ink` because orange alone fails.
- **Chart contrast (3:1):** every system series passes on white.
- **Colour is never the only cue:** states already carry text or a symbol ("!" and similar markers in the heatmap). Keep that rule in every new chart.
- **Keyboard:** visible 2px gold focus on buttons, chips, selects and navigation.
- **Screen readers:** SVG charts have `role="img"` and `aria-label`. _To validate:_ offer the data table behind each chart.
- **Reduced motion:** `prefers-reduced-motion` is respected.

- [ ] Run an axe audit on the `Live3_DS` version

## Data and architecture

The portal stores no data: it queries each customer's Data Cube live with the person's token, and Power BI applies RLS before returning any number.

AI mode only translates the question into a measure; the number is always calculated by the Data Cube.

> **To validate:** what to show when the token expires or the analytics DB is behind.

## Success metrics

The portal succeeds if people come back every week and stop asking Analytics for reports; numeric targets are still to be set.

| Metric | What it shows | Target |
|---|---|---|
| Weekly active users / licensed users | Real adoption | To define |
| Ask questions answered without rephrasing | Quality of the question → measure mapping | To define |
| Report requests to Analytics per customer | Customer autonomy | Down vs. baseline |
| Saved views reopened per week | Recurring value of My views | To define |
| Empty completions and empty signed-off controls | Impact on the customer's operation | Down month on month |

> **To validate:** where these are measured. The account insights skill already tracks active users and published-dashboard views per customer; it can serve as the baseline.

## Risks, open questions and decisions

The main risk is trust: if an Ask answer is wrong once, the user stops using the portal.

### Risks

| Risk | Mitigation |
|---|---|
| Ask returns a wrong number | Visible trace on every answer; if the question doesn't map to a measure, say so instead of guessing |
| Stale data the user doesn't notice | Refresh indicator on every view; pipeline freshness check |
| Fabric / Power BI capacity saturated by live queries | Measure CU per query before opening to more tenants |
| The portal drifts from the Design System over time | Every colour, radius and size comes from tokens; anything new is proposed to the system |
| Comparing sites exposes sensitive information between teams | RLS + decide which roles see the multi-site view |

### Open questions
- Does the portal replace or live alongside the dashboards published in Power BI?
- Which feature is the reason to buy?
- Who pays the AI model cost, and with what limits?
- Targets per site (80% complete, 85% availability) or fixed?
- Are saved views shared with the team?

### Decisions made

| Decision | Why |
|---|---|
| No sample data when not connected | Never show figures that look real and aren't |
| Security delegated to Power BI RLS | One source of permissions, the one Analytics already manages |
| Styles from Design System tokens | Consistency with the other products and global changes in one place |

## Next steps and references

- [ ] Review the `Live3_DS` version on screen (blue series and KPIs at weight 500)
- [ ] Raise the Data visualisation proposal with Karina
- [ ] Fill in each feature's status in the map
- [ ] Validate roles and main questions with 2–3 customers
- [ ] Set numeric targets for the success metrics

### References
- Obzervr Design System — Storybook (Chromatic)
- Design System — Notion hub
- Tokens: `obzervr.core` / `Obzervr.Web.Components/theme/obzervr-design-tokens.scss` (develop)
- Status component: `Obzervr.Web.Components/ui/src/lib/label/label.component.scss`
- Prototype: [`Obzervr_Analytics_Portal_Live3.html`](Obzervr_Analytics_Portal_Live3.html) and the aligned version `Obzervr_Analytics_Portal_Live3_DS.html`
