# Dashboard Design System

## Overview
This document defines the standard card patterns, color scheme, and component guidelines for the modernized Home Assistant dashboard.

## Card Patterns

### 1. Light Controls (Tile Cards)

**Standard Light Tile:**
```yaml
type: tile
entity: light.room_main_lights
name: Main Lights
icon: mdi:ceiling-light
vertical: false
tap_action:
  action: toggle
hold_action:
  action: more-info
```

**Light Grid Layout (2-3 columns):**
```yaml
type: grid
columns: 2  # Use 3 for rooms with many lights
square: false
cards:
  - type: tile
    entity: light.room_light_1
    name: Light 1
  - type: tile
    entity: light.room_light_2
    name: Light 2
```

**Common Light Icons:**
- `mdi:ceiling-light` - Main ceiling lights
- `mdi:ceiling-light-multiple` - Multiple ceiling lights
- `mdi:lamp` - Table/floor lamps
- `mdi:led-strip-variant` - LED strips
- `mdi:wall-sconce` - Wall sconces
- `mdi:lightbulb-night` - Night lights
- `mdi:door` - Closet/pantry lights

---

### 2. Switch Controls (Tile Cards)

**Standard Switch Tile:**
```yaml
type: tile
entity: switch.room_device
name: Device Name
icon: mdi:power
tap_action:
  action: toggle
hold_action:
  action: more-info
```

**Common Switch Icons:**
- `mdi:fan` - Fans
- `mdi:power-plug` - Outlets/plugs
- `mdi:air-filter` - Exhaust fans

---

### 3. Climate Display (Tile Cards)

**Standard Climate Grid (3 columns):**
```yaml
type: grid
columns: 3
square: false
cards:
  # Temperature
  - type: tile
    entity: sensor.room_temperature
    name: Temperature
    icon: mdi:thermometer
    color: red

  # Humidity
  - type: tile
    entity: sensor.room_humidity
    name: Humidity
    icon: mdi:water-percent
    color: blue

  # CO2
  - type: tile
    entity: sensor.room_co2
    name: CO2
    icon: mdi:molecule-co2
    color: green
```

**Climate Icons:**
- `mdi:thermometer` - Temperature
- `mdi:water-percent` - Humidity
- `mdi:molecule-co2` - CO2 levels
- `mdi:gauge` - General sensors

---

### 4. Media Controls (Mini Media Player)

**Standard Mini Media Player:**
```yaml
type: custom:mini-media-player
entity: media_player.room_tv
name: Room TV
icon: mdi:television
artwork: cover
hide:
  power_state: false
  volume: false
shortcuts:
  columns: 4
  buttons:
    - name: Netflix
      type: script
      id: script.launch_netflix_room
      icon: mdi:netflix
    - name: YouTube
      type: script
      id: script.launch_youtube_room
      icon: mdi:youtube
    - name: Disney+
      type: script
      id: script.launch_disney_room
      icon: mdi:disney
    - name: Plex
      type: script
      id: script.launch_plex_room
      icon: mdi:plex
```

**Note:** Requires scripts to launch apps via Android TV Remote integration:
```yaml
# Example script in scripts.yaml
launch_netflix_room:
  alias: Launch Netflix on Room TV
  sequence:
    - service: media_player.select_source
      target:
        entity_id: media_player.room_tv
      data:
        source: "Netflix"
```

---

### 5. Monitoring Sensors (Entities Card)

**Standard Monitoring Section:**
```yaml
type: entities
title: Monitoring
show_header_toggle: false
state_color: true
entities:
  # Motion/Presence
  - entity: binary_sensor.room_motion
    name: Motion
    secondary_info: last-changed
    icon: mdi:motion-sensor

  # Moisture sensors
  - entity: binary_sensor.room_moisture
    name: Moisture
    secondary_info: last-changed
    icon: mdi:water-alert

  # Door/Window contacts
  - entity: binary_sensor.room_door
    name: Door
    secondary_info: last-changed
    icon: mdi:door
```

**Monitoring Icons:**
- `mdi:motion-sensor` - Motion/occupancy
- `mdi:water-alert` - Moisture/leak sensors
- `mdi:door` - Door contacts
- `mdi:window-closed` - Window contacts
- `mdi:eye` - Presence sensors

---

## Room Page Structure Template

Every room page follows this structure:

```yaml
- type: sections
  title: [Room Name]
  path: [room-path]
  icon: mdi:[room-icon]
  max_columns: 3
  sections:

    # SECTION 1: LIGHTS (if multiple lights)
    - type: grid
      title: Lights
      cards:
        - type: grid
          columns: 2  # or 3 for many lights
          square: false
          cards:
            - type: tile
              entity: light.room_light_1
            - type: tile
              entity: light.room_light_2

    # SECTION 2: CLIMATE (if applicable)
    - type: grid
      title: Climate
      cards:
        - type: grid
          columns: 3
          square: false
          cards:
            - type: tile
              entity: sensor.room_temperature
            - type: tile
              entity: sensor.room_humidity
            - type: tile  # Optional CO2
              entity: sensor.room_co2

    # SECTION 3: MEDIA (if applicable)
    - type: grid
      title: Media
      cards:
        - type: custom:mini-media-player
          entity: media_player.room_tv
          shortcuts:
            columns: 4
            buttons:
              - name: Netflix
                type: script
                id: script.launch_netflix

    # SECTION 4: MONITORING
    - type: grid
      title: Monitoring
      cards:
        - type: entities
          show_header_toggle: false
          state_color: true
          entities:
            - entity: binary_sensor.room_motion
              name: Motion
              secondary_info: last-changed

  # BADGES (standardized across all rooms)
  badges:
    - type: entity
      entity: weather.forecast_home
      show_name: true
      show_state: true
      show_icon: true
      state_content:
        - state
        - temperature
        - humidity
        - uv_index
        - wind_speed
      show_entity_picture: true
    - type: entity
      entity: sensor.patio_temp_humidity_sensor_temperature
      name: Outside Temperature
      show_name: true
      show_state: true
      show_icon: true
    - type: entity
      entity: sensor.patio_temp_humidity_sensor_humidity
      name: Outside Humidity
      show_name: true
      show_state: true
      show_icon: true
```

---

## Color Scheme

### Entity State Colors (HA Default)
- **On/Active**: Blue (#5294E2)
- **Off/Inactive**: Gray (dimmed)
- **Unavailable**: Red
- **Warning**: Yellow/Amber
- **Error**: Red

### Sensor-Specific Colors
- **Temperature**: Red/Orange
- **Humidity**: Blue
- **CO2**: Green
- **Motion**: Blue (active), Gray (clear)
- **Moisture**: Red (wet), Green (dry)

---

## Typography & Spacing

### Card Titles
- Font size: 18px
- Weight: Medium (500)
- Color: Primary text color

### Entity Names
- Font size: 14px
- Weight: Regular (400)

### Sensor Values
- Font size: 14-16px
- Weight: Bold (600-700)

### Spacing
- Gap between cards: 12px (HA default)
- Gap between sections: 24px
- Card padding: 16px (HA default)
- Border radius: 12px (HA default)

---

## Icon Guidelines

### Consistency Rules
1. Use MDI (Material Design Icons) exclusively
2. Match icon to device function, not brand
3. Keep icons semantic (ceiling-light for lights, not lightbulb-outline)
4. Use `-outline` variants sparingly

### Common Icon Patterns
- **Rooms**: `mdi:bed-king`, `mdi:shower`, `mdi:countertop`, `mdi:garage`
- **Lights**: `mdi:ceiling-light`, `mdi:lamp`, `mdi:led-strip-variant`
- **Climate**: `mdi:thermometer`, `mdi:water-percent`, `mdi:molecule-co2`
- **Security**: `mdi:shield-lock`, `mdi:door-open`, `mdi:window-closed`
- **Media**: `mdi:television`, `mdi:speaker`, `mdi:cast`

---

## Home Page Status Enhancement

### Room Status Button (with state display)

**Template for Home page buttons:**
```yaml
type: custom:button-card
entity_id:
  - light.room_light_1
  - light.room_light_2
  - light.room_light_3
name: Room Name
icon: mdi:room-icon
tap_action:
  action: navigate
  navigation_path: /my-dashboard/room-path
show_state: false
custom_fields:
  status: >
    [[[
      const lights = ['light.room_light_1', 'light.room_light_2', 'light.room_light_3'];
      const on = lights.filter(e => states[e].state === 'on').length;
      if (on === 0) return 'All off';
      if (on === lights.length) return 'All on';
      return `${on} light${on === 1 ? '' : 's'} on`;
    ]]]
styles:
  card:
    - height: 120px
  custom_fields:
    status:
      - font-size: 12px
      - color: var(--secondary-text-color)
      - margin-top: 8px
```

**Note:** This requires `custom:button-card` from HACS.

**Alternative (simpler, no status):**
Keep existing tile buttons but add subtitle manually or accept no status display.

---

## Required HACS Custom Cards

1. **mini-media-player** - For modern Google TV controls
2. **button-card** (optional) - For Home page status indicators

### Installation Commands
```bash
# Via HACS UI:
# 1. Go to HACS > Frontend
# 2. Search "Mini Media Player"
# 3. Install
# 4. Restart Home Assistant
```

---

## Implementation Notes

### Testing Checklist
- [ ] All tile cards toggle correctly
- [ ] Hold action opens more-info dialog (timestamps visible)
- [ ] Climate tiles show current values
- [ ] Media player shortcuts launch apps
- [ ] Monitoring sensors show relative timestamps
- [ ] Mobile responsive (test on phone)
- [ ] Colors consistent across all rooms

### Rollout Strategy
1. Implement Kitchen & Living Room first (most complex)
2. Test thoroughly on mobile + desktop
3. Apply pattern to remaining rooms
4. Update Home page last (optional enhancement)

### Backup Reminder
Original dashboard backed up at: `my_dashbaord.yaml.backup`

---

## Version History
- v1.0 (2025-01-05): Initial design system created
