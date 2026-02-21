# Cursor Build Prompt (copy/paste)

Build a mobile/iPad-first web app called XODraw using Next.js (App Router) + TypeScript.

Goal (v0.1):
Voice + text -> deterministic PlayPlan JSON -> render as simple SVG play diagram -> show/share/print -> save locally.

Routes:
- / : Create screen
- /diagram : Diagram screen
- /show : Fullscreen Show Mode
- /saved : Saved diagrams list

Create screen (/):
- Sport toggle: Soccer | Hockey
- Soccer view: Half-field | Full-field
- Hockey view: Full rink (zones optional later; v0.1 can be full rink only)
- Big hold-to-talk button using Web Speech API (SpeechRecognition). If unavailable, fall back to typed transcript input.
- Text notes box (constraints).
- Generate button -> creates a PlayPlan and navigates to /diagram?id=<pageId>
- Recent saved list preview.

Generation:
- Implement a rule-based parser to convert transcript+notes into a PlayPlan:
  - Positions recognized:
    Soccer: GK, LB, CB, RB, DM, CM, AM, LW, RW, ST (also D/M/F)
    Hockey: LW, C, RW, LD, RD, G (optional F1/F2/F3)
  - Keywords recognized:
    Soccer: overlap, switch, press, cross, through
    Hockey: breakout, rim, cycle, forecheck, point, net-front, chip
  - Directions: left/right, boards/middle, wide, low/high
- If parsing fails, generate a sensible default formation for the sport and put the transcript in notes.

Signifiers / Rendering:
- Players: labeled circles
- Ball/puck: small filled dot
- MOVE: solid arrows (curved for rotation)
- PASS: dashed arrows
- SHOT: thick arrow to goal
- Minimal field/rink outline appropriate to the selected view
- Enforce simplicity: if too many actions, keep the core and move extras to notes.

Diagram screen (/diagram):
- Render PlayPlan to SVG
- Buttons: Show Mode, Save, Download PNG, Share, Print
- Refinement chips that deterministically modify the PlayPlan and re-render:
  - Flip L/R (mirror x)
  - Simplify (reduce actions)
  - Emphasize rotation (convert certain MOVE arrows to curved)

Export/sharing:
- Convert SVG to PNG via canvas for download/share
- navigator.share on mobile if available, otherwise provide download

Persistence:
- Save last 20 diagrams locally (IndexedDB preferred; localStorage acceptable v0.1)
- Model stores: id, createdAt, sport, view, transcript, notes, PlayPlan JSON, pngDataUrl

Printing:
- Print button triggers window.print()
- Add print CSS: title + large diagram + notes + timestamp

Deliverables:
- Working Next.js project with the above routes and components
- Typed PlayPlan interfaces
- Utilities: speech capture, plan generation, svg rendering, png export, local persistence
- Keep UI minimal, high-contrast, readable on phone and iPad.
