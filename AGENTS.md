# AGENTS.md
## TripTracker — agent rules (Codex / external AI tools)

Codex and any non-Claude AI tool reads this file before doing work.
Mirrors [`CLAUDE.md`](CLAUDE.md). If the two drift, reconcile them in
the same change.

---

## 0) What you are building

**TripTracker** is a personal adventure database and trip planner.
Users save places they want to experience, organize them on a map, and
turn those saved places into realistic local adventures, weekend trips,
and bucket-list vacations.

The core product is not a generic AI itinerary chatbot. It is:

> Save places. Build adventures. Remember trips.

Initial direction from planning docs:
- free to collect saved places
- paid value comes from import, optimization, feasibility, export, and
  group/refinement features
- support local day plans, 1-3 day trips, and larger bucket-list trips
- tracking should motivate without turning the app into a fitness or
  achievement app first

Sister projects / shared ecosystem:
DesignKit · FitnessTracker / Stack · GameKit / GameDrawer.

---

## 0.1) Project status — current facts

Slow-moving facts. Update this table in the same commit when a fact
changes.

| Fact | Value | Last updated |
|------|-------|--------------|
| Repo / target name | `TripTracker` | 2026-05-12 |
| Product stage | Fresh SwiftUI starter + planning docs | 2026-05-12 |
| Current milestone | v0.1 foundation / triage | 2026-05-12 |
| Product frame | Adventure database + trip planner | 2026-05-12 |
| MVP user loop | Save place → organize → plan simple adventure | 2026-05-12 |
| Bundle ID | `lauterstar.TripTracker` currently generated; confirm before first TestFlight | 2026-05-12 |
| Target iOS | Generated as 26.2; choose real support floor before implementation | 2026-05-12 |
| Swift / UI | SwiftUI, generated Swift 5 project; decide Swift 6 before implementation | 2026-05-12 |
| Design system | Use shared DesignKit by default | 2026-05-12 |
| Persistence direction | SwiftData local-first; CloudKit sync later if needed | 2026-05-12 |

---

## 0.2) Where to look for live state

| Question | Source |
|----------|--------|
| What changed recently? | `git log --oneline -20` |
| What is the active product triage? | `docs/App-Triage.md` |
| What inspired the product? | `docs/plan1.md`, `docs/plan2.md`, `docs/plan3.md`, `UiInspo/` |
| What release notes exist? | `docs/releases/` |
| What project settings are locked? | this file §0.1 + Xcode project |

When asked for status, read `docs/App-Triage.md` and `git log` before
answering from memory.

---

## 1) Absolute constraints

### Tech / architecture
- SwiftUI app.
- Prefer Swift 6 before serious implementation.
- Use lightweight MVVM. No TCA / Redux unless explicitly asked.
- Use SwiftData for local domain data unless triage proves another
  store is better.
- Use UserDefaults only for tiny settings.
- Local-first. The app must be useful without an account.
- If sync ships, prefer CloudKit + Sign in with Apple for Apple-native
  user data sync.
- Firebase Analytics / Crashlytics / Remote Config are acceptable early
  for product learning and safety, but never store secrets in the app.
- External APIs for geocoding/maps/AI must be isolated behind services
  so they can be swapped or disabled.

### Product
- Do not build a generic chat-first AI planner.
- The saved-place database is the durable asset; AI is a helper layer.
- Capture must be fast. If saving a place feels slow, the core loop
  fails.
- Free collection is important. Put paid value around advanced import,
  route optimization, feasibility checks, trip export, collaboration,
  and refinement.
- Avoid scope creep into social network, fitness tracker, or full CRM.

### Privacy / safety
- Treat location history, saved places, trip dates, photos, and notes as
  private user data.
- Ask for location permission only when there is obvious user value.
- Do not require always-on location for MVP.
- Do not upload personal place data to AI services without an explicit
  product decision and user-facing disclosure.
- Never commit API keys, service plist secrets, or private credentials.

### Design
- Use DesignKit semantic tokens for colors, typography, spacing, radii,
  motion, and reusable components.
- No hard-coded UI colors/radii/spacing in app screens unless they are
  temporary prototypes clearly marked and removed before release.
- Respect accessibility: Dynamic Type, Reduce Motion, contrast, and
  clear location-permission copy.

---

## 2) Project structure target

Use this shape once implementation starts:

```text
TripTracker/
  App/              TripTrackerApp, app-wide wiring
  Core/             shared stores, settings, environment, errors
  Models/           SwiftData models and domain value types
  Services/         geocoding, map search, import parsing, AI planning
  Features/
    Capture/        save/import flow
    Places/         place detail, lists, filters
    Map/            saved-place map
    Planner/        trip builder and itinerary views
    Tracking/       visited/completed record layer
    Settings/       privacy, sync, export/import, about
  Resources/        assets, localization
docs/
  App-Triage.md     current product/architecture decisions
  releases/         per-version internal release notes
```

Keep feature logic close to the feature. Promote shared code only when
it has two real consumers.

---

## 3) Data model guardrails

The first real schema should handle:
- `SavedPlace`: name, coordinates, address/region, type, source, notes,
  priority, saved date, visited/completed status.
- `PlaceSource`: manual, share sheet, URL, screenshot, Google Maps,
  TikTok/Instagram/YouTube/blog, unknown.
- `TripPlan`: title, dates/duration, start/end, constraints, selected
  places, route summary, itinerary days, generated metadata.
- `ItineraryItem`: place/activity, day, time window, duration, notes,
  travel time before/after.
- `CompletionRecord`: visited date, rating, notes, photos reference,
  associated trip.

Do not overbuild every planned field on day one. Keep the schema
additive and versioned.

---

## 4) Testing expectations

- Unit tests for pure planning, clustering, filtering, import parsing,
  and feasibility logic.
- Persistence tests for SwiftData round trips once models exist.
- Export/import JSON uses `schemaVersion` and has round-trip tests.
- Service wrappers should be mockable. Do not call live external APIs in
  unit tests.
- UI tests stay minimal unless a flow is fragile or App Store critical.

---

## 5) Definition of done

A task is done when:
- code compiles or docs are directly inspected
- behavior/decision is recorded in the right doc
- privacy/data implications are called out if relevant
- no project setting is silently changed
- AGENTS.md and CLAUDE.md stay mirrored for rules changes
- release notes are updated for significant app behavior changes

---

## 6) When unsure

Choose the smallest vertical slice that proves the product loop:

1. manually save a place
2. see it on a map/list
3. create a simple plan from saved places
4. mark something visited

Ask before adding account systems, payments, live AI calls, or major
third-party SDKs.
