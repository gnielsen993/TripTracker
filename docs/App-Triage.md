# TripTracker App Triage

Last updated: 2026-05-12

## 1) Product decision

TripTracker should start as a **personal adventure database with a
planning layer**, not as a generic AI travel chatbot.

Core promise:

> Save places you want to experience. Turn them into realistic plans.
> Remember what you completed.

The planning docs already point to the right center of gravity:
- `docs/plan1.md`: personal bucket-list travel agent
- `docs/plan2.md`: broader adventure database for local + travel use
- `docs/plan3.md`: lightweight completion/tracking layer

Decision: use `plan2` as the main product frame, with `plan1` as the
AI planning value prop and `plan3` as a later retention layer.

## 2) MVP cut

MVP should prove one loop, not the whole vision.

### MVP loop
1. User manually saves a place.
2. User sees saved places in list + map.
3. User tags/prioritizes places.
4. User creates a simple day/weekend plan from saved places.
5. User marks a place or plan item as visited/completed.

### In MVP
- Manual save place form.
- Saved places list.
- Map with saved pins.
- Place detail with notes, source URL, type, priority, visited status.
- Basic filters: type, priority, visited/unvisited, distance/region if
  location is available.
- Simple planner: choose start, duration, pace, must-include pins;
  produce a structured itinerary without full AI optimization.
- Export/import JSON backup.
- Settings: privacy, location permission explanation, backup, about.

### Not MVP
- Full TikTok/Instagram import automation.
- Group trip planning.
- Payments/subscriptions.
- Full AI route optimizer.
- Social feed or public sharing.
- Deep national parks/states/countries progress dashboards.
- Always-on location tracking.

## 3) First key architecture decisions

### App platform
- SwiftUI app.
- Convert project to Swift 6 before meaningful implementation unless a
  package blocks it.
- Current generated iOS target is 26.2. Decide actual support floor
  before coding. Recommendation: iOS 17+ or iOS 18+, depending on MapKit
  and SwiftData needs.

### Design system
- Add local package dependency on `../DesignKit` early.
- Use DesignKit for tokens and common cards/buttons/chrome.
- Do not hard-code final UI colors or spacing.

### Data/storage
- SwiftData local-first is the default.
- UserDefaults only for tiny settings.
- Export/import JSON with `schemaVersion` from the beginning.
- CloudKit sync is a later phase, not required for MVP.

### Authentication
- No account required for MVP.
- If sync arrives, use Sign in with Apple + CloudKit.
- Do not create email/password accounts.

### Maps/location
- Use MapKit for first-party map UI.
- Ask for location only for “near me,” distance sorting, and local
  adventure planning.
- Do not request always-on location.
- Store saved place coordinates, not continuous movement history.

### External place data
- Start with manual entry + URL/source capture.
- Add geocoding/search behind a `PlaceLookupService` protocol.
- Candidate providers: Apple MapKit/CLGeocoder first; Google Places only
  if Apple search quality blocks the product.
- Keep provider-specific IDs optional and isolated.

### AI/import
- AI is not part of the persistence core.
- Add import parsing behind an `ImportService` protocol.
- Start with deterministic URL/manual fields before live AI calls.
- If AI is used to parse source links/screenshots, disclose that place
  data may be sent to a provider and keep user confirmation in the flow.

### Analytics/crash reporting
- Firebase Analytics/Crashlytics/Remote Config is acceptable early if
  configured safely.
- Track product events, not private content: place_saved, plan_created,
  import_started, import_confirmed, backup_exported.
- Do not log place names, notes, exact coordinates, trip dates, or source
  URLs in analytics.

## 4) Proposed first schema

Keep v1 schema narrow and additive.

### SavedPlace
- `id: UUID`
- `name: String`
- `type: PlaceType`
- `latitude: Double?`
- `longitude: Double?`
- `address: String?`
- `locality: String?`
- `region: String?`
- `country: String?`
- `sourceURL: URL?`
- `sourceKind: PlaceSourceKind`
- `notes: String`
- `priority: PlacePriority`
- `status: PlaceStatus`
- `createdAt: Date`
- `updatedAt: Date`
- `visitedAt: Date?`

### PlaceType
Start with broad types only:
- hike
- restaurant
- lodging
- viewpoint/photo spot
- activity/attraction
- city/region
- park/nature
- other

### TripPlan
- `id: UUID`
- `title: String`
- `startDate: Date?`
- `durationDays: Int`
- `startLocationName: String?`
- `pace: TripPace`
- `budgetLevel: BudgetLevel?`
- `notes: String`
- `createdAt: Date`
- `updatedAt: Date`

### ItineraryItem
- `id: UUID`
- `tripPlanID: UUID`
- `savedPlaceID: UUID?`
- `title: String`
- `dayIndex: Int`
- `sortOrder: Int`
- `durationMinutes: Int?`
- `timeWindow: String?`
- `notes: String`

### CompletionRecord
- `id: UUID`
- `savedPlaceID: UUID?`
- `tripPlanID: UUID?`
- `completedAt: Date`
- `rating: Int?`
- `notes: String`

## 5) Early screen map

1. **Home / Today**
   - quick actions: Save place, Plan adventure
   - nearby/unvisited prompts later

2. **Places**
   - list, filters, search
   - place detail

3. **Map**
   - saved pins
   - filter by type/status/priority

4. **Planner**
   - choose constraints
   - select must-include places
   - generated/simple itinerary

5. **Progress / Memories**
   - visited places and completed trips
   - keep basic for MVP

6. **Settings**
   - DesignKit theme, privacy, backup, about

## 6) First implementation phases

### Phase 0 — Foundation
- Confirm bundle ID.
- Choose real iOS deployment target.
- Switch to Swift 6 if clean.
- Add DesignKit package dependency.
- Create app folder structure.
- Add release notes template.

### Phase 1 — Local saved places
- SwiftData models for `SavedPlace` and enums.
- Manual add/edit/delete place.
- Places list + detail.
- Basic tests for model/export shape.

### Phase 2 — Map and location
- MapKit saved pins.
- Optional current location permission.
- Distance sorting if permission granted.
- Privacy copy and no always-on tracking.

### Phase 3 — Simple planner
- TripPlan + ItineraryItem models.
- Select saved places for a plan.
- Basic duration/pace constraints.
- No live AI required.

### Phase 4 — Import helpers
- Share sheet / URL capture.
- Source URL storage.
- Manual confirmation flow.
- Optional geocoding/search provider.

### Phase 5 — Backup and polish
- JSON export/import.
- Empty states.
- DesignKit theme screen.
- Accessibility pass.

## 7) Open decisions for Gabe

1. **Name:** keep TripTracker, or move toward Adventure List / Adventure
   Atlas / BucketMap style branding?
2. **Bundle ID:** current generated ID is `lauterstar.TripTracker`; lock
   or change before first TestFlight?
3. **iOS floor:** iOS 17+ for wider reach, or iOS 18+ if newer MapKit/UI
   APIs matter?
4. **Map provider:** start Apple-only unless search quality is poor?
5. **AI timing:** include AI import in v0.1, or prove manual/database loop
   first?
6. **Monetization:** delay payments until loop is proven, or design free
   vs paid boundaries now?

## 8) Risks

- Scope creep: plans mention many pin types, tracking categories, and AI
  import paths. MVP must stay narrow.
- Privacy: saved places and trip plans are sensitive. Analytics and AI
  calls need strict boundaries.
- API dependency: Google Places/AI could add cost and key-management
  overhead. Start with Apple/manual where possible.
- Planning quality: bad routing destroys trust. Label early planner output
  honestly until feasibility logic is strong.
- Capture friction: if saving a place takes too many steps, retention dies.

## 9) Recommended next build task

Do Phase 0 first:
- confirm bundle ID + iOS floor
- switch Swift settings if needed
- add DesignKit
- create folder structure
- add `SavedPlace` model skeleton and manual save screen

Do not start with AI import. Prove the saved-place database first.
