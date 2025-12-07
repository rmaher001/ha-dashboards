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
4. **Deprecated HACS resource format** - `?hacstag=` format causing loading issues

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

**Fix:** Complete fresh reinstall of fold-entity-row via HACS. The deprecated `?hacstag=` resource format was causing the runtime error.

### Post-Fix Work

- Replaced stack-in-card usage (Fridge/Freezer cards) with native `horizontal-stack`
- Removed card_mod styling (card-mod was removed)

### Lessons Learned

1. WebRTC Camera integration has a frontend bug in v3.6.1 - avoid unless actively needed
2. Multiple HACS cards can pollute global JavaScript scope with minified variable conflicts
3. Deprecated HACS resource format (`?hacstag=`) can cause runtime errors even when cards load successfully
4. Fresh reinstall via HACS updates the resource registration format and resolves loading issues

### Prevention

- Regularly update HACS cards to avoid deprecated resource formats
- Remove unused HACS frontend components
- Test in incognito window when diagnosing frontend issues
- Check browser console for JavaScript errors when cards fail
