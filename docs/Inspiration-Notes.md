# TripTracker Inspiration Notes

Last updated: 2026-05-12

## Source notes

Current local references:
- `UiInspo/AllTrails iOS 26.png`
- `UiInspo/Flighty iOS 12.png`
- `UiInspo/Tesla iOS 9.png`

Mobbin public pages are reachable, but search/discover content is mostly dynamic and did not expose useful result detail through simple fetch. Keep adding exported screenshots from Mobbin or other apps to `UiInspo/` when a pattern feels useful.

## Product screen inventory

### 1) Explore / Map
Purpose: browse saved and public/cloud places.

Patterns to consider:
- Full-screen map as the primary surface.
- Search bar floating at the top.
- Horizontal filter chips below search: All, Hikes, Food, Viewpoints, Parks, Saved, Visited.
- Map controls stacked on one side: layers, 3D/terrain, download/offline if later needed.
- Bottom sheet with count or selected region summary.
- Floating current-location button.

Inspo: AllTrails map screen. Strong for saved adventure discovery.

### 2) Saved Places
Purpose: the user's private adventure database.

Patterns to consider:
- List/map toggle.
- Filters: type, priority, visited, region, source, nearby.
- Quick sort: recently saved, nearest, highest priority, planned soon.
- Empty state should push the core behavior: save from anywhere.
- Place rows should show type, location, source, priority, and visited/planned status.

### 3) Place Detail
Purpose: one canonical home for a place.

Needed sections:
- Hero/map preview.
- Name, type, location, distance.
- User-owned data: notes, priority, tags, source URL, saved date, visited/completed state.
- Cloud data: public place metadata, community reviews/signals, aggregate tips, external links.
- Actions: add to plan, mark visited, open in Maps, share, edit.

Key rule: local cache private/user-owned content; cloud-backed for non-user-owned shared data.

### 4) Save Place / Capture
Purpose: the fastest possible add flow.

Patterns to consider:
- One primary field: place name/link/search.
- Type and priority quick chips.
- Confirm map result before saving.
- If source is unclear, save as draft rather than blocking.
- AI import is planned for v1 after manual loop is proven.

### 5) Plan Builder
Purpose: convert saved places into a realistic adventure.

Inputs:
- Start location.
- Duration: today, half-day, full day, weekend, custom days.
- Pace: relaxed, balanced, packed.
- Must-include places.
- Place types to include/exclude.
- Budget level later.

Output:
- Itinerary cards by day.
- Map route preview.
- Warnings for unrealistic drive time, closed hours, too many hard hikes, or weak location data.

### 6) Trip Detail / Itinerary
Purpose: execution mode for a planned trip.

Patterns to consider:
- Status-card layout like Flighty: compact, glanceable, with important timing/status first.
- Day cards with route/order, drive time, activity duration, notes.
- Big action row: open route, edit plan, mark completed, export/share.
- Keep map context visible or one tap away.

Inspo: Flighty card over map. Good for a plan summary over geographic context.

### 7) Progress / Memories
Purpose: lightweight tracking, not a full social/achievement product.

Start simple:
- Visited places.
- Completed trips.
- Favorite places.
- Basic stats: places saved, places visited, trips completed, states/countries later.

Do not overbuild national park/state/country dashboards until the core loop is working.

### 8) Settings / Account
Purpose: privacy, CloudKit/SIWA, export, app preferences.

Needed:
- Sign in with Apple / iCloud status.
- Sync state.
- Privacy explanation.
- Export/import user-owned data.
- Theme if DesignKit theme picker is used.
- About/support.

## Visual and interaction ideas from current inspo

### AllTrails
Useful:
- Map-first experience with floating search and chips.
- Clear category filters.
- Bottom sheet count/summary.
- Side map controls.
- Places/trails are spatial first, list second.

TripTracker adaptation:
- Make saved places feel like a living map, not a static list.
- Use chips for quick narrowing by adventure type.
- Let map and list stay tightly connected.

### Flighty
Useful:
- Dramatic geography backdrop.
- Bottom card with high-signal summary.
- Status language that is calm and confidence-building.
- Compact actions in the top/right chrome.

TripTracker adaptation:
- Trip detail can show map/route as the emotional backdrop.
- Itinerary summary card should feel like a control tower for the trip.
- Feasibility status should be plain: realistic, tight, too much driving, missing data.

### Tesla
Useful:
- One object-focused dashboard.
- Clear status and controls.
- Dense but calm dark cards.
- Sliders/actions separated from status info.

TripTracker adaptation:
- For an active trip, one dashboard could show the current plan status: next stop, time remaining, route health, weather/closure warnings later.
- Keep actions grouped and obvious.

## Design principles

1. Map-first, but not map-only.
2. Capture must be faster than opening Notes.
3. Private user data and cloud/public data should look and behave differently.
4. Planning output must be honest about uncertainty.
5. Avoid chatbot UI as the main product surface.
6. Use AI as import/planning assistance, not as the app's identity.
7. Keep Apple-first patterns: MapKit, CloudKit, SIWA, SwiftUI, iOS 26 visual language.
8. Design for future Android via service boundaries, not by compromising v1 UI.

## Near-term design tasks

1. Build rough wireframes for five screens:
   - Explore Map
   - Saved Places
   - Place Detail
   - Save Place
   - Plan Builder / Trip Detail
2. Decide tab structure:
   - Option A: Explore, Places, Plan, Memories, Settings
   - Option B: Map, Saved, Trips, Progress, Settings
   - Recommendation for v1: Map, Saved, Plan, Memories, Settings.
3. Define the public/cloud vs private/local visual distinction.
4. Define the first empty states.
5. Add more Mobbin exports for: Google Maps saved lists, Apple Maps guides, Wanderlog/Tripsy itinerary, Beli/restaurant saved lists, Notion/Airtable database patterns.
