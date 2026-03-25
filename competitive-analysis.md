# Competitive Analysis: Mall Navigation Apps & Indoor Wayfinding

## 1. Westfield (Westfield Plus App)

**Platform:** iOS, Android, Kiosk (100+ terminals)
**Wayfinding Provider:** 22Miles

### Navigation Features
- Interactive digital maps with step-by-step directions to stores, restrooms, parking
- 100+ customizable kiosks integrated with mapping software
- Parking integration: book, pay, and locate your car within the app
- 45% of customers use parking + wayfinding features

### Unique/Innovative
- **AR Wayfinding Trial (M&S Food Hall):** Users enter a product name and follow an on-screen AR path to the shelf. When phone is held up, AR markers appear; when held down, a compass directs the user.
- Custom icon and illustration language designed for intuitive navigation
- Valet services and parking timer notifications integrated into navigation flow

### Key Takeaway
Westfield treats navigation as part of a broader "mall experience" platform — parking, valet, wayfinding, and services are unified. The AR grocery wayfinding is forward-thinking.

---

## 2. Simon Malls (SIMON App)

**Platform:** iOS, Android
**Wayfinding Provider:** Mappedin

### Navigation Features
- **200 interactive 3D mall maps** across all Simon properties
- **Blue-dot navigation** (real-time position tracking) in select centers
- Search for stores, categories, and brands — works across all 200 centers or within a specific mall
- **Best entrance recommendation** — tells you which entrance to use (and where to park) based on your destination
- Mini-maps embedded on store detail pages showing surrounding stores
- Tap any store to get directions to/from another location

### Floor Transitions
- Accessibility mode: routes prioritize elevators and ramps over stairs/escalators

### Unique/Innovative
- **Parking pin:** Drop a pin on the map where you parked to find your car later
- **Entrance optimization:** App suggests the best entrance based on destination — reduces walking time
- Mini-maps on store pages provide instant spatial context without opening full map

### Key Takeaway
Simon's app excels at reducing friction: entrance suggestions, parking pins, and embedded mini-maps all minimize the cognitive load of navigating a large mall. The 200-property scale is impressive.

---

## 3. Mappedin (SDK/Platform)

**Platform:** Web, Mobile SDK, Kiosk
**Used by:** Simon, and 1000+ venues

### Navigation Features
- **Blue-dot indoor positioning** — infrastructure-free (visual positioning + Wi-Fi + device sensors)
- **Turn-by-turn text directions** — e.g., "Turn right and go 10 meters"
- Directions shown as list overview OR current-step-only with map showing next leg
- **Animated path drawing** on the map between origin and destination
- Returns: path as array of nodes, total walking distance in meters, list of text instructions

### Floor Transitions
- **Interactive tooltip with connection icon** (elevator/stairs/escalator) appears when route crosses floors
- Tapping the tooltip switches the map view to the destination floor
- Auto-detects current floor and can auto-switch the displayed floor

### Multi-Building Support
- Routes can span multiple buildings and outdoor spaces
- Handles complex venue topologies

### Unique/Innovative
- **Product-level search:** Find which stores carry a specific item
- Real-time analytics on shopper search patterns and routes
- Infrastructure-free positioning (no beacons needed)
- Custom branding per venue

### Key Takeaway
Mappedin's floor transition pattern (interactive tooltip with icon at the transition point) is elegant and widely adopted. Their turn-by-turn system providing both overview and step-by-step modes is a strong pattern to follow.

---

## 4. MazeMap

**Platform:** Web, iOS, Android
**Primary Market:** Campuses, hospitals, workplaces (but applicable patterns)

### Navigation Features
- Search for rooms, departments, or points of interest
- Click destination on map OR search to get directions
- **Flexible A-B-C routing** — plan routes with multiple stops
- **Blue-dot positioning** with seamless outdoor-to-indoor transition
- Blue dot updates in real-time as user moves

### Floor Transitions
- Uses WiFi, GPS, beacons, and magnetic fields for floor detection
- Automatic floor switching as user moves between levels

### Accessibility
- Color supported by icons and labels (not color-dependent)
- All text/UI legible at 200% zoom
- Full keyboard navigation for all actions
- **Turn-by-turn audio instructions** for visually impaired users

### Unique/Innovative
- **Shareable directions:** Share button generates URL + QR code for any route
- QR codes can integrate with appointment systems and timetables
- Multi-stop routing (A-B-C) is rare in mall apps but very useful

### Key Takeaway
MazeMap's accessibility features set the gold standard. Shareable route URLs/QR codes and multi-stop routing are features mall apps should adopt. The audio turn-by-turn for visually impaired users is essential.

---

## 5. Google Maps Indoor

**Platform:** iOS, Android, Web
**Coverage:** 10,000+ indoor venues globally

### Navigation Features
- Floor plans visible when zooming in to supported venues
- Floor/level selector in bottom-left corner
- Points of interest labels (stores, gates, ATMs, restrooms)
- Turn-by-turn indoor directions: search indoor location > tap Directions > tap walking icon

### Floor Transitions
- Simple floor picker (level selector dropdown)
- No animated transitions or visual route continuity between floors

### Limitations
- GPS-dependent — accuracy degrades significantly indoors
- Floor plans submitted by venue owners — coverage is inconsistent
- No blue-dot accuracy comparable to dedicated indoor solutions
- No product-level or category search

### Key Takeaway
Google Maps provides the baseline mental model that users already understand (blue dot, floor picker, directions). Our app should feel familiar to Google Maps users but exceed it in indoor accuracy and mall-specific features.

---

## 6. Asian Mall Apps

### Duon Wayfinding (Philippines)
**Platform:** Android (iOS coming), Free
**Tagline:** "Waze for malls"

**Navigation Features:**
- Input destination in search bar; Duon auto-detects your position
- Shows distance in both **minutes and meters**
- 3D map with pinch-to-zoom
- Store categories: cinema, fast food, supermarket, etc.
- Each store shows the floor it's on
- **Offline navigation** — works after initial data download

**Unique/Innovative:**
- Building switcher — select different buildings within the app
- Category-first browsing (not just search)
- Offline-capable navigation

### InMapz (Multi-Country)
**Coverage:** Philippines (SM Megamall), Vietnam (Vincom, AEON), Indonesia, Hong Kong, Bangkok

**Key Feature:** Cross-country mall coverage with consistent interface

### Key Takeaway
Duon's "Waze for malls" positioning resonates with users. Showing distance in both time AND meters is a smart dual-metric approach. Offline capability is essential for malls with poor indoor signal. Category-first browsing matches how many shoppers think ("I want food" not "I want store #247").

---

## 7. SMAPS (UX Case Study)

**Designer:** V. Chen — detailed UX study of ideal mall navigation

### UI Components (4 main elements)
1. **Search Bar** — search by store name, product, or food item; includes microphone icon for voice input (Siri/Google Assistant)
2. **Floor Bar** — left side; select individual floors OR tap "All" for 3D exploded view of entire mall
3. **2D/3D Toggle** — circular buttons on right side to switch between floor plan and perspective views
4. **Map Options** — toggle overlays: discount indicators, price ranges, ratings

### Active Navigation State
- Clear visible route line with **progress bar on top** of screen
- Users choose floor-transition method: elevator, escalator, or stairs
- Auto-switches to **3D view** when route spans multiple floors
- **Auto floor detection** — map updates as user changes floors

### Unique/Innovative
- **Product/food search** — search "sushi" and see all locations that serve it
- **Discount overlay** — toggle to see which stores have active deals
- **3D exploded view** — see all floors simultaneously to understand mall layout
- Floor transition method choice (elevator vs. stairs vs. escalator)

### Key Takeaway
SMAPS represents the "ideal" mall navigation UX. The progress bar during navigation, user choice of transition method, and the 3D exploded view are all features worth adopting.

---

## 8. Battersea Power Station App

**Platform:** 40+ kiosk terminals
**Developer:** Gearheart

### Navigation Features
- Graph-based pathfinding algorithm across 5 levels + exterior
- Category browsing with level filtering
- Smart keyboard with autocomplete (only available characters shown)
- Navigation pop-ups for switching route segments between floors

### Floor Transitions
- Level switcher control
- Navigation pop-ups help user move forward/backward through route segments
- Handles edge cases: e.g., elevator only goes to penultimate floor, algorithm finds intermediate route
- Accessible routes: avoids stairs, escalators, steps

### Key Takeaway
The navigation pop-ups for floor-by-floor route segments and smart handling of partial elevator coverage are sophisticated patterns. The autocomplete keyboard that only shows available characters is a nice search UX touch.

---

# Key Patterns & What MallNav Should Adopt

## MUST-HAVE Features (Table Stakes)

| Feature | Who Does It Best | Priority |
|---------|-----------------|----------|
| Interactive 2D map with store labels | All | P0 |
| Search by store name | All | P0 |
| Route drawing (path line on map) | Mappedin, Simon | P0 |
| Floor selector/switcher | All | P0 |
| Blue-dot current position | Mappedin, MazeMap | P0 |
| Turn-by-turn text directions | Mappedin, MazeMap | P0 |
| Distance display (meters + minutes) | Duon | P0 |

## SHOULD-HAVE Features (Differentiators)

| Feature | Who Does It Best | Priority |
|---------|-----------------|----------|
| Floor transition icons (elevator/stairs/escalator) at transition points | Mappedin | P1 |
| User choice of transition method (elevator vs. stairs) | SMAPS | P1 |
| Accessibility mode (elevator-priority routing) | Simon, MazeMap | P1 |
| Progress bar during active navigation | SMAPS | P1 |
| Category browsing (food, fashion, etc.) | Duon, Battersea | P1 |
| Parking pin / car finder | Simon | P1 |
| Best entrance recommendation | Simon | P1 |
| Mini-map on store detail pages | Simon | P1 |

## NICE-TO-HAVE Features (Innovation Layer)

| Feature | Who Does It Best | Priority |
|---------|-----------------|----------|
| 3D exploded view (all floors visible) | SMAPS | P2 |
| Product/item-level search ("sushi", "shoes") | SMAPS, Mappedin | P2 |
| AR overlay navigation | Westfield/M&S | P2 |
| Shareable route URL/QR code | MazeMap | P2 |
| Multi-stop routing (A > B > C) | MazeMap | P2 |
| Discount/deal overlay on map | SMAPS | P2 |
| Offline navigation | Duon | P2 |
| Voice search | SMAPS | P2 |

## Critical UI Design Patterns to Follow

### 1. Navigation Screen Layout
- **Top:** Search bar (always accessible)
- **Left side:** Floor selector (vertical bar with floor numbers)
- **Right side:** 2D/3D toggle, zoom controls, "locate me" button
- **Bottom:** Destination card / turn-by-turn instruction panel
- **On map:** Route line, blue dot, store labels, transition point icons

### 2. Active Navigation State
- Route line highlighted on map (animated or pulsing recommended)
- Current instruction displayed prominently (bottom sheet or top banner)
- Progress indicator showing how far along the route
- Next floor transition shown with icon (elevator/stairs) at the transition point
- Tapping transition icon switches map to next floor

### 3. Floor Transition Pattern (Mappedin Model)
1. Route line leads to transition point (elevator/escalator/stairs)
2. Interactive tooltip appears with icon indicating transition type
3. User taps tooltip -> map animates to destination floor
4. Route continues from transition point on new floor
5. User can navigate back to previous floor segment

### 4. Search & Discovery
- Support both search-first AND browse-first (categories) flows
- Show results with floor number and distance
- Tapping result centers map on location with option to "Get Directions"

### 5. Accessibility (Non-Negotiable)
- Accessible routing toggle (elevators + ramps only)
- Icons + labels (never color-alone for information)
- Screen reader compatible turn-by-turn
- Sufficient contrast and zoom support
