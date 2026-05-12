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
- CloudKit + Sign in with Apple should be foundation-level, not a late sync bolt-on.
- Keep the app Apple-ecosystem-first for cost, speed, privacy posture, and maintenance simplicity.
- Local SwiftData/cache is for user-owned data only: saved places, private notes, plans, completion records, and settings.
- Data not owned by the user, such as public place metadata, shared reviews, rankings, crowd signals, and recommendations, should stay cloud-backed and should not be broadly persisted on device beyond temporary caches.
- UserDefaults only for tiny settings.
- Export/import JSON with `schemaVersion` from the beginning for user-owned data.
- Prefer CloudKit-only until a real feature proves it is insufficient. Consider a first-party backend only for advanced moderation, public ranking/recommendation systems, server-side AI jobs, cross-platform Android support, or non-Apple identity.

### Authentication
- Use Sign in with Apple as the primary identity path.
- Keep email/password out unless there is a later backend-specific reason.
- The app should be able to cache and show user data locally, but the core product is cloud-aware because cross-device data and eventual shared/crowd value matter.

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
- Prove the manual saved-place loop first.
- Plan AI import for v1 before public ship, not as a vague future idea.
- AI is not part of the persistence core.
- Add import parsing behind an `ImportService` protocol.
- Start with deterministic URL/manual fields, then add live AI import once the save/list/map/plan loop is solid.
- If AI is used to parse source links/screenshots, disclose that place data may be sent to a provider and keep user confirmation in the flow.

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

## 7) Locked decisions / open decisions

### Locked / current direction
1. **Public name:** choose a new public name later; keep generic internal
   TripTracker repo/target naming for now.
2. **Bundle ID:** lock the current generic bundle ID approach.
3. **iOS floor:** target iOS 26.x for now.
4. **AI timing:** prove the manual/database loop first, but plan AI import
   for v1 before ship.
5. **Architecture:** start CloudKit + SIWA-first. Local cache only user-owned data; keep non-user-owned/shared data cloud-backed. Prefer CloudKit-only until a feature proves backend complexity is needed.

### Still open
1. **Public brand/name:** pick later when product shape is clearer.
2. **Map provider:** Apple MapKit first unless Google Places has a clear
   quality advantage for place search/details.
3. **Backend shape:** stay CloudKit-only by default; revisit backend only for moderation, ranking/recommendations, server-side AI jobs, Android, or non-Apple identity.
4. **Platform:** stay Swift/CloudKit first or pursue Android/cross-platform later. Current recommendation: Swift/CloudKit for v1; Android only after iOS proves the loop.
5. **Monetization:** delay payments until loop is proven, or design free
   vs paid boundaries now?

## 8) Risks

- Scope creep: plans mention many pin types, tracking categories, and AI
  import paths. MVP must stay narrow.
- Privacy: saved places and trip plans are sensitive. Analytics and AI
  calls need strict boundaries.
- API dependency: Google Places/AI could add cost and key-management
  overhead. Start Apple-first, but keep `PlaceLookupService` abstract so
  Google Places can be added later if its search/details/reviews/photos
  clearly beat MapKit for save/import quality.
- Planning quality: bad routing destroys trust. Label early planner output
  honestly until feasibility logic is strong.
- Capture friction: if saving a place takes too many steps, retention dies.

## 9) Recommended next build task

Do Phase 0 first:
- keep generic bundle ID locked and iOS 26.x target
- switch Swift settings if needed
- add DesignKit
- add SIWA/CloudKit foundation decisions and entitlements plan
- create folder structure
- add `SavedPlace` model skeleton and manual save screen

Do not start with AI import. Prove the saved-place database first.

## 10) Platform recommendation

Stay Swift + CloudKit for v1. This matches the current iOS 26.x target, keeps cost low, fits SIWA/CloudKit/MapKit, and lets the product move quickly inside the Apple ecosystem. Android should be a later expansion trigger, not a starting constraint. Revisit Android when one of these is true:

- iOS retention proves the saved-place/planning loop.
- users ask for group trips with Android friends often enough to block sharing.
- public places/reviews become valuable enough to justify a non-Apple backend anyway.
- the public brand has traction beyond Gabe's Apple-first app suite.

Design service boundaries now so Android is not impossible later, but do not pay the cross-platform/backend tax before product-market evidence.
