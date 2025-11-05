# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Home Assistant Dashboard Reorganization Project** - transforming a 1749-line single-view dashboard into a clean, mobile-friendly multi-page layout using Home Assistant's native `sections` type.

**Status:** Planning/Development Phase
**Current:** 17 views (mixed masonry/sections), 1749 lines
**Target:** 15 views (all sections), room-per-page organization

## Critical Context

### This is NOT Production
- **Location:** `/Users/richard/ha/repositories/dashboards/` (development workspace)
- **Production HA:** Located elsewhere - **DO NOT MODIFY**
- Changes made here will be tested and deployed to production when complete

### Project Type: Declarative YAML Configuration
- **Not a traditional codebase** - This is declarative configuration for Home Assistant Lovelace UI
- **No build system** - No compilation, bundling, or transpilation
- **No automated tests** - Manual validation and visual inspection only
- **Deployment:** Copy validated YAML to production and reload configuration

## File Organization

- **my_dashbaord.yaml** - Current dashboard (1749 lines, working copy for reorganization)
- **REQUIREMENTS.md** - Complete entity mappings and design requirements
- **IMPLEMENTATION_PLAN.md** - Phase-by-phase implementation guide with line references
- **README.md** - Project status and overview
- **CLAUDE.md** - This file

## Development Commands

### YAML Validation
```bash
# Python YAML parser (recommended)
python3 -c "import yaml; yaml.safe_load(open('my_dashbaord.yaml'))"

# Optional: yamllint (if installed)
yamllint my_dashbaord.yaml
```

### Backup & Restore
```bash
# Create backup before changes
cp my_dashbaord.yaml my_dashbaord.yaml.backup

# Restore if issues arise
cp my_dashbaord.yaml.backup my_dashbaord.yaml
```

### Home Assistant Validation (Production only)
```
1. Copy file to HA config directory
2. Settings > System > Check Configuration
3. Reload YAML (or restart Home Assistant)
4. Navigate to dashboard and verify
```

## Architecture & Design Patterns

### Dashboard Layout Pattern
**Current:** Masonry layout (Pinterest-style, poor mobile UX)
**Target:** Sections layout (responsive grid, mobile-first)

```yaml
# Target structure for ALL new views
- type: sections
  title: Page Name
  path: page-path
  icon: mdi:icon-name
  max_columns: 2-4
  sections:
    - type: grid
      title: Section Name
      cards:
        # Cards go here
  badges:
    # Standard weather + outdoor temp/humidity
```

### Home as Navigation Hub Pattern
- **Home view:** Large navigation buttons to all areas
- **Grid layout:** `grid_options` for consistent button sizing
- **Navigation action:** `action: navigate` with `navigation_path`
- **Icons:** Intuitive MDI icons for each area

### Confirmation Dialog Pattern (CRITICAL)
**REQUIRED on all door/lock/security controls:**

```yaml
tap_action:
  action: toggle
  confirmation:
    text: "Descriptive confirmation message?"
```

**Why:** User accidentally taps controls while scrolling on mobile

**Controls requiring confirmations:**
- All garage door controls
- Front door lock controls
- Manual garage override button (`input_button.garage_confirm_person_present`)
- Any security/access controls

### Card Preservation Pattern
**DO NOT rebuild these working custom cards - copy exactly:**

| Card | Lines | Reason |
|------|-------|--------|
| Washer picture-elements | 933-1029 | Google Home Max casting integration |
| Dryer picture-elements | 1030-1105 | Google Home Max casting integration |
| Fridge dashboard | 1107-1378 | Complex custom button-cards with color-coded alerts |
| Plants (5 flower cards) | 1527-1583 | Custom HACS integration, no native equivalent |
| Water softener | 1386-1520 | ApexCharts history visualization |

**Critical:** Washer/Dryer view is cast to Google Home Max via "Display Washer Dryer Cards" automation. Card structure must remain identical.

### Entity Organization Pattern

**Room-Based Grouping Rules:**
- **Kitchen & Living Room** - Combined (open floor plan) + Stairs
- **Master Suite** - Bedroom + Vanity + Bathroom (logical grouping)
- **Garage** - ONLY garage entities (no utilities)
  - `sensor.garage_dryer_plug_current` → Appliances page
  - Water softener entities → Water Softener page

**Target Structure (15 Views):**
1. Home (navigation hub)
2-9. Room pages (Kitchen/Living, Master Suite, Dima's Bedroom, Zoe's Bedroom, Upstairs Bath, Garage, Patio, Entrance)
10-14. System pages (Climate, Appliances, Plants, Water Softener, Entry & Security)

**Views to Remove:**
- `z2m` (Zigbee diagnostics) → Move to separate diagnostic dashboard
- `Cameras Native` → Will use Scrypted in Phase 2 (future)

### Badge Pattern
**Standard badges for most views:**
```yaml
badges:
  - type: entity
    entity: weather.forecast_home
    state_content: [state, temperature, humidity, uv_index, wind_speed]
  - type: entity
    entity: sensor.patio_temp_humidity_sensor_temperature
    name: Outside Temperature
  - type: entity
    entity: sensor.patio_temp_humidity_sensor_humidity
    name: Outside Humidity
```

## Home Assistant Card Types

### Native Cards (Prefer These)
- `button` - Interactive controls with tap actions
- `tile` - Entity tiles
- `entities` - Entity list displays
- `thermostat` - Climate control widgets
- `picture-entity` - Camera feeds with entity overlays
- `gauge` - Visual numeric displays
- `history-graph` - Time-series data
- `markdown` - Dynamic templated content
- `vertical-stack` / `horizontal-stack` - Layout containers
- `conditional` - Conditional rendering
- `media-control` - Media player controls

### Custom Cards (Use Only Where Necessary)
- `custom:flower-card` - Plant monitoring (no native equivalent)
- `custom:button-card` - Advanced button features (fridge/freezer status with color-coded temperature alerts)
- `custom:mushroom-template-card` - Alert cards with templated content
- `custom:apexcharts-card` - Advanced charting (water softener history)
- `custom:template-entity-row` - Custom entity row displays
- `picture-elements` - Pixel-perfect image overlays (washer/dryer state visualization)
- `custom:scrypted-nvr-camera` - Scrypted camera integration

## Integration-Specific Details

### Garage Auto-Close System
**Integration:** `auto_close_garage_door_package.yaml` (LLM Vision-based)

**Related Entities:**
- `cover.group_garage_door_opener_door` - Main garage door
- `input_button.garage_confirm_person_present` - Manual override (extends timeout to 60 min)
- `binary_sensor.group_garage_door_opener_motion` - Motion sensor
- `binary_sensor.garage_camera_smart_occupancy_sensor_occupancysensor` - AI occupancy

**Behavior:** Auto-closes garage after timeout unless people/vehicles detected. Manual override button must have confirmation dialog.

### Security System Modes
**Modes:**
- **Home Mode** - IVS monitoring only, normal daily life
- **Away Mode** - Full lockdown, all sensors monitored

**Control:** `input_select.home_security_mode`

**Face Recognition Sensors (4):**
- `binary_sensor.entrance_face_recognition_sensor`
- `binary_sensor.doorbell_face_recognition_sensor_motionsensor`
- `binary_sensor.front_full_camera_face_recognition_sensor_motionsensor`
- `binary_sensor.front_side_camera_face_recognition_sensor_motionsensor`

### LG ThinQ Smart Appliances
**Washer:** Custom picture-elements card showing wash cycle states (Detecting, Washing, Rinsing, Spinning)
**Dryer:** Custom picture-elements card showing dry cycle states (Drying, Cooling)

**Critical Integration:** "Display Washer Dryer Cards" automation casts this view to Google Home Max. DO NOT change card structure.

### Bosch Cooktop
**New to Appliances page:**
- Power state and operation state sensors
- Childproof lock switch
- Zone selector and power level controls
- MoveMode zone controls (front, middle, rear)
- Timer state

See REQUIREMENTS.md for complete entity list.

## Development Workflow

### Before Starting
1. **Read all documentation:**
   - README.md (overview)
   - REQUIREMENTS.md (complete entity mappings)
   - IMPLEMENTATION_PLAN.md (step-by-step guide with line references)
   - CLAUDE.md (this file)

2. **Create backup:**
   ```bash
   cp my_dashbaord.yaml my_dashbaord.yaml.backup
   ```

3. **Understand current structure:**
   - Current file is 1749 lines, 17 views
   - View 1 (Home): Lines 2-674 (masonry layout, all rooms mixed)
   - Views 2-17: Mixed sections/masonry layouts

### Implementation Approach
**Follow IMPLEMENTATION_PLAN.md sequentially:**

1. **Phase 1:** Create Home navigation page with large buttons
2. **Phase 2:** Create 8 room pages (copy entities from current Home view)
3. **Phase 3:** Create 5 system pages (preserve existing custom cards)
4. **Phase 4:** Remove deprecated views (z2m, Cameras Native)
5. **Phase 5:** Final cleanup
6. **Phase 6:** Testing & validation

**After each phase:**
1. Validate YAML syntax
2. Check for indentation errors (spaces only, no tabs)
3. Verify entity references
4. Test in Home Assistant if possible

### Common YAML Errors to Avoid
- Inconsistent indentation (use 2 spaces, never tabs)
- Missing colons after keys
- Incorrect list syntax (must use `- ` with space)
- Unclosed quotes
- Missing required fields (entity_id, type, etc.)

### Testing Strategy
**Manual validation only - no automated tests**

**After each phase:**
1. YAML validation (Python parser)
2. Visual inspection in Home Assistant UI
3. Entity interaction (toggle lights, verify states)
4. Confirmation dialog testing
5. Mobile layout check

**Final testing checklist:**
- [ ] All 15 views load without errors
- [ ] Home navigation buttons work
- [ ] All entity states display correctly
- [ ] Confirmations work on door/lock controls
- [ ] Custom cards render properly (flower, washer, dryer, fridge)
- [ ] Mobile layout is usable (large touch targets, no horizontal scroll)
- [ ] No broken entity references
- [ ] Washer/dryer cards still work for Google Home Max casting
- [ ] Garage auto-close manual override works with confirmation
- [ ] Security system mode controls work
- [ ] Climate controls operate HVAC

## Common Pitfalls to Avoid

### 1. Breaking Custom Card Structure
**DON'T rebuild - copy exactly:**
```yaml
# WRONG - Rebuilding washer card
- type: entities
  entities:
    - sensor.top_load_washer_run_state
```

**DO - Copy exact structure:**
```yaml
# RIGHT - Exact copy from source
- type: vertical-stack
  cards:
    - type: picture-elements
      elements:
        # ... (exact copy from lines 933-1029)
```

### 2. Forgetting Confirmations
**Every door/lock/security control must have confirmation dialog:**
```yaml
# WRONG
- type: button
  entity: cover.garage_door_opener_door
  tap_action:
    action: toggle

# RIGHT
- type: button
  entity: cover.garage_door_opener_door
  tap_action:
    action: toggle
    confirmation:
      text: "Open/close garage door?"
```

### 3. Wrong Entity Organization
**DON'T mix utilities in Garage:**
- ❌ `sensor.garage_dryer_plug_current` in Garage
- ❌ `sensor.water_softener_salt_level` in Garage

**DO organize by function:**
- ✅ Dryer current → Appliances page
- ✅ Water softener → Water Softener page
- ✅ Only garage-specific entities in Garage page

### 4. Using Wrong Layout Type
**All new views must use `sections` layout:**
```yaml
# WRONG
- path: some-page
  type: masonry  # Old style, poor mobile UX

# RIGHT
- path: some-page
  type: sections  # Modern mobile-friendly layout
```

### 5. Missing Badges
**Most views should include standard badge set:**
```yaml
badges:
  - type: entity
    entity: weather.forecast_home
    state_content: [state, temperature, humidity, uv_index, wind_speed]
  - type: entity
    entity: sensor.patio_temp_humidity_sensor_temperature
    name: Outside Temperature
  - type: entity
    entity: sensor.patio_temp_humidity_sensor_humidity
    name: Outside Humidity
```

## Quick Reference

### Navigation Icons
- Kitchen & Living Room: `mdi:countertop`
- Master Suite: `mdi:bed-king`
- Dima's Bedroom: `mdi:bed`
- Zoe's Bedroom: `mdi:bed`
- Upstairs Bathroom: `mdi:shower`
- Garage: `mdi:garage`
- Patio: `mdi:patio-heater`
- Entrance: `mdi:door`
- Climate Control: `mdi:hvac`
- Appliances: `mdi:washing-machine`
- Plants: `mdi:flower`
- Water Softener: `mdi:water`
- Entry & Security: `mdi:shield-home`

### Grid Options for Navigation Buttons
```yaml
grid_options:
  columns: 4  # Width
  rows: 3     # Height
```

### Navigation Action
```yaml
tap_action:
  action: navigate
  navigation_path: /dashboard/page-path
```

### Entity Card Best Practices
```yaml
- type: entities
  entities:
    - entity: light.some_light
      name: Custom Name
      secondary_info: last-changed
      icon: mdi:lightbulb
  title: Section Title
  state_color: true  # Color icon by state
```

## Important Reminders

- **This is a development workspace** - Not production Home Assistant
- **Plan before executing** - User prefers approval before major changes (per global CLAUDE.md)
- **Copy, don't rebuild** - Preserve working custom cards exactly as-is
- **Test incrementally** - Validate YAML after each phase, don't wait until end
- **Mobile-first** - Always consider touch targets (min 48x48px) and scrolling
- **Confirmations required** - All door/lock/security controls must have them
- **Entity organization matters** - Follow room groupings in REQUIREMENTS.md exactly
- **Report issues** - Don't hide problems, present options to user

## Success Criteria

Project is complete when:
1. ✅ All 15 views created with correct structure
2. ✅ All entities migrated correctly (see REQUIREMENTS.md)
3. ✅ No YAML syntax errors
4. ✅ All confirmations work on security controls
5. ✅ Mobile usability verified (large touch targets, minimal scrolling)
6. ✅ Custom cards preserved and rendering correctly
7. ✅ Navigation works from Home page
8. ✅ Washer/dryer Google Home Max casting still works
9. ✅ No broken entity references
10. ✅ User approves final layout

## Future Enhancements (Out of Scope)

**Phase 2 - Scrypted Cameras:**
- Replace native camera entities with Scrypted NVR cards
- Requires ID mapping from Scrypted

**Phase 3 - Diagnostic Dashboard:**
- Separate dashboard file for z2m statistics, system counters, network diagnostics

**Phase 4 - Energy Monitoring:**
- Dedicated energy page with dishwasher plug, dryer current, power trends

## Getting Help

If you encounter issues:
1. **Check YAML syntax** - Most errors are indentation/formatting
2. **Verify entity IDs** - Use Developer Tools > States in Home Assistant
3. **Check Home Assistant logs** - Logs show entity errors and warnings
4. **Review source** - Compare to working sections in `my_dashbaord.yaml`
5. **Consult documentation** - REQUIREMENTS.md and IMPLEMENTATION_PLAN.md have detailed guidance
6. **Ask user** - When unclear, ask before proceeding (user prefers this per global CLAUDE.md)
