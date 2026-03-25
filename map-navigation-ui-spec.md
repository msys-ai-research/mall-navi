# Mobile Map Navigation UI - Design Spec

Research compilation of UI component patterns for indoor mall navigation, drawn from Google Maps, Apple Maps, Mappedin, MazeMap, Material Design, and Map UI Patterns.

---

## 1. Draggable Bottom Sheet

The bottom sheet is the primary information surface during navigation. It floats above the map and supports three snap states.

### Three States

| State | Height | Content | Use Case |
|-------|--------|---------|----------|
| **Peek** | ~15-20% of viewport | Destination name, ETA, distance, drag handle | Default during active navigation |
| **Half** | ~50% of viewport | Step-by-step direction list, scrollable | User pulls up for more detail |
| **Full** | ~90-95% of viewport | Full directions, alternate routes, details | Deep inspection |

### Implementation (Web/CSS)

**Recommended approach: CSS Scroll Snap** (zero-JS animation logic)

- The host element becomes a vertical scroll container with `scroll-snap-type: y mandatory`
- Invisible 1px snap-point elements are positioned at each state height using CSS custom properties
- Browser native scroll physics handle all drag/snap behavior on the compositor thread
- A `.sheet` surface element visually moves as the container scrolls
- Snap points declared in HTML: `style="--snap: 15vh"`, `--snap: 50vh`, `--snap: 90vh`

**Key behaviors:**
- Drag handle: horizontal pill indicator (40px wide, 4px tall, centered, border-radius 2px, gray)
- Swipe-down from peek state does NOT dismiss (persistent/non-modal)
- Nested scrolling: direction list scrolls within half/full state without collapsing the sheet
- Map remains interactive behind the sheet at all times (non-modal)

**Libraries:** `pure-web-bottom-sheet` (web component, CSS scroll snap), `react-modal-sheet` (React + Motion)

### Visual Spec
```
+------------------------------------------+
|            [ --- ] drag handle            |
|  Nike Store - Floor 2                     |
|  3 min walk  ~  250m  ~  [X] End Nav     |
+------------------------------------------+
```

---

## 2. Map Overlay Controls

Floating controls sit at absolute positions over the map. They do NOT move with pan/zoom.

### Standard Layout (Right Side, Vertical Stack)

```
+------------------+
|              [C]  |   C = Compass (appears when map rotated, auto-hides)
|                   |
|              [F3] |   Floor selector (vertical pill stack)
|              [F2] |   - Active floor highlighted
|              [F1] |   - Ground floor selected by default
|                   |
|              [+]  |   Zoom in
|              [-]  |   Zoom out
|                   |
|              [O]  |   Recenter / My Location
+------------------+
```

### Positioning Rules
- **Right side** of map, vertically stacked with 8-12px gaps
- Floor selector and zoom controls on the **same side**
- Keep total height under ~40% of viewport to avoid crowding
- Controls should be **48x48dp minimum** touch targets (WCAG / Material)
- Use subtle drop shadow or background blur for visibility over any map content
- Z-index above map, below bottom sheet

---

## 3. Floor Selector

Based on Map UI Patterns (mapuipatterns.com) research and ArcGIS Indoors / Mappedin implementations.

### Design

- **Vertical pill stack** of buttons, mimicking the physical vertical arrangement of a building
- Labels: short abbreviations -- `L1`, `L2`, `L3` or `1`, `2`, `3`, or `GL`, `UG` for ground/underground
- Active floor: filled/highlighted button; inactive: outline/muted
- Max 5 visible buttons; scroll for additional floors (no up/down arrows needed)
- Position: right side of map, opposite from any other controls if possible, or grouped with zoom

### Activation Strategies

| Strategy | When to Use |
|----------|-------------|
| **Always On** | Single-building apps (our mall case) -- show immediately |
| **On Demand** | Multi-building campuses -- show when user selects/zooms into a building |

### Behavior
- Ground floor selected by default on load
- Tapping a floor button instantly switches the SVG floor plan
- During navigation across floors: auto-switch floors as user progresses, with a visual indicator like "Take elevator to Floor 2" interstitial card
- When route spans multiple floors, subtly highlight the floors involved (dot indicator or badge)

---

## 4. Destination Card (Navigation Header)

The top-anchored card shown during active navigation.

### Layout
```
+------------------------------------------+
|  [<] Navigating to                        |
|      Nike Store                           |
|      Floor 2, Near Center Court           |
|      3 min ~ 250m         [End Nav]       |
+------------------------------------------+
```

### Components
- **Back/close button** (`[<]` or `[X]`): top-left, ends navigation and returns to browse mode
- **"Navigating to" label**: small, muted text
- **Destination name**: bold, primary text (16-18sp)
- **Location hint**: floor + landmark reference (14sp, secondary color)
- **ETA + Distance**: bottom row, medium weight
- **End Navigation button**: text button or icon, top-right or inline

### Behavior
- Fixed to top of screen, does not scroll
- Height is dynamic based on content (single-line vs multi-line destination name)
- Google Nav SDK uses a primary header for the next maneuver and a secondary header for lane guidance -- for indoor nav, the primary header IS the destination card since maneuvers are simpler

---

## 5. Direction Step Cards

How walking directions are presented inside the bottom sheet (half/full state).

### Step List Design (Recommended for Indoor)

```
+------------------------------------------+
|  1. [->]  Head straight past Starbucks    |
|           120m                             |
|  2. [<-]  Turn left at Center Court       |
|           80m                              |
|  3. [/\]  Take elevator to Floor 2        |
|           ---                              |
|  4. [->]  Turn right after elevator       |
|           50m                              |
|  5. [O]   Arrive at Nike Store            |
|           On your right                    |
+------------------------------------------+
```

### Icon Set (SVG)
| Icon | Meaning |
|------|---------|
| `arrow-straight` | Continue straight |
| `arrow-left` | Turn left |
| `arrow-right` | Turn right |
| `arrow-slight-left` | Bear left |
| `arrow-slight-right` | Bear right |
| `arrow-u-turn` | U-turn |
| `stairs-up` | Take stairs up |
| `stairs-down` | Take stairs down |
| `elevator` | Take elevator |
| `escalator-up` | Take escalator up |
| `escalator-down` | Take escalator down |
| `destination-pin` | Arrival / destination |

**Icon sources:** Noun Project (743+ wayfinding icons, free SVG/PNG), Smashing Magazine wayfinding set (164 icons), Flat Icons 80-icon wayfinding set

### Step Card Behavior
- Active step is highlighted (background tint or left border accent)
- Completed steps are muted/checked
- Tapping a step centers the map on that segment
- Distance shown per segment in meters
- Floor-change steps get special visual treatment (divider line, different background, floor badge)

### Alternative: Swipeable Cards (Not Recommended)
Some apps use horizontally swipeable cards at the bottom showing one step at a time. This works for outdoor driving but is **less ideal for indoor** where routes are short and users benefit from seeing the full path overview.

---

## 6. Recenter Button

Appears when the user pans or zooms away from their current position on the route.

### Design
- **Icon**: Crosshair / target / "my location" icon (Material: `my_location`)
- **Shape**: Circular FAB, 48x48dp
- **Position**: Bottom-right, above the bottom sheet peek state (or in the right-side control stack)
- **Background**: White/surface with drop shadow
- **State**: Only visible when map is NOT centered on user position; hidden otherwise

### Behavior
- Tap: smoothly animates map back to user's current position with route visible
- Auto-hides after recentering (with ~300ms fade)
- Google Nav SDK: recenter button repositions dynamically when custom controls are added; speed limit icon hides when recenter button is showing
- If user is in "overview" mode (seeing full route), recenter returns to follow mode

---

## 7. SVG Map Interactions (Mobile Browser)

For rendering and interacting with SVG floor plans in a mobile web context.

### Two Approaches

| Approach | Pros | Cons |
|----------|------|------|
| **CSS Transform** (`transform: matrix()`) | Smooth, GPU-accelerated, works with pointer events on elements | SVG content doesn't reflow; stroke widths scale |
| **ViewBox Manipulation** | Content reflows naturally; stroke widths stay consistent | Can be janky without requestAnimationFrame; harder to animate |

### Recommended: CSS Transform via Library

**Best libraries:**
- **Panzoom** (`@panzoom/panzoom`) -- universal pan/zoom, uses pointer events, supports SVG directly, touch + pinch gestures built in
- **svg-pan-zoom** (`bumbu/svg-pan-zoom`) -- SVG-specific, supports Hammer.js for pinch/double-tap, custom event handlers

### Touch Event Handling
- Use **Pointer Events API** (unified mouse + touch) rather than separate touch/mouse handlers
- Pinch-to-zoom: track two pointer distances, calculate scale delta, apply to transform matrix
- Pan: single-pointer drag updates translate values
- Use `touch-action: none` on the SVG container to prevent browser scroll/zoom interference
- Apply transforms to a `<g>` wrapper inside the SVG, not the SVG element itself

### Performance Tips
- Use `will-change: transform` on the transform target
- Throttle transform updates to `requestAnimationFrame`
- For large floor plans: consider limiting max zoom level (4-5x) to prevent blurriness
- Min zoom should show entire floor plan with padding
- Double-tap to zoom: center on tap point, zoom to 2x

### SVG Element Interaction
- Store tappable regions (rooms, stores) as `<path>` or `<rect>` with `data-store-id` attributes
- Apply `pointer-events: all` on interactive elements
- On tap: show store info in bottom sheet peek state
- Highlight tapped store with fill color change + slight scale animation
- Route path: `<path>` with stroke, animated with `stroke-dasharray` + `stroke-dashoffset` for "drawing" effect

---

## 8. Accessibility

Based on WCAG 2.1 guidelines, MazeMap accessibility features, and Mappedin accessible navigation documentation.

### Screen Reader Support
- All map controls must have `aria-label` attributes ("Floor 1", "Zoom in", "Recenter map")
- Floor selector buttons: `role="radiogroup"` with `aria-checked` on active floor
- Bottom sheet state changes: announce via `aria-live="polite"` region
- Direction steps: semantic `<ol>` list with descriptive text (not just icons)
- Provide text-based directions as an alternative to the visual map route
- Active navigation step announced when it changes

### High Contrast Mode
- Route line: minimum 3px stroke, high-contrast color (blue #4285F4 on light map, or configurable)
- Ensure 4.5:1 contrast ratio for all text over map backgrounds
- Floor selector active state must be distinguishable by more than color alone (bold weight, border, or fill pattern)
- Store highlights on the SVG map: use both color AND pattern/border for color-blind users
- Support `prefers-contrast: more` and `prefers-color-scheme: dark` media queries

### Motor Accessibility
- All touch targets minimum 48x48dp
- Bottom sheet can be controlled via keyboard (Tab to drag handle, Enter to cycle states)
- Zoom controllable via buttons (not just pinch)
- Navigation steps tappable with generous hit areas

### Barrier-Free Routing
- Route engine should support "accessible route" option: avoids stairs, prefers elevators/ramps
- Indicate in step cards when accessible alternatives exist
- MazeMap and Mappedin both support configurable accessible routing that avoids stairs and prioritizes ramps/elevators

---

## Component Summary

| Component | Position | Z-Index | Persists During Nav |
|-----------|----------|---------|---------------------|
| SVG Floor Plan | Full screen, behind all | 0 | Yes |
| Map Overlay Controls | Right side, absolute | 10 | Yes |
| Floor Selector | Right side, mid-height | 10 | Yes |
| Destination Header Card | Top, fixed | 20 | Yes |
| Bottom Sheet | Bottom, fixed | 20 | Yes (peek minimum) |
| Recenter Button | Right side, above sheet | 15 | Conditional |
| Step Cards | Inside bottom sheet | -- | In half/full state |

---

## Sources

- [Map UI Patterns - Floor Selector](https://mapuipatterns.com/floor-selector/)
- [Map UI Patterns - Zoom Control](https://mapuipatterns.com/zoom-control/)
- [Google Navigation SDK - Android Controls](https://developers.google.com/maps/documentation/navigation/android-sdk/controls)
- [Google Maps JavaScript API - Controls](https://developers.google.com/maps/documentation/javascript/controls)
- [Native-Like Bottom Sheets on the Web (CSS Scroll Snap)](https://viliket.github.io/posts/native-like-bottom-sheets-on-the-web/)
- [pure-web-bottom-sheet (GitHub)](https://github.com/viliket/pure-web-bottom-sheet)
- [Panzoom Library (GitHub)](https://github.com/timmywil/panzoom)
- [svg-pan-zoom Library (GitHub)](https://github.com/bumbu/svg-pan-zoom)
- [Accessible Maps and Wayfinding - UVA Digital Accessibility](https://digitalaccessibility.virginia.edu/accessible-maps-and-wayfinding-essentials-inclusive-navigation)
- [MazeMap Accessibility](https://www.mazemap.com/accessibility)
- [Mappedin Accessible Indoor Navigation](https://www.mappedin.com/resources/blog/accessible-indoor-navigation-software/)
- [Mappedin Multi-Floor Navigation](https://developer.mappedin.com/enterprise/showcase/multi-floor-navigation/)
- [LogRocket - Bottom Sheet UX Design](https://blog.logrocket.com/ux-design/bottom-sheets-optimized-ux/)
- [Mobbin - Bottom Sheet Design Patterns](https://mobbin.com/glossary/bottom-sheet)
- [Smashing Magazine - Wayfinding Icon Sets (164 icons)](https://www.smashingmagazine.com/2022/01/freebie-wayfinding-icon-sets-exhibition-concert-museum/)
- [Noun Project - Wayfinding Icons](https://thenounproject.com/browse/icons/term/wayfinding/)
- [Navigine - Accessible Indoor Navigation 2026 Guide](https://navigine.com/blog/accessible-indoor-navigation-the-2026-guide-to-compliance-and-universal-design/)
