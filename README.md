# Weekly Tracker

A minimal, local-first weekly tracking dashboard for Voice, Sport, Work, and Guitar practice.

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

- All data is stored in **LocalStorage** (browser-side only)
- Data persists across browser sessions
- Data is specific to the browser/domain you use

### Export / Import

- Click **Export JSON** to download a backup of all your data
- Click **Import JSON** to restore from a backup file

## Usage

### Daily Logging (10-30 seconds)

1. Click any day cell in the weekly grid
2. In the drawer, check/update your activities:
   - **Voice**: Mini Activation, Articulation, Vocal Session
   - **Sport**: None / Cardio / Weights + Cardio
   - **Work**: +/- deep work blocks
   - **Guitar**: Rate 0-5 (0 = no practice)
3. Changes auto-save instantly

### Navigation

- **< >** buttons: Navigate between weeks
- **+ New Week**: Create current week (or next chronological week)
- **Targets**: Edit this week's specific targets
- **History**: View all weeks with summary stats
- **Overall**: All-time aggregated statistics
- **Gear icon**: Edit default targets & settings

### Targets System

- Default targets apply to new weeks
- Each week can override targets independently
- "Use Defaults" in Targets modal resets to defaults

## Metrics

**Percentage Calculations:**
- Voice: Average of Mini%, Articulation%, Vocal%
- Sport: sportDaysCompleted / targetDays
- Work: totalDeepBlocks / targetBlocks
- Overall: Average of Voice, Sport, Work (Guitar excluded)
- Guitar: Average rating of practiced days only (rating > 0)

## Keyboard Shortcuts

- `Escape`: Close any open drawer or modal
