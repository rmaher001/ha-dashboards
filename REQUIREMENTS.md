# Dashboard Reorganization Requirements

## Current State Analysis

### Source File
- **File:** `my_dashbaord.yaml`
- **Lines:** 1749
- **Current Views:** 17

### Current View Structure

1. **Home** (masonry) - Lines 2-674
   - Garage door and front door controls with confirmations
   - Master bedroom entities
   - Living room entities
   - Dima bedroom entities
   - Kitchen entities
   - Master bathroom entities
   - Zoe bedroom entities
   - Garage entities
   - Patio and entrance entities
   - Stairs entities
   - Upstairs bathroom entities
   - Upstairs vanity entities
   - Washer entities
   - Thermostats (2)
   - Dryer entities
   - Plants (2 flower cards)
   - Counters (domain statistics)
   - CO2 history graph
   - Low battery button

2. **Entry** (sections) - Lines 675-871
   - Garage door picture-entity with camera (with confirmation)
   - Front door picture-entity with camera (with confirmation)
   - Garage door button (with confirmation)
   - Front door button (with confirmation)
   - Door/window contact sensors
   - Manual person present button for garage (with confirmation)
   - Security system controls

3. **HVAC** (sections) - Lines 872-924
   - Downstairs thermostat
   - Upstairs thermostat

4. **Washer Dryer** (sections) - Lines 925-1106
   - Custom washer card with state visualization
   - Custom dryer card with state visualization

5. **Fridge** (sections) - Lines 1107-1378
   - Custom button cards for fridge/freezer status
   - Temperature gauges
   - Humidity monitoring
   - History graphs
   - Conditional alerts
   - Battery sensors
   - Automation status

6. **Water Softener** (sections) - Lines 1379-1520
   - Salt level status and gauge
   - History chart (ApexCharts)
   - System info
   - Alert controls

7. **Plants** (sections) - Lines 1521-1618
   - 5 custom flower cards

8. **Cameras Native** (sections) - Lines 1619-1692
   - 11 native camera feeds

9. **z2m** (sections) - Lines 1693-1733
   - Zigbee network statistics
   - Device error monitoring

10. **Scrypted** (sections) - Lines 1734-1751
    - Scrypted camera card (ID 70)

## Desired State

### Target Structure (15 Views)

#### Navigation
1. **Home** - Navigation hub with large room/area buttons

#### Room Pages (8 pages)
2. **Kitchen & Living Room** - Combined open space + stairs
3. **Master Suite** - Bedroom + vanity + bathroom combined
4. **Dima's Bedroom**
5. **Zoe's Bedroom**
6. **Upstairs Bathroom**
7. **Garage** - Only garage controls (no utilities)
8. **Patio**
9. **Entrance**

#### System Pages (5 pages)
10. **Climate Control** - All thermostats, humidifier, HVAC
11. **Appliances** - Fridge, Washer, Dryer, Bosch Cooktop, Dishwasher
12. **Plants** - Keep existing
13. **Water Softener** - Keep existing
14. **Entry & Security** - Cleanup, keep essential

#### Removed
- **z2m** (move to separate diagnostic dashboard)
- **Cameras Native** (will use Scrypted in Phase 2)

## Entity Mappings by Room

### Kitchen & Living Room + Stairs

**Kitchen Entities:**
- `light.kitchen_main_lights`
- `light.kitchen_under_cabinet`
- `light.kitchen_pantry`
- `binary_sensor.hue_motion_01_motion`
- `sensor.hue_motion_01_temperature`
- `binary_sensor.kitchen_sink_moisture_sensor_water_leak`
- `binary_sensor.kitchen_dishwasher_moisture_sensor_water_leak`
- `binary_sensor.kitchen_refrigerator_moisture_sensor_water_leak`
- `binary_sensor.kitchen_door_contact_sensor_contact`

**Living Room Entities:**
- `light.living_room_lamp`
- `light.living_room_spot_lights`
- `light.living_room_presence_rgb_light`
- `binary_sensor.living_room_presence_sensors`
- `sensor.filtered_living_room_illuminance`
- `sensor.living_room_presence_2_dps310_temperature`
- `sensor.living_room_presence_2_co2`
- `light.living_room_christmas_tree_outlet`
- `light.front_door_area`
- `input_datetime.livingroom_kitchen_button_pressed_timestamp` (timer display)
- `binary_sensor.patio_door_contact_sensor_contact`
- `remote.living_room_tv_2`
- `media_player.living_room_tv_2_4`

**Stairs Entities:**
- `light.stairs_spot_lights`
- `light.stairs_led_strip_2`
- `binary_sensor.stairs_motion_sensors`

### Master Suite (Bedroom + Vanity + Bathroom)

**Master Bedroom Entities:**
- `light.master_bedroom_main_lights`
- `light.master_bedroom_bedside_lamps`
- `light.master_bedroom_olesia_lamp`
- `light.master_bedroom_richard_lamp`
- `light.master_bedroom_floor_lamp`
- `light.master_bedroom_under_bed_led_strip`
- `light.dlight_01_slider`
- `light.master_bedroom_closet_area`
- `light.master_bedroom_vanity`
- `light.master_bedroom_vanity_plus_closet`
- `remote.tv`
- `media_player.tv_2`
- `sensor.master_bedroom_illuminance_moving_average`
- `binary_sensor.master_bedroom_vanity_moisture_sensor_water_leak`
- `binary_sensor.everything_presence_lite_5cd9e4_zone_1_occupancy`
- `binary_sensor.master_bedroom_presence_sensors`
- `sensor.everything_presence_lite_5cd9e4_co2`
- `sensor.master_bedroom_temp_humidity_temperature`
- `sensor.master_bedroom_temp_humidity_humidity`
- `binary_sensor.master_bedroom_window_contact_sensor_contact`

**Master Bathroom Entities:**
- `sensor.master_bathroom_temp_humidity_2_temperature`
- `sensor.master_bathroom_temp_humidity_2_humidity`
- `sensor.master_bathroom_derivative_humidity`
- `binary_sensor.master_bathroom_humidity_target`
- `sensor.master_bathroom_presence_co2`
- `switch.master_bathroom_main_lights`
- `light.master_bathroom_presence_rgb_light`
- `switch.master_bathroom_exhaust_fan`
- `timer.master_bathroom_fan_timer`
- `binary_sensor.master_bathroom_moisture_sensor_water_leak`
- `binary_sensor.master_bathroom_motion_sensors`

### Dima's Bedroom

- `light.dima_bedroom_main_lights`
- `binary_sensor.dima_bedroom_presence_radar_target`
- `sensor.dima_bedroom_presence_co2`
- `sensor.dima_bedroom_presence_ltr390_light`
- `sensor.dima_bedroom_presence_dps310_temperature`
- `timer.dima_bedroom_lights_timer`

### Zoe's Bedroom

- `light.zoe_bedroom_main_lights`
- `light.christmas_tree_zoe_outlet`
- `binary_sensor.zoe_bedroom_presence_sensors`
- `sensor.zoe_bedroom_presence_2_co2`
- `sensor.zoe_bedroom_temp_humidity_temperature`
- `sensor.zoe_bedroom_temp_humidity_humidity`
- `sensor.zoe_bedroom_presence_2_ltr390_light`
- `timer.zoe_bedroom_lights_timer`
- `binary_sensor.zoe_bedroom_window_contact_sensor_contact`

### Upstairs Bathroom

- `sensor.upstairs_bathroom_temp_humidity_2_temperature`
- `sensor.upstairs_bathroom_temp_humidity_2_humidity`
- `binary_sensor.upstairs_bathroom_humidity_target`
- `sensor.upstairs_bathroom_derivative_humidity`
- `light.upstairs_bathroom_main_lights`
- `fan.upstairs_bathroom_exhaust_fan`
- `binary_sensor.upstairs_bathroom_motion_sensors`
- `timer.upstairs_bathroom_fan_timer`
- `binary_sensor.upstairs_bathroom_moisture_sensor_water_leak`
- `light.upstairs_bathroom_vanity_lights`
- `binary_sensor.upstairs_vanity_motion_occupancy`
- `binary_sensor.upstairs_vanity_moisture_sensor_water_leak`

### Garage

- `cover.group_garage_door_opener_door`
- `cover.ratgdov25i_78fbe5_door`
- `binary_sensor.group_garage_door_opener_motion`
- `binary_sensor.garage_camera_smart_occupancy_sensor_occupancysensor`
- `light.group_garage_door_opener_light`
- `light.garage_wall_lights`
- `light.meross_smart_switch_3` (Shop Light)
- `light.garage_door_christmas_lights`
- `binary_sensor.garage_water_heater_moisture_sensor_water_leak`
- `input_button.garage_confirm_person_present`

**Note:** Dryer current and water softener moved to Appliances and Water Softener pages respectively.

### Patio

- `light.front_porch_sconces`
- `light.entrance_overhead_light`
- `switch.patio_bistro_lights_2`
- `switch.patio_christmas_lights_2`
- `light.front_side_camera_floodlight_timed`
- `light.patio_camera_floodlight_timed`
- `sensor.patio_temp_humidity_sensor_temperature`
- `sensor.patio_temp_humidity_sensor_humidity`

### Entrance

- `lock.front_door_lock`
- `binary_sensor.front_door_lock_low_battery_level`
- `sensor.front_door_lock_battery_level`
- `sensor.entrance_temp_humidity_temperature`
- `sensor.entrance_temp_humidity_humidity`
- `light.sidewalk_street_camera_illuminator`
- `light.front_full_camera_illuminator`
- `sensor.doorbell_cpu_usage`

## System Pages Entity Mappings

### Climate Control

**Thermostats:**
- `climate.home_downstairs`
- `climate.home_upstairs`

**Additional (if available):**
- Humidifier controls
- HVAC mode controls
- Temperature sensors from each room

### Appliances

**Fridge (keep existing custom cards):**
- All fridge entities from lines 1107-1378

**Washer (keep existing custom cards):**
- All washer entities from lines 933-1029

**Dryer (keep existing custom cards):**
- All dryer entities from lines 1030-1105
- `sensor.garage_dryer_plug_current`

**Bosch Cooktop:**
- `sensor.bosch_cooktop_power_state`
- `sensor.bosch_cooktop_operation_state`
- `switch.bosch_cooktop_childproof_lock`
- `binary_sensor.bosch_cooktop_connection_state`
- `sensor.bosch_cooktop_selected_zone`
- `number.bosch_cooktop_selected_zone_power_level`
- `number.bosch_cooktop_movemode_front_zone_level`
- `number.bosch_cooktop_movemode_middle_zone_level`
- `number.bosch_cooktop_movemode_rear_zone_level`
- `sensor.bosch_cooktop_timer_state`
- `sensor.bosch_cooktop_active_zone`
- Additional cooktop entities as needed

**Dishwasher:**
- Energy plug entity (to be identified)

### Plants

Keep existing from lines 1521-1618:
- `plant.satsuma_mandarin_2`
- `plant.dracaena_fragrans_massangeana`
- `plant.aspidistra_elatior`
- `plant.dracaena_fragrans`
- `plant.schefflera_arboricola`

### Water Softener

Keep existing from lines 1379-1520:
- `sensor.water_softener_salt_status`
- `sensor.water_softener_salt_level`
- `binary_sensor.water_softener_sensor_out_of_range`
- `sensor.water_softener_firmware_version`
- `input_datetime.water_softener_last_refill`
- `input_datetime.water_softener_last_notification`
- `input_boolean.disable_leak_alerts`

### Entry & Security

**Door/Lock Controls:**
- `cover.group_garage_door_opener_door` (with camera and confirmation)
- `lock.front_door_lock` (with camera and confirmation)
- `input_button.garage_confirm_person_present` (with confirmation)

**Contact Sensors:**
- `binary_sensor.front_door_group_contact_sensor`
- `binary_sensor.kitchen_door_group_contact_sensor`
- `binary_sensor.patio_door_contact_sensor_contact`
- `binary_sensor.zoe_bedroom_window_contact_sensor_contact`
- `binary_sensor.master_bedroom_window_contact_sensor_contact`

**Security System:**
- `input_select.home_security_mode`
- `input_datetime.home_security_last_analysis`
- `input_boolean.home_security_audio_alerts`
- `input_number.home_security_tts_volume`
- `input_number.home_security_confidence_threshold`
- `input_number.home_security_warning_rate_limit`

**Face Recognition Sensors:**
- `binary_sensor.entrance_face_recognition_sensor`
- `binary_sensor.doorbell_face_recognition_sensor_motionsensor`
- `binary_sensor.front_full_camera_face_recognition_sensor_motionsensor`
- `binary_sensor.front_side_camera_face_recognition_sensor_motionsensor`

## Design Requirements

### Layout

- **Type:** `sections` for all views (except Home navigation which uses buttons)
- **Max Columns:** Varies by page (2-4 typical)
- **Mobile-First:** Large touch targets, minimal scrolling per page
- **Grid Options:** Use for precise card placement when needed

### Components

**Native Components (Preferred):**
- `button` - For controls and navigation
- `tile` - For entity tiles
- `entities` - For lists
- `thermostat` - For climate controls
- `gauge` - For numeric displays
- `history-graph` - For trends
- `picture-entity` - For camera + entity status
- `horizontal-stack` / `vertical-stack` - For layouts
- `markdown` - For dynamic content

**Custom Components (Only Where Necessary):**
- `custom:flower-card` - Plants (no native equivalent)
- `custom:button-card` - Fridge/Freezer status (existing, keep)
- `custom:mushroom-template-card` - Alerts (existing, keep)
- `custom:apexcharts-card` - Water softener history (existing, keep)
- `custom:template-entity-row` - Timer display (existing, keep)
- `picture-elements` - Washer/dryer visualization (existing, keep)

### Confirmations

**REQUIRED on all door/lock controls:**
```yaml
tap_action:
  action: toggle
  confirmation:
    text: "Open/close garage door?"
```

**Current Confirmed Controls:**
- Garage door (all instances)
- Front door lock (all instances)
- Garage confirm person present button

### Badges

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

## Implementation Constraints

1. **Preserve Existing Custom Cards** - Don't rebuild washer/dryer/fridge cards, copy them
2. **Native Components First** - Only use custom if no native alternative
3. **Mobile Touch Targets** - Minimum 48x48px tap targets
4. **Confirmation Dialogs** - All door/lock/security controls must have confirmations
5. **Icon Consistency** - Use MDI icons consistently
6. **State Colors** - Enable `state_color: true` where appropriate
7. **Secondary Info** - Use `secondary_info: last-changed` for entity cards

## Special Considerations

### Washer/Dryer Google Home Max Casting

The washer/dryer view is currently cast to Google Home Max display via the "Display Washer Dryer Cards" automation. The Appliances page should preserve the exact same card structure for this view.

### Garage Auto-Close Integration

The garage controls integrate with the auto-close automation package:
- `cover.group_garage_door_opener_door`
- `input_button.garage_confirm_person_present`
- `binary_sensor.group_garage_door_opener_motion`

All garage door controls must have confirmation dialogs.

### Security System Modes

The Entry & Security page controls the security system modes:
- Home Mode (IVS monitoring only)
- Away Mode (Full lockdown)

## Future Enhancements (Not in Current Scope)

### Phase 2 - Scrypted Camera Integration
Replace camera entities with Scrypted NVR cards:
- `custom:scrypted-nvr-camera`
- ID mapping from Scrypted

### Phase 3 - Diagnostic Dashboard
Separate dashboard file for:
- z2m statistics
- Zigbee network health
- Domain counters
- System diagnostics

### Phase 4 - Energy Monitoring
Dedicated energy page:
- Dishwasher energy plug
- Dryer current monitoring
- Power usage trends
- Solar/grid integration (if available)

## Testing Checklist

After implementation, verify:

- [ ] All 15 views load without errors
- [ ] Home navigation buttons work
- [ ] All entity states display correctly
- [ ] Confirmations work on door/lock controls
- [ ] Custom cards render properly (flower, washer, dryer, fridge)
- [ ] Mobile layout is usable (test on phone)
- [ ] Badges appear correctly
- [ ] No broken entity references
- [ ] Security system controls work
- [ ] Climate controls work
- [ ] History graphs display
- [ ] Washer/dryer cards still work for Google Home Max casting
- [ ] Garage auto-close button works with confirmation
