# Weekly Tracker

A minimal, local-first weekly tracking dashboard with customizable habits.

## Quick Start

Open `index.html` in any modern browser. No server or build step required.

```bash
# Option 1: Direct open
open index.html

# Option 2: Python server (for local development)
python3 -m http.server 8000
# Then visit http://localhost:8000
```

## Data Storage

- **Location**: Browser LocalStorage under key `weeklyTracker.data`
- **Persistence**: Data persists across browser sessions
- **Scope**: Data is specific to the browser/domain you use
- **Schema Version**: v2 (automatic migration from older formats)

### Auto Backup

When enabled (default), the app automatically downloads a JSON backup file when you start a new week. This ensures you always have a backup of the previous week's data.

- Toggle in **Settings** (gear icon) → "Enable auto backup on week change"
- Backup file name: `tracker-backup-YYYY-MM-DD.json`
- Triggers when the current week changes from your last session

### Export / Import

- Click **Export** to download a backup of all your data
- Click **Import** to restore from a backup file
- **Safe Merge**: When importing, the app intelligently merges data:
  - Newer entries (by `updatedAt` timestamp) take precedence
  - Habits and targets are merged from both sources
  - No data is lost during import

## Features

### Habit Types

The app supports three habit types:

1. **Checklist**: Multiple boolean items (e.g., Voice with Mini Activation, Articulation, Vocal Session)
2. **Single Choice**: Radio-style options (e.g., Sport with None/Cardio/Weights+Cardio)
3. **Scale**: Numeric 0-5 rating buttons (e.g., Work, Guitar)

### Default Habits

- **Voice** (checklist): Mini Activation, Articulation, Vocal Session
- **Sport** (singleChoice): None, Cardio, Weights + Cardio
- **Work** (scale): 0-5 deep work blocks
- **Guitar** (scale): 0-5 practice rating

### Habit Manager

Access via the **Habits** button to:
- Add new habits (any type)
- Edit existing habits (label, type, options)
- Archive/unarchive habits
- Reorder habits with up/down buttons

### Targets System

- Set weekly targets for each habit
- Checklist habits: target per item (days per week)
- Scale habits: target is the weekly sum
- Single choice habits: target days with activity

## Usage

### Daily Logging (10-30 seconds)

1. Click any day cell in the weekly grid
2. In the drawer, check/update your activities
3. Changes auto-save instantly

### Navigation

- **< >** buttons: Navigate between weeks
- **+ New Week**: Create current week (or next chronological week)
- **Targets**: Edit this week's specific targets
- **Habits**: Manage your habit configuration
- **Settings**: Toggle auto backup

### Week Summary

The summary cards show:
- Progress toward weekly targets
- Color-coded performance (green=100%+, cyan=70-99%, yellow=40-69%, red=<40%)
- Overall percentage (habits with `includeInOverall: true`)

## Metrics

**Percentage Calculations:**
- Checklist: Average completion of all items vs their targets
- Scale: Sum of daily values vs target
- Single Choice: Days with non-zero selection vs target
- Overall: Weighted average of habits marked for overall inclusion

## Keyboard Shortcuts

- `Escape`: Close any open drawer or modal

## Data Schema (v2)

```javascript
{
  schemaVersion: 2,
  habitsConfig: [...],  // Array of habit definitions
  targets: {...},       // Key-value pairs for targets
  weeks: {              // Weekly data keyed by ISO week (YYYY-Www)
    "2025-W51": {
      days: {
        "2025-12-15": {
          habits: {...},
          updatedAt: "2025-12-15T10:30:00.000Z"
        }
      }
    }
  },
  meta: {
    lastSeenWeekKey: "2025-W51",
    lastAutoBackupWeekKey: "2025-W50",
    autoBackupEnabled: true
  }
}
```
