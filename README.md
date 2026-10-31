# OceanX Personalised Tour Curation and Impact Measurement

A low-code prototype for IS615 Digital Transformation Strategy (sponsor: OceanX Education).
It tests whether a route-personalised AI debrief raises post-tour survey completion and yields richer learning evidence than a static survey, and whether sharing those results across the partner network leads partners to make evidence-based programme decisions (the North Star metric).

**Author:** Koh Yi Jing Priscilla
**Built with:** Claude artifact (used as a Power Apps equivalent)
**Source file:** `oceanx-impact-loop.html`

---

## What it does

One workflow, end to end, seen by three audiences:

| Audience | Views | Look | What they see |
|---|---|---|---|
| Visitor | 1 Route curation (iPad kiosk), 2 Tour guide & survey (own phone) | Sunlit blue, immersive deep-ocean theme with drifting "marine snow" | Only their own route and debrief |
| OceanX Education staff (master admin) | Impact dashboard, plus every visitor and partner view | Near-black "operations room", blue accent, no animation | Every individual response, scores, exports, and a preview of any partner's hub |
| Partner | Partner hub | Twilight-zone indigo with a violet accent and gold for "your programme" | Aggregated results only, benchmarked against the network |

The prototype opens on a **"One tour, three views" landing page** with one card per audience, each in that view's colours and stating what the audience sees and shares (or its access level). Choosing a card opens that view. Visitors and partners only ever see their own tabs; **OceanX is the master admin** and sees every tab, with a **Preview as** picker in the Partner hub that shows exactly what a given partner sees. An **All views** button returns to the landing page. The colours follow the ocean depth story: sunlit (visitor), midnight (OceanX staff), twilight (partner).


1. **Route curation (visitor, on a shared iPad kiosk at the entrance):** three taps capture intent, preferred ocean zone and a baseline ocean-agency score (1–5). No personal data is typed on the iPad. The app curates a four-stop route, draws it as a dive profile, and issues a route card with a code and a QR code. After a minute without a touch, the kiosk resets for the next visitor.
2. **Registration, tour guide and debrief (visitor, on their own phone):**
   - **Registration first:** scanning the QR code opens a registration screen. **Email is required** to open the tour guide (used for the route and one follow-up about this visit). **Marketing consent** is a separate, optional box, unticked by default. A note asks a parent or guardian to register for children.
   - **Tour guide:** an exhibit map with the route, a "you are here" marker and the next stop; a stop card with walking directions and what to look for; an audio guide with transcript; **Mark as visited**, next/previous arrows and an **All stops** list.
   - **Debrief:** a short swipe-or-tap card deck (true/false concept cards, an exhibit-clarity card, pledge cards), one reflection sentence, and the agency question again for a before→after result. The result screen shows the **date** of their three-question check-in (two weeks later by default).
3. **Impact dashboard (OceanX staff):**
   - **At a glance:** the visitor funnel (planned on the iPad → registered on phone → debriefed), follow-ups overdue and due soon, the network North Star, and partners behind the median.
   - **Follow-up queue:** every action OceanX owes, each with a **due date that can be changed** and a status (overdue, due soon, upcoming, done): visitor check-ins (batched by week), reminders to visitors who registered but didn't finish, exhibit stops to fix and **re-measure by** a date, partner decision reviews, partners to support or scale, partners with too few responses, and partner support calls. Partner reviews close automatically when the partner sends a progress update.
   - **From partners:** progress updates, new results and support requests sent from the Partner hub.
   - **Progress over time:** each metric month by month (debrief completion, concepts understood, learning depth, actionable intent, agency shift, network North Star), with key events and partner decisions marked on the timeline, previous vs current month, change since the start, and a "then vs now" table.
   - Headline results against targets, the survey method test (AI debrief vs static survey), agency shift, learning depth (0–3 rubric), stop-by-stop review flags with the most common misconception, results by audience, every response (with a ✉ tag for registered visitors), CSV export and AI-drafted programme recommendations.
4. **Partner hub (partner programme teams):** a partner signs in (simulated with a picker) and sees:
   - a **review-due prompt** when a decision's review date has passed without an update;
   - an **at-a-glance strip**: indicators above the network median, learners debriefed, decisions logged, next (or overdue) review and the North Star;
   - **their programme's core indicators** compared with the network median, each with a **Q1 → Q3 sparkline** of their own progress;
   - a **suggested next step**, **how they compare with the network**, **what worked elsewhere** (with **Adopt this idea**) and a **decision log** with review dates that feeds the North Star;
   - **Share with OceanX:** send a **progress update** on a decision (did the indicator move, new value, what happened), **new results** for a period, or a **support request** with a preferred call date. These go to OceanX only, not to other partners;
   - **Your follow-ups with OceanX:** review dates, booked support calls and the next partner network session, each with its status;
   - **data sharing with other partners:** Private, Anonymised or Named, plus what partners can and can't see;
   - **Print or save as PDF** for an outcome report.

## How to run it

- **Live version:** open the published artifact in Claude (claude.ai). The database, AI features and staff sign-in only work there.
- **This file on its own:** opening `oceanx-impact-loop.html` in a browser shows the layout only. Saving, AI cards, scoring and the dashboard data need the Claude artifact runtime.
- **Standalone demo:** `index.html` (also the hosted site) opens in any browser with no sign-in. It uses 40 demo records, the built-in card deck and keyword scoring, and saves test runs and partner decisions in that browser only (a Reset button restores the demo data).

## File structure

Everything is in one self-contained HTML file:

| Part | Contents |
|---|---|
| HTML | The landing page plus four views: Route curation and Tour guide & survey (visitor), Impact dashboard (OceanX staff), Partner hub (partner) |
| CSS | Three audience themes switched by `body[data-aud]` (visitor, staff, partner), fonts (Archivo, Instrument Sans), responsive layout, print styles for the partner report |
| JavaScript | Route logic, QR code (qrcodejs), phone tour guide (exhibit map, directions, audio guide), swipe deck, timers, rubric scoring, dashboard charts and tables, CSV export, AI prompts, partner benchmarks, lessons library, decision log and North Star |

## Data model

Four database collections:

- `responses`: one record per visitor keyed by route code. The iPad taps create it, phone registration adds `contact` (email, registration time, marketing consent and follow-up due date), and the debrief completes it.
- `decisions`: one record per partner decision: programme, decision type, indicator it should move, lesson used as evidence (if any), what will change, review date and timestamp.
- `shares`: what partners send OceanX: progress updates (decision, outcome, new value, note), results (period, participants, concepts, intent) and support requests (topic, preferred date, note).
- `followups`: OceanX's changes to the follow-up queue: a new due date, or done with the date it was done.

Partner programmes are reported as aggregates on the same core indicators. The OceanX tour's figures are calculated live from `responses`; the other partners' figures, and their Q1–Q2 history, are fixed illustrative values. The tour's May–August history is illustrative; months with real records, and the North Star in every month, are calculated live.

Key fields: intent, zone, baseline and post agency (1–5), route and visited stops, status (`pre` / `started` / `complete`), timestamps and survey seconds, per-card answers with correctness and time, reflection text, recall score (0–3) and its source, actionable-intent flag, ocean literacy principles, pledge category.

## AI capability

Claude, built in as the equivalent of an AI Builder custom prompt:

- writes each visitor's card deck and reflection question from the stops they visited;
- scores the reflection against the 0–3 recall rubric and the Yes/No actionable-intent rule;
- drafts programme recommendations on the staff dashboard.

If the AI is unavailable, a fixed card deck and keyword-based scoring are used instead.

## Trackable signals

Saved records, status flags (`pre` → `started` → `complete`), timestamps and survey duration, rubric scores, intent flags, automatic review flags for stops below 60% concept accuracy, logged partner decisions with their evidence source and review date, partner progress updates and support requests, and follow-up due dates and completion dates.

## Success metrics measured

| Metric | Target |
|---|---|
| Survey completion rate | ≥30% relative lift over control |
| Median completion time | ≤90 seconds |
| Average concept recall | ≥2.0 (0–3 rubric) |
| Actionable intent | ≥60% of responses |
| Phone registration | Share of routes planned on the iPad that are registered on a phone (new measure, no target yet) |
| Follow-ups on time | Follow-ups completed by their due date (new measure, no target yet) |
| **North Star:** partner programmes with a documented improvement or expansion decision using shared evidence | Rising each quarter (demo starts at 3 of 6 programmes, 50%) |

## Known limitations

- Exhibit stops, concepts and misconceptions are placeholders, not OceanX's real exhibits.
- Partner organisations, programmes, lessons and their figures are fictional. Partner sign-in is simulated with a picker; a real version would give each partner its own login and show it only its own programme.
- Partners never see individual visitor records, and groups under 10 responses are hidden from other partners. These rules are shown in the prototype but would need formal data-sharing agreements and consent wording before a pilot.
- Visited stops are self-reported, not tracked by sensors.
- The exhibit map layout and walking distances are illustrative, not OceanX's real floor plan.
- The audio guide uses the device's built-in text-to-speech voice reading a short script; where speech isn't supported it plays silently with the transcript.
- No emails are actually sent; marking a follow-up done only records it. Demo visitors who scanned their QR code carry fictional `@example.com` addresses. A real pilot would need PDPA-compliant notice and consent wording, a retention period and an unsubscribe link; making email required is only defensible because it's needed for the route and follow-up, which is why marketing is a separate optional box.
- Visitors who plan a route on the iPad but never scan the QR code leave no email and can't be followed up. The funnel on the dashboard shows how many.
- OceanX admin access and partner sign-in are simulated by the landing page. A real version needs proper logins and roles.
- In the demo, visitor follow-ups more than four weeks old are shown as already done so the queue looks like a working one.
- The QR hand-off assumes the visitor is signed in on their phone. A scanned QR link opens the tour guide directly; otherwise an unfinished route's code is pre-filled and the landing page shows first.
- 40 labelled demo records fill the dashboard until real data arrives, and 3 labelled demo decisions seed the partner decision log.
- The static control survey runs outside the app.

## GenAI declaration

The prototype was built with the help of Claude (Anthropic) via vibe coding, and Claude also serves as its AI capability.

## Device testing

For v4, re-checked at phone width (375 px) and iPad Air portrait and landscape (820 × 1180, 1180 × 820). For v3, checked in a browser at phone width (375 px) and at iPad sizes: iPad mini (768 × 1024), iPad Air (820 × 1180), iPad Pro 12.9" (1024 × 1366), and landscape (1180 × 820, 1366 × 1024). No page scrolls sideways at any size. On a phone or portrait iPad, the staff dashboard's wide tables scroll inside their own boxes. These checks used browser screen-size emulation, so a final pass on a real iPad in Safari is still worth doing.

## Version history

- **v4:** visitors register with a required email on their phone after scanning the QR code (marketing consent separate and optional), with the iPad kept as an anonymous kiosk that resets itself; OceanX as master admin with access to every view and a "Preview as" partner picker; a dated follow-up queue (visitor check-ins, debrief reminders, exhibit re-measures, partner reviews, support calls) with editable due dates; "Share with OceanX" in the Partner hub (progress updates, results, support requests) with review-due prompts and a partner follow-up list; progress over time for every metric with decisions marked on the timeline, and quarterly sparklines for partners.
- **v3:** OceanX logo in the top bar on every page (with a "student prototype, not an official OceanX site" note in the footer); at-a-glance panels for OceanX (with "Next moves with partners") and for partners; optional visitor email with consent for a two-week follow-up; iPad layout checks.
- **v2:** landing page with three audience views (visitor, OceanX staff, partner) in sunlit, midnight and twilight colours; Partner hub with network benchmarks, shared lessons, decision log and North Star; survey method test shown on the staff dashboard.
- **v1 (midterm):** route curation, phone tour guide, swipe debrief and impact dashboard.

