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
| Visitor | 1 Route curation, 2 Tour guide & survey | Sunlit blue, immersive deep-ocean theme with drifting "marine snow" | Only their own route and debrief |
| OceanX Education staff | 3 Impact dashboard | Near-black "operations room", blue accent, no animation | Every individual response, scores, exports |
| Partner | 4 Partner hub | Twilight-zone indigo with a violet accent and gold for "your programme" | Aggregated results only, benchmarked against the network |

The prototype opens on a **"One tour, three views" landing page** with one card per audience, each in that view's colours and stating what the audience sees and shares (or its access level). Choosing a card opens that view; the tab bar then shows only that audience's tabs plus an **All views** button to return. The colours follow the ocean depth story: sunlit (visitor), midnight (OceanX staff), twilight (partner).


1. **Route curation (visitor, before the visit):** three taps capture intent, preferred ocean zone and a baseline ocean-agency score (1–5). The app curates a four-stop route, draws it as a dive profile, and issues a route card with a code and a QR code.
2. **Tour guide and debrief (visitor, during and after):** the QR code opens a phone-style tour guide the visitor carries round the exhibition:
   - an **exhibit map** of the hall (shoreline, sunlit, twilight and deep-sea zones) showing their numbered stops, the route line, the next stop and a "you are here" marker;
   - a **stop card** with walking directions from the previous stop and what to look for;
   - an **audio guide** button with a progress bar and transcript, read aloud by the device's built-in voice where the browser supports it;
   - **Mark as visited**, next/previous arrows and an **All stops** list.

   After the tour they answer a short swipe-or-tap card deck (true/false concept cards, an exhibit-clarity card, pledge cards), write one reflection sentence, and repeat the agency question to see their before→after result.
3. **Impact dashboard (OceanX staff only):** headline results against targets, the partner network North Star, the survey method test (AI debrief vs static survey, the prototype's core hypothesis), agency shift, learning depth (0–3 rubric), stop-by-stop review flags with the most common misconception, results by audience, every response, CSV export and AI-drafted programme recommendations.
4. **Partner hub (partner programme teams):** a partner signs in (simulated with a "Signed in as" picker) and sees:
   - **their programme's core indicators** (concepts understood, agency shift, actionable intent, learning depth), each compared with the network median;
   - a **suggested next step** that names their biggest gap and the partner lesson that moved the same indicator;
   - **how they compare with the network:** one dot per programme on each indicator, with the network median and OceanX targets, plus a table view;
   - **what worked elsewhere:** a library of changes other partners made and the before→after result, with ideas matching their gap first and an **Adopt this idea** button;
   - a **decision log:** record a decision (improve, expand, adopt another partner's approach, or pause), the indicator it should move, the evidence used and a review date. Each saved decision updates the **network North Star** live;
   - **data sharing:** choose Private, Anonymised (by programme type) or Named, plus a plain list of what partners can and can't see;
   - **Print or save as PDF** for an outcome report to share with funders.

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

Two database collections:

- `responses`: one record per visitor keyed by route code. The pre-tour taps create it and the debrief completes it.
- `decisions`: one record per partner decision: programme, decision type, indicator it should move, lesson used as evidence (if any), what will change, review date and timestamp.

Partner programmes are reported as aggregates on the same core indicators. The OceanX tour's figures are calculated live from `responses`; the other partners' figures are fixed illustrative values.

Key fields: intent, zone, baseline and post agency (1–5), route and visited stops, status (`pre` / `started` / `complete`), timestamps and survey seconds, per-card answers with correctness and time, reflection text, recall score (0–3) and its source, actionable-intent flag, ocean literacy principles, pledge category.

## AI capability

Claude, built in as the equivalent of an AI Builder custom prompt:

- writes each visitor's card deck and reflection question from the stops they visited;
- scores the reflection against the 0–3 recall rubric and the Yes/No actionable-intent rule;
- drafts programme recommendations on the staff dashboard.

If the AI is unavailable, a fixed card deck and keyword-based scoring are used instead.

## Trackable signals

Saved records, status flags (`pre` → `started` → `complete`), timestamps and survey duration, rubric scores, intent flags, automatic review flags for stops below 60% concept accuracy, and logged partner decisions with their evidence source and review date.

## Success metrics measured

| Metric | Target |
|---|---|
| Survey completion rate | ≥30% relative lift over control |
| Median completion time | ≤90 seconds |
| Average concept recall | ≥2.0 (0–3 rubric) |
| Actionable intent | ≥60% of responses |
| **North Star:** partner programmes with a documented improvement or expansion decision using shared evidence | Rising each quarter (demo starts at 3 of 6 programmes, 50%) |

## Known limitations

- Exhibit stops, concepts and misconceptions are placeholders, not OceanX's real exhibits.
- Partner organisations, programmes, lessons and their figures are fictional. Partner sign-in is simulated with a picker; a real version would give each partner its own login and show it only its own programme.
- Partners never see individual visitor records, and groups under 10 responses are hidden from other partners. These rules are shown in the prototype but would need formal data-sharing agreements and consent wording before a pilot.
- Visited stops are self-reported, not tracked by sensors.
- The exhibit map layout and walking distances are illustrative, not OceanX's real floor plan.
- The audio guide uses the device's built-in text-to-speech voice reading a short script; where speech isn't supported it plays silently with the transcript.
- The QR hand-off assumes the visitor is signed in on their phone. A scanned QR link opens the tour guide directly; otherwise an unfinished route's code is pre-filled and the landing page shows first.
- 40 labelled demo records fill the dashboard until real data arrives, and 3 labelled demo decisions seed the partner decision log.
- The static control survey runs outside the app.

## GenAI declaration

The prototype was built with the help of Claude (Anthropic) via vibe coding, and Claude also serves as its AI capability.
