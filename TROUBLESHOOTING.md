# Dashboard Troubleshooting Log

## Issue: Frontend Errors in Monitoring Sections (2025-12-07)

### Symptoms
- Monitoring sections displayed "Configuration error"
- Raw CSS text visible instead of fold-entity-row content
- Multiple JavaScript errors in browser console

### Root Causes Identified

1. **WebRTC Camera v3.6.1** - Used ES module syntax but loaded as regular script, corrupting JS environment
2. **Multiple HACS cards with scope pollution** - Cards using minified variable names (`e`, `se`) conflicting in global scope
3. **card-mod + layout-card recursion** - Infinite loop between these two cards
4. **Corrupted browser cache** - Old cached JavaScript loaded in broken state due to environment pollution

### Components Removed

| Component | Reason |
|-----------|--------|
| WebRTC Camera integration | ES module loading error, not actively used |
| browser_mod | 404 resource error |
| llmvision (resource) | Frontend errors |
| more-info-card | "Identifier 'e' already declared" |
| state-switch | "Identifier 'e' already declared" |
| apexcharts-card | "Identifier 'se' already declared" |
| mini-media-player | "Identifier 'se' already declared" |
| mini-graph-card | Not used |
| card-mod | Infinite recursion with layout-card |
| stack-in-card | "Identifier 'e' already declared" |

### Resolution

After removing problematic components, fold-entity-row still failed with:
```
TypeError: Cannot read properties of undefined (reading 'has')
```

**Fix:** Complete fresh reinstall of fold-entity-row via HACS. The fresh install generated a new `?hacstag=` value (HACS's cache-busting parameter), forcing the browser to fetch a completely fresh copy without corrupted cache.

### Post-Fix Work

- Initially replaced stack-in-card with native `horizontal-stack` (lost container styling)
- Reinstalled stack-in-card fresh via HACS - works correctly now
- Restored stack-in-card for Fridge/Freezer cards with proper container styling
- card_mod styling removed (card-mod was removed due to recursion issues)

### Lessons Learned

1. WebRTC Camera integration has a frontend bug in v3.6.1 - avoid unless actively needed
2. Multiple HACS cards can pollute global JavaScript scope with minified variable conflicts
3. Browser cache corruption can persist even after fixing root cause - fresh HACS reinstall clears it
4. The `?hacstag=` parameter is HACS's cache-busting mechanism - reinstalling generates new values and forces fresh downloads

### Prevention

- Remove unused HACS frontend components to reduce potential conflicts
- Test in incognito window when diagnosing frontend issues (bypasses cache)
- Check browser console for JavaScript errors when cards fail
- If cards fail after an integration update, try fresh HACS reinstall to clear cache
- **NEVER do development/testing on production systems** - test in dev environment first
