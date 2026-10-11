# Phoenix OS — run doc

## What this project is
A single, self-contained local web app: `index.html` (HTML5 + CSS3 + vanilla JS).
No build step, no package manager, no dependencies, no network calls. All state lives in
`localStorage` under the key `phoenix-os-v2` (plus the legacy `workout_presets` compat key).

## Reproduce the artifacts a fresh checkout needs
Nothing. There are no generated artifacts, no env files, and no install step.
(`index - Copy.html` / `index - Copy (2).html` are older manual backups — not used at runtime.)

## Run it
Lightest option — open the file directly, or register the HTML preview:

1. `htmlPath` preview: `C:\Users\DELL\Documents\Notion_2.0\index.html`
2. Or plain file open: `start index.html` (Windows) — `file://` works, everything is offline.

Optional static server (only if a `file://` origin is a problem):

```bash
python -m http.server 8765 --bind 127.0.0.1
# then browse http://127.0.0.1:8765/index.html
```

No port is baked into the app, so any free port works.

## Verify a change
Extract the single inline `<script>` and syntax-check it:

```bash
python - <<'PY'
import re, pathlib
s = pathlib.Path('index.html').read_text(encoding='utf-8')
pathlib.Path('.freebuff/_inline.js').write_text(re.findall(r'<script(?![^>]*src=)[^>]*>(.*?)</script>', s, re.S)[-1], encoding='utf-8')
PY
node --check .freebuff/_inline.js
```

Then open the app and confirm the Dashboard, Daily Log, School, Goal Matrix, Film Study,
Finance, Recruiting CRM and Mental tabs each render without console errors.

Quick manual QA of the newest surfaces:

- **Live Workout Mode** — Daily Log & Workouts → Workout Sessions → `Start Live Workout`.
  Tap Make/Miss/Rep, start a 30/60/90s rest (it pings and flashes on completion), push
  attempts past 15 with a sub-40% rate to see the mental banner, then `Finish & Save Session`
  and check the workout history + badge counters.
- **Notion export** — Dashboard → `Export to Notion` → copy the Markdown and paste it into a
  Notion page.
- **Optional live timer** — inside Live Mode, `⏱ Timer: On/Off` hides the stopwatch and saves the
  session with counters only. `−1 Make / −1 Miss / −1 Rep` undo accidental double-taps.
- **Investing & Wealth** — Finance Tracker → Section D: portfolio KPIs, asset breakdown,
  compound calculator (typing only updates the outputs) and the investment log.
- **Recovery & game averages** — Dashboard slider logs a daily 1-10 score and correlates it with
  shooting %; Film Study → My Games shows PPG/APG/RPG/FG% for the filtered season.
