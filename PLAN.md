# Implementation Plan: Voice & Sport Detail Pages

## Overview
Add dedicated full-screen modal pages for Voice and Sport habits, providing more space for detailed logging without cluttering the main drawer.

---

## Patch A: Voice Detail Page

### Goal
Create a dedicated modal for Voice habit details with better layout for checklists, notes, and templates.

### UI Changes
1. Add "Open Voice Details" button in the drawer's Voice section
2. Create `voiceDetailsModal` - full-screen modal with:
   - Header: Date + Close button
   - Three collapsible sections (Mini Activation, Articulation, Vocal Session)
   - Each section: checkbox status, checklist items, notes, template buttons
   - Better spacing and larger touch targets

### Code Changes
- **HTML**: Add `voiceDetailsModal` markup (~40 lines) near existing modals (line ~1233)
- **CSS**: Add modal-specific styles for voice layout (~30 lines)
- **JS**:
  - `openVoiceDetailsModal(weekId, dayIndex)` - opens modal, renders content
  - `renderVoiceDetailsInModal(weekId, dayIndex)` - full render with all 3 items
  - Event delegation for checklist add/edit/delete, template save/load
  - Modal close handler that refreshes the day drawer

### No Data Model Changes
- Uses existing `day.details.voice[itemId]` structure
- Uses existing `db.voiceTemplates` for templates

---

## Patch B: Sport Detail Page

### Goal
Create a dedicated modal for Sport habit details with better layout for exercise logging.

### UI Changes
1. Add "Open Sport Details" button in the drawer's Sport section (only shows if Sport = Done)
2. Create `sportDetailsModal` - full-screen modal with:
   - Header: Date + Close button
   - Mode toggle (Cardio Only / Weights + Cardio)
   - Cardio section: type dropdown, duration input
   - Weights section: full-width exercise table with inline set editing
   - Add Exercise / Add Set buttons with better UX

### Code Changes
- **HTML**: Add `sportDetailsModal` markup (~30 lines)
- **CSS**: Add modal-specific styles for sport layout (~40 lines, mainly table layout)
- **JS**:
  - `openSportDetailsModal(weekId, dayIndex)` - opens modal, renders content
  - `renderSportDetailsInModal(weekId, dayIndex)` - full render with exercises table
  - Event delegation for mode toggle, exercise/set add/remove
  - Modal close handler that refreshes the day drawer

### No Data Model Changes
- Uses existing `day.details.sport` structure:
  ```js
  { mode, cardioType, durationMin, exercises: [{ name, sets: [{ weightKg, reps }] }] }
  ```

---

## Files Changed

| File | Patch A | Patch B |
|------|---------|---------|
| `index.html` | ✓ | ✓ |

(Single-file app - all changes in `index.html`)

---

## Data Model Changes

**None required.** The existing schema v3 already supports:

1. ✓ `day.details.voice[itemId].checklist` - array of `{text, done}`
2. ✓ `day.details.voice[itemId].notes` - string
3. ✓ `db.voiceTemplates[itemId]` - template storage
4. ✓ `day.details.sport.mode` - 'cardio' | 'weights_cardio'
5. ✓ `day.details.sport.cardioType` - string
6. ✓ `day.details.sport.durationMin` - number
7. ✓ `day.details.sport.exercises` - array of exercise objects
8. ✓ `day.habits.sport` - boolean gate for sport details
9. ✓ Import/export already handles all these fields
10. ✓ Migration chain (v0→v3) already exists

---

## Implementation Order

### Patch A (Voice)
1. Add `voiceDetailsModal` HTML structure
2. Add CSS for modal layout
3. Add `openVoiceDetailsModal()` function
4. Add `renderVoiceDetailsInModal()` function
5. Wire up "Open Details" button in drawer
6. Add event handlers for checklist/template operations
7. Test with existing data, verify export/import

### Patch B (Sport)
1. Add `sportDetailsModal` HTML structure
2. Add CSS for modal layout (exercise tables)
3. Add `openSportDetailsModal()` function
4. Add `renderSportDetailsInModal()` function
5. Wire up "Open Details" button in drawer (only when Sport=Done)
6. Add event handlers for exercise/set CRUD
7. Test with existing data, verify export/import

---

## Backward Compatibility

- Drawer-based details remain functional (progressive enhancement)
- No schema version bump needed
- Existing JSON exports import without modification
- Detail page buttons are additive, not replacing existing UI
