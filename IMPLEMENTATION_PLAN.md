# Dashboard Reorganization Implementation Plan

## Overview

This plan transforms the current single-view dashboard into a clean, mobile-friendly multi-page layout. Work sequentially through each phase, testing after each major change.

## Pre-Implementation

### Backup
```bash
cp my_dashbaord.yaml my_dashbaord.yaml.backup
```

### Validation Command
After each change, validate YAML:
```bash
# Check for YAML syntax errors
yamllint my_dashbaord.yaml

# Or use a Python YAML parser
python3 -c "import yaml; yaml.safe_load(open('my_dashbaord.yaml'))"
```

## Phase 1: Create Home Navigation Page

**Goal:** Replace current masonry Home view with clean navigation hub

### Step 1.1: Create New Home View Structure

Replace lines 2-674 with:

```yaml
views:
  - path: default_view
    title: Home
    type: sections
    icon: mdi:home
    max_columns: 3
    sections:
      - type: grid
        cards:
          # Navigation buttons go here (next step)
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

### Step 1.2: Add Navigation Buttons

Create large buttons for each area:

```yaml
# In the cards section of Home view grid
- type: button
  name: Kitchen & Living Room
  icon: mdi:countertop
  tap_action:
    action: navigate
    navigation_path: /dashboard/kitchen-living
  grid_options:
    columns: 4
    rows: 3

- type: button
  name: Master Suite
  icon: mdi:bed-king
  tap_action:
    action: navigate
    navigation_path: /dashboard/master-suite
  grid_options:
    columns: 4
    rows: 3

# Continue for all 8 room pages...

- type: button
  name: Climate Control
  icon: mdi:hvac
  tap_action:
    action: navigate
    navigation_path: /dashboard/climate
  grid_options:
    columns: 4
    rows: 3

# Continue for all 5 system pages...
```

**Icons for buttons:**
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

**Test:** Verify Home view displays with navigation buttons

## Phase 2: Create Room Pages

### Step 2.1: Kitchen & Living Room + Stairs

Create new view after Home:

```yaml
- type: sections
  title: Kitchen & Living Room
  path: kitchen-living
  icon: mdi:countertop
  max_columns: 3
  sections:
    - type: grid
      title: Kitchen
      cards:
        # Copy kitchen entities from current Home view (lines 203-232)
        - type: entities
          entities:
            - entity: light.kitchen_main_lights
            - entity: light.kitchen_under_cabinet
            # ... etc
          title: Kitchen Lights & Sensors
          state_color: true

    - type: grid
      title: Living Room
      cards:
        # Copy living room entities from current Home view (lines 128-168)
        - type: entities
          entities:
            - entity: light.living_room_lamp
            # ... etc
          title: Living Room
          state_color: true

        # Copy TV controls (lines 169-178)
        - type: entities
          entities:
            - remote.living_room_tv_2
        - type: media-control
          entity: media_player.living_room_tv_2_4

    - type: grid
      title: Stairs
      cards:
        # Copy stairs entities from current Home view (lines 370-381)
        - type: entities
          entities:
            - entity: light.stairs_spot_lights
            # ... etc
          title: Stairs
          state_color: true

  badges:
    # Standard badges (weather, outside temp/humidity)
```

**Source Lines:**
- Kitchen: 203-232
- Living Room: 128-178
- Stairs: 370-381

**Test:** Navigate to Kitchen & Living Room page, verify entities

### Step 2.2: Master Suite

```yaml
- type: sections
  title: Master Suite
  path: master-suite
  icon: mdi:bed-king
  max_columns: 3
  sections:
    - type: grid
      title: Bedroom
      cards:
        # Copy master bedroom entities (lines 60-121)
        - type: entities
          entities:
            - entity: light.master_bedroom_main_lights
            # ... etc
          title: Bedroom Lights & Controls

        # Copy TV control (lines 122-123)
        - type: media-control
          entity: media_player.tv_2

    - type: grid
      title: Bathroom
      cards:
        # Copy master bathroom entities (lines 233-271)
        - type: entities
          entities:
            - entity: sensor.master_bathroom_temp_humidity_2_temperature
            # ... etc
          title: Bathroom
          state_color: true

  badges:
    # Standard badges
```

**Source Lines:**
- Bedroom: 60-123
- Bathroom: 233-271

**Test:** Navigate to Master Suite, verify entities

### Step 2.3: Dima's Bedroom

```yaml
- type: sections
  title: Dima's Bedroom
  path: dima-bedroom
  icon: mdi:bed
  max_columns: 2
  sections:
    - type: grid
      cards:
        # Copy Dima bedroom entities (lines 180-201)
        - type: entities
          entities:
            - entity: light.dima_bedroom_main_lights
            # ... etc
          title: Dima's Bedroom
          state_color: true

  badges:
    # Standard badges
```

**Source Lines:** 180-201

**Test:** Navigate to Dima's Bedroom, verify entities

### Step 2.4: Zoe's Bedroom

```yaml
- type: sections
  title: Zoe's Bedroom
  path: zoe-bedroom
  icon: mdi:bed
  max_columns: 2
  sections:
    - type: grid
      cards:
        # Copy Zoe bedroom entities (lines 272-303)
        - type: entities
          entities:
            - entity: light.zoe_bedroom_main_lights
            # ... etc
          title: Zoe's Bedroom
          state_color: true

  badges:
    # Standard badges
```

**Source Lines:** 272-303

**Test:** Navigate to Zoe's Bedroom, verify entities

### Step 2.5: Upstairs Bathroom

```yaml
- type: sections
  title: Upstairs Bathroom
  path: upstairs-bathroom
  icon: mdi:shower
  max_columns: 2
  sections:
    - type: grid
      title: Bathroom
      cards:
        # Copy upstairs bathroom entities (lines 382-412)
        - type: entities
          entities:
            - entity: sensor.upstairs_bathroom_temp_humidity_2_temperature
            # ... etc
          title: Bathroom
          state_color: true

    - type: grid
      title: Vanity
      cards:
        # Copy upstairs vanity entities (lines 414-426)
        - type: entities
          entities:
            - entity: light.upstairs_bathroom_vanity_lights
            # ... etc
          title: Vanity
          state_color: true

  badges:
    # Standard badges
```

**Source Lines:**
- Bathroom: 382-412
- Vanity: 414-426

**Test:** Navigate to Upstairs Bathroom, verify entities

### Step 2.6: Garage

```yaml
- type: sections
  title: Garage
  path: garage
  icon: mdi:garage
  max_columns: 2
  sections:
    - type: grid
      cards:
        # Copy garage entities (lines 304-333)
        # BUT EXCLUDE: sensor.garage_dryer_plug_current (moves to Appliances)
        # BUT EXCLUDE: sensor.water_softener_salt_level (moves to Water Softener)
        - type: entities
          entities:
            - entity: cover.group_garage_door_opener_door
            # ... etc (see REQUIREMENTS.md for garage entity list)
          title: Garage
          state_color: true

        # Add manual override button
        - type: button
          entity: input_button.garage_confirm_person_present
          name: Confirm Person Present
          icon: mdi:account-check
          tap_action:
            action: toggle
            confirmation:
              text: Extend garage timeout to 60 minutes?

  badges:
    # Standard badges
```

**Source Lines:** 304-333 (minus dryer current and water softener)

**Test:** Navigate to Garage, verify entities, test confirmation

### Step 2.7: Patio

```yaml
- type: sections
  title: Patio
  path: patio
  icon: mdi:patio-heater
  max_columns: 2
  sections:
    - type: grid
      cards:
        # Copy patio entities (lines 334-368)
        # Extract only patio-specific items
        - type: entities
          entities:
            - entity: switch.patio_bistro_lights_2
            - entity: switch.patio_christmas_lights_2
            - entity: light.patio_camera_floodlight_timed
            - entity: sensor.patio_temp_humidity_sensor_temperature
            - entity: sensor.patio_temp_humidity_sensor_humidity
          title: Patio
          state_color: true

  badges:
    # Standard badges
```

**Source Lines:** 334-368 (patio items only)

**Test:** Navigate to Patio, verify entities

### Step 2.8: Entrance

```yaml
- type: sections
  title: Entrance
  path: entrance
  icon: mdi:door
  max_columns: 2
  sections:
    - type: grid
      cards:
        # Copy entrance entities from lines 334-368
        # Extract entrance-specific items
        - type: entities
          entities:
            - entity: light.front_porch_sconces
            - entity: light.entrance_overhead_light
            - entity: lock.front_door_lock
            - entity: sensor.entrance_temp_humidity_temperature
            - entity: sensor.entrance_temp_humidity_humidity
            - entity: light.front_side_camera_floodlight_timed
            - entity: light.sidewalk_street_camera_illuminator
            - entity: light.front_full_camera_illuminator
          title: Entrance
          state_color: true

  badges:
    # Standard badges
```

**Source Lines:** 334-368 (entrance items only)

**Test:** Navigate to Entrance, verify entities

## Phase 3: Create System Pages

### Step 3.1: Climate Control

```yaml
- type: sections
  title: Climate Control
  path: climate
  icon: mdi:hvac
  max_columns: 2
  sections:
    - type: grid
      cards:
        # Copy thermostats from current HVAC view (lines 872-924)
        - type: thermostat
          entity: climate.home_downstairs
          features:
            - style: icons
              type: climate-hvac-modes

        - type: thermostat
          entity: climate.home_upstairs
          features:
            - style: icons
              type: climate-hvac-modes

  badges:
    # Standard badges
```

**Source Lines:** 872-924

**Test:** Navigate to Climate Control, verify thermostats

### Step 3.2: Appliances

**IMPORTANT:** Preserve exact structure of custom cards for Google Home Max casting

```yaml
- type: sections
  title: Appliances
  path: appliances
  icon: mdi:washing-machine
  max_columns: 3
  sections:
    - type: grid
      title: Washer & Dryer
      cards:
        # Copy ENTIRE washer card from lines 933-1029 (picture-elements + conditional)
        - type: vertical-stack
          cards:
            - type: picture-elements
              # ... (exact copy)
            - type: conditional
              # ... (exact copy)

        # Copy ENTIRE dryer card from lines 1030-1105 (picture-elements + conditional)
        - type: vertical-stack
          cards:
            - type: picture-elements
              # ... (exact copy)
            - type: conditional
              # ... (exact copy)

        # Add dryer current sensor from garage
        - type: entities
          entities:
            - entity: sensor.garage_dryer_plug_current
              name: Dryer Current
          title: Dryer Power

    - type: grid
      title: Refrigerator
      cards:
        # Copy ENTIRE fridge section from lines 1107-1378
        # This includes custom button-cards, gauges, history graphs, etc.
        # Keep ALL cards exactly as-is

    - type: grid
      title: Bosch Cooktop
      cards:
        - type: entities
          entities:
            - entity: sensor.bosch_cooktop_power_state
            - entity: sensor.bosch_cooktop_operation_state
            - entity: switch.bosch_cooktop_childproof_lock
            - entity: sensor.bosch_cooktop_selected_zone
            - entity: number.bosch_cooktop_selected_zone_power_level
          title: Cooktop Controls
          state_color: true

        - type: entities
          entities:
            - entity: number.bosch_cooktop_movemode_front_zone_level
              name: Front Zone
            - entity: number.bosch_cooktop_movemode_middle_zone_level
              name: Middle Zone
            - entity: number.bosch_cooktop_movemode_rear_zone_level
              name: Rear Zone
          title: Zone Power Levels

    # Future: Add dishwasher energy plug when identified

  badges:
    # Standard badges
```

**Source Lines:**
- Washer: 933-1029
- Dryer: 1030-1105
- Fridge: 1107-1378
- Dryer current: From garage entities
- Cooktop: New entities

**Test:**
- Navigate to Appliances
- Verify washer/dryer cards render correctly
- Verify fridge section works
- Test if washer/dryer still casts to Google Home Max

### Step 3.3: Plants

```yaml
- type: sections
  title: Plants
  path: plants
  icon: mdi:flower
  max_columns: 3
  sections:
    - type: grid
      cards:
        # Copy ENTIRE plants section from lines 1527-1583
        # Keep all 5 custom flower cards exactly as-is
        - type: custom:flower-card
          # ... (exact copy of each plant)
      column_span: 3

  badges:
    # Standard badges (lines 1590-1618)
```

**Source Lines:** 1527-1618

**Test:** Navigate to Plants, verify all flower cards render

### Step 3.4: Water Softener

```yaml
- type: sections
  title: Water Softener
  path: water-softener
  icon: mdi:water
  max_columns: 2
  sections:
    - type: grid
      cards:
        # Copy ENTIRE water softener section from lines 1386-1520
        # Keep all cards exactly as-is (gauge, ApexCharts, etc.)

  badges:
    # Standard badges
```

**Source Lines:** 1386-1520

**Test:** Navigate to Water Softener, verify ApexCharts renders

### Step 3.5: Entry & Security

**Goal:** Clean up existing Entry view, keep essential controls

```yaml
- type: sections
  title: Entry & Security
  path: entry-security
  icon: mdi:shield-home
  max_columns: 3
  sections:
    - type: grid
      title: Door Controls
      cards:
        # Copy door controls from current Entry view (lines 684-771)
        - show_state: true
          show_name: true
          camera_view: live
          type: picture-entity
          entity: cover.group_garage_door_opener_door
          camera_image: camera.garage_camera_hd_stream
          tap_action:
            action: toggle
            confirmation:
              text: Open/close garage door?
          name: Garage Door

        - show_state: true
          show_name: true
          camera_view: live
          type: picture-entity
          entity: lock.front_door_lock
          camera_image: camera.front_full_camera_sub_2
          tap_action:
            action: toggle
            confirmation:
              text: Lock/unlock front door?
          name: Front Door

        # Manual override button
        - type: button
          entity: input_button.garage_confirm_person_present
          name: Confirm Person Present
          icon: mdi:account-check
          tap_action:
            action: toggle
            confirmation:
              text: Extend garage timeout to 60 minutes?

    - type: grid
      title: Contact Sensors
      cards:
        # Copy contact sensors (lines 740-761)
        - type: entities
          entities:
            - entity: binary_sensor.front_door_group_contact_sensor
            - entity: binary_sensor.kitchen_door_group_contact_sensor
            - entity: binary_sensor.patio_door_contact_sensor_contact
            - entity: binary_sensor.zoe_bedroom_window_contact_sensor_contact
            - entity: binary_sensor.master_bedroom_window_contact_sensor_contact
          title: Doors & Windows
          state_color: true

    - type: grid
      title: Security System
      cards:
        # Copy security system controls (lines 774-838)
        - type: markdown
          content: |
            # Security System
            # ... (copy existing markdown)

        - type: horizontal-stack
          cards:
            # ... (copy mode buttons)

        - type: entities
          title: System Status
          # ... (copy status entities)

        - type: entities
          title: Settings
          # ... (copy settings)

  badges:
    # Copy badges from current Entry view (lines 840-867)
```

**Source Lines:** 684-871 (current Entry view)

**Test:**
- Navigate to Entry & Security
- Verify camera feeds in picture-entities
- Test confirmations on door controls
- Test security mode buttons

## Phase 4: Remove Deprecated Views

### Step 4.1: Delete Cameras Native View

Remove lines 1619-1692

**Reason:** Will use Scrypted cards in Phase 2 (future)

**Test:** Verify no broken navigation references

### Step 4.2: Delete z2m View

Remove lines 1693-1733

**Reason:** Moving to separate diagnostic dashboard (future)

**Test:** Verify no broken navigation references

### Step 4.3: Delete Scrypted View

Remove lines 1734-1751

**Reason:** Will integrate Scrypted cards into room pages in Phase 2 (future)

**Test:** Verify no broken navigation references

## Phase 5: Final Cleanup

### Step 5.1: Remove Old Home View Content

The original Home view (lines 2-674) has been replaced in Phase 1. Ensure it's completely replaced with the new navigation structure.

### Step 5.2: Remove Old HVAC View

Lines 872-924 content moved to Climate Control page in Phase 3.1. Remove the old HVAC view.

### Step 5.3: Remove Old Washer Dryer View

Lines 925-1106 content moved to Appliances page in Phase 3.2. Remove the old Washer Dryer view.

### Step 5.4: Verify View Count

Final structure should have exactly **15 views**:
1. Home (navigation)
2. Kitchen & Living Room
3. Master Suite
4. Dima's Bedroom
5. Zoe's Bedroom
6. Upstairs Bathroom
7. Garage
8. Patio
9. Entrance
10. Climate Control
11. Appliances
12. Plants
13. Water Softener
14. Entry & Security
15. (No more views after this)

## Phase 6: Testing & Validation

### Step 6.1: YAML Validation
```bash
python3 -c "import yaml; yaml.safe_load(open('my_dashbaord.yaml'))"
```

### Step 6.2: Home Assistant Configuration Check

In Home Assistant:
1. Settings > System > Check Configuration
2. Fix any errors
3. Reload YAML (or restart if needed)

### Step 6.3: Functional Testing

Test each page:

- [ ] **Home** - All navigation buttons work
- [ ] **Kitchen & Living Room** - All entities respond, TV controls work
- [ ] **Master Suite** - Bedroom and bathroom entities work, TV controls
- [ ] **Dima's Bedroom** - All entities respond
- [ ] **Zoe's Bedroom** - All entities respond
- [ ] **Upstairs Bathroom** - Bathroom and vanity entities work
- [ ] **Garage** - Door controls with confirmation, manual override button
- [ ] **Patio** - All lights and sensors work
- [ ] **Entrance** - All lights and sensors work
- [ ] **Climate Control** - Both thermostats control HVAC
- [ ] **Appliances** - Washer/dryer cards render, fridge dashboard works, cooktop controls
- [ ] **Plants** - All 5 flower cards display correctly
- [ ] **Water Softener** - Gauge and ApexCharts render
- [ ] **Entry & Security** - Camera feeds work, confirmations work, security modes change

### Step 6.4: Mobile Testing

Test on actual mobile device:
- [ ] Navigation buttons are large enough
- [ ] No excessive horizontal scrolling
- [ ] Entity cards are readable
- [ ] Confirmations appear on tap
- [ ] Pages load quickly

### Step 6.5: Integration Testing

- [ ] **Washer/Dryer Google Home Max Cast** - Verify automation still works
- [ ] **Garage Auto-Close** - Manual override button extends timeout
- [ ] **Security System** - Mode changes trigger automations
- [ ] **Door/Lock Confirmations** - Prevent accidental actions

## Rollback Plan

If issues arise:

```bash
# Restore backup
cp my_dashbaord.yaml.backup my_dashbaord.yaml

# In Home Assistant
# Settings > System > Reload YAML
```

## Post-Implementation

### Document Changes
- Update any related documentation
- Note any issues encountered
- Document any entity IDs that changed

### Monitor Logs
Watch for errors in Home Assistant logs:
```bash
# View logs
tail -f /config/home-assistant.log | grep -i error
```

### Performance Check
- Dashboard load times
- Entity state update responsiveness
- Custom card rendering speed

## Success Criteria

Implementation is complete when:

1. ✅ All 15 views created and accessible
2. ✅ All entities migrated from old views
3. ✅ No YAML errors
4. ✅ All confirmations work
5. ✅ Mobile usability verified
6. ✅ Custom cards render correctly
7. ✅ Navigation works from Home page
8. ✅ Washer/dryer Google Home Max casting still works
9. ✅ No broken entity references
10. ✅ User approves final layout
