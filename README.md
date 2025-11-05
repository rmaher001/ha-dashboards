# Home Assistant Dashboard Reorganization

**Status:** Planning Phase
**Target:** Modern, mobile-friendly dashboard with room-per-page layout

## Project Overview

This project reorganizes the main Home Assistant dashboard from a single long view to a clean, mobile-friendly multi-page layout using Home Assistant's native `sections` type.

## Current State

- **Source File:** `my_dashbaord.yaml` (1749 lines)
- **Current Views:** 17 views (mixed masonry and sections layouts)
- **Issues:**
  - Long Home view with all rooms mixed together
  - Hard to find specific controls on mobile
  - Lots of scrolling required
  - Mixed content types (rooms, diagnostics, utilities)

## Desired State

- **Target Views:** 15 views total
  - 1 Home navigation page (large buttons for each area)
  - 8 Room pages (one per room/area)
  - 5 System pages (Climate, Appliances, Plants, Water Softener, Entry & Security)
  - Remove: z2m, Cameras Native

- **Design Goals:**
  - Mobile-first design with large touch targets
  - Room-per-page organization
  - Clean, modern sections layout
  - Native components only (no custom unless necessary)
  - Easy navigation from Home page

## Room Pages

1. **Kitchen & Living Room** - Combined open space + stairs
2. **Master Suite** - Bedroom + vanity + bathroom combined
3. **Dima's Bedroom**
4. **Zoe's Bedroom**
5. **Upstairs Bathroom**
6. **Garage** - Only garage controls (no utilities)
7. **Patio**
8. **Entrance**

## System Pages

9. **Climate Control** - All thermostats, humidifier, HVAC
10. **Appliances** - Fridge, Washer, Dryer, Bosch Cooktop, Dishwasher (energy plug)
11. **Plants** - Keep existing
12. **Water Softener** - Keep existing
13. **Entry & Security** - Cleanup, keep essential only

## Future Enhancements (TODOs)

- **Phase 2:** Replace camera entities with Scrypted cards
- **Diagnostic Dashboard:** Separate file for z2m, system counters, network stats
- **Energy Monitoring Page:** Dishwasher plug, dryer current, power monitoring

## Implementation Approach

See `IMPLEMENTATION_PLAN.md` for detailed step-by-step implementation guide.

See `REQUIREMENTS.md` for complete requirements and entity mappings.

See `CLAUDE.md` for agent context and development guidance.

## Important Notes

- Washer/dryer view currently cast to Google Home Max via "Display Washer Dryer Cards" automation
- All door/lock controls must have confirmations to prevent accidental taps
- Use sections type for modern mobile layout
- Preserve existing custom cards where needed (flower-card, washer/dryer cards, etc.)

## Files

- `my_dashbaord.yaml` - Current dashboard (working copy)
- `REQUIREMENTS.md` - Complete requirements document
- `IMPLEMENTATION_PLAN.md` - Step-by-step implementation guide
- `CLAUDE.md` - Agent context and guidance
- `README.md` - This file
