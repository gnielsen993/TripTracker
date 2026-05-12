# Adventure Tracking Add-On Plan

## Purpose

Add a lightweight tracking layer that lets users see what they have completed across places, routes, parks, states, countries, continents, hikes, roads, and trips.

This turns the app from only a planning tool into a personal adventure record.

Core idea:

> Track where you have been, what you have completed, and what is still on your adventure list.

## Tracking Philosophy

Tracking should feel motivating, not overwhelming.

The app should not become a fitness tracker or social achievement app first. Tracking should support the main product:

- Save places
- Plan adventures
- Complete adventures
- Remember adventures
- Track progress over time

## Main Tracking Categories

## 1. Roads / Routes Traveled

Users should be able to track roads, scenic routes, and completed drives.

Examples:

- Pacific Coast Highway
- Going-to-the-Sun Road
- Highway 12 in Utah
- Route 66 segments
- Beartooth Highway
- Mirror Lake Highway
- Scenic byways
- Custom road-trip routes

Capabilities:

- Mark route as completed
- Save date completed
- Add photos
- Add notes
- Add favorite stops
- Track partial completion
- Display completed roads on map
- Display planned roads differently from completed roads
- Add route rating
- Save road conditions notes
- Link route to a trip

Map display:

- Completed roads: highlighted route line
- Planned roads: dashed route line
- Bucket-list roads: muted route line
- Favorite roads: emphasized marker or label

## 2. National Parks

Track national parks visited.

Fields:

- Park name
- Country
- State/region
- Date visited
- Trip associated
- Favorite rating
- Photos
- Notes
- Must-return status
- Main activities completed
- Trails completed
- Scenic drives completed
- Stamp/passport status if desired

Progress examples:

- U.S. National Parks: 12/63
- Utah Mighty 5: 3/5
- Canada National Parks: 2/37
- World national parks visited: 18 total

Views:

- List view
- Map view
- Progress bar
- Completed/unvisited filter
- Favorites
- By state/region
- By country

## 3. States

Track U.S. states visited.

Fields:

- State name
- Visited status
- Date first visited
- Trips associated
- Favorite city/place
- Notes
- Photos
- Bucket-list places remaining
- Completion level

Completion levels:

- Not visited
- Passed through
- Visited
- Explored
- Favorite

Progress:

- States visited: 18/50
- States explored: 7/50
- States with saved pins: 31/50
- States with completed trips: 12/50

This distinction matters because “drove through Nebraska once” is different from actually exploring it.

## 4. Countries

Track countries visited.

Fields:

- Country name
- Date first visited
- Cities/regions visited
- Trips associated
- Photos
- Notes
- Favorite places
- Bucket-list places remaining
- Visa/passport notes if user wants
- Completion level

Completion levels:

- Not visited
- Layover only
- Passed through
- Visited
- Explored
- Want to return

Progress:

- Countries visited: 8/195
- Countries with saved pins: 22/195
- Countries planned: 3
- Countries revisited: 2

## 5. Continents

Track continents visited.

Fields:

- Continent
- Countries visited within continent
- Favorite trip
- Photos
- Notes
- Bucket-list countries remaining

Progress:

- Continents visited: 3/7
- North America: 3 countries visited
- Europe: 5 countries visited
- Asia: planned
- South America: bucket list

## 6. Hikes / Trails

Track completed hikes.

Fields:

- Trail name
- Distance
- Elevation gain
- Difficulty
- Date completed
- Time completed
- Photos
- Notes
- Rating
- Weather
- Who went
- Trip associated
- AllTrails/external link
- Would repeat
- Favorite view
- Completion status

Completion statuses:

- Saved
- Planned
- Attempted
- Completed
- Want to repeat
- Not worth repeating

Progress examples:

- Hikes completed: 42
- Total hiking miles: 186
- Total elevation gain: 38,500 ft
- Favorite hike: user-selected
- Hardest hike: user-selected or calculated
- Hikes planned: 14

## 7. Cities / Towns

Track cities visited.

Fields:

- City name
- Country/state
- Date first visited
- Trip associated
- Favorite food spot
- Favorite activity
- Photos
- Notes
- Would return
- Bucket-list places remaining

Progress:

- Cities visited: 57
- Cities with saved pins: 103
- Favorite cities
- Cities planned next

## 8. Restaurants / Food Places

Track restaurants, cafes, bakeries, and food spots.

Fields:

- Name
- Cuisine
- City
- Date visited
- Rating
- Favorite item
- Cost level
- Photos
- Notes
- Would return
- Source link
- Trip associated

Progress:

- Restaurants tried: 84
- Coffee shops tried: 31
- Favorite food cities
- Saved food spots remaining
- Food places near current location

## 9. Bucket-List Completion

Every saved pin should have a status:

- Want to go
- Planned
- Visited
- Skipped
- Maybe later
- Want to revisit

This lets the app track:

- Total saved places
- Places visited
- Places planned
- Places remaining
- Completion percentage
- High-priority pins completed
- Pins completed by category
- Pins completed by region

Example:

- Saved places: 312
- Visited: 47
- Planned: 13
- Remaining: 252
- High-priority completed: 9/38

## Map Tracking Features

The map should support multiple tracking layers.

Layers:

- Saved pins
- Planned pins
- Visited pins
- Favorite pins
- Completed roads
- Planned roads
- National parks visited
- States visited
- Countries visited
- Trip routes
- Hikes completed

Users should be able to toggle layers on/off.

Example toggles:

- Show only visited places
- Show only bucket-list places
- Show completed trips
- Show completed roads
- Show national parks
- Show countries visited
- Show upcoming plans

## Progress Dashboard

Add a dashboard that summarizes adventure progress.

Possible widgets:

- States visited: 18/50
- Countries visited: 8/195
- Continents visited: 3/7
- U.S. National Parks visited: 12/63
- Hikes completed: 42
- Restaurants tried: 84
- Roads completed: 9
- Trips taken this year: 5
- Bucket-list pins completed: 47/312
- Favorite trip this year
- Next planned adventure

## Personal Stats

Optional stats:

- Total trips
- Total day trips
- Total overnight trips
- Total road trips
- Total hiking miles
- Total driving miles tracked
- Total elevation gain
- Favorite state
- Favorite country
- Most visited region
- Most saved category
- Most completed category
- Average trip length
- Trips by year
- Adventures by month

## Achievement System

Achievements should be optional and subtle.

Examples:

- First saved pin
- First completed hike
- First completed trip
- First national park
- 5 national parks visited
- 10 states visited
- 5 countries visited
- First international trip
- First road trip route completed
- 100 saved places
- 25 bucket-list places completed
- First repeat-worthy place

Avoid making it too gimmicky. The app should feel premium, not like a mobile game.

## Completion Levels

Some tracked items should support nuance.

For states/countries/cities:

- Not visited
- Passed through
- Visited briefly
- Explored
- Favorite
- Want to return

For hikes/roads:

- Saved
- Planned
- Attempted
- Partially completed
- Completed
- Want to repeat

This makes tracking more accurate and personal.

## Trip-Based Auto Tracking

When a user completes
