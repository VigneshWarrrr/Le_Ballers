# SITREP·GEN — AI Tactical Situation Report Generator

> **⚠ FOR TRAINING, SIMULATION & DEMONSTRATION USE ONLY. NOT FOR OPERATIONAL USE.**

A browser-based AI tool that transforms raw battlefield field data into structured military Situation Reports (SITREPs), complete with live tactical map visualization and threat assessment.

---

## 📸 Features

- **Dual AI Provider Support** — works with both Anthropic (Claude) and Google Gemini APIs
- **Live Tactical Map** — auto-plots all extracted coordinates as color-coded markers (enemy / friendly / event)
- **Structured SITREP Output** — generates formatted reports with DTG, classification, situation, mission, execution, threat assessment, recommended actions, intel gaps, and event timeline
- **Threat Level Indicator** — visual LOW / MEDIUM / HIGH / CRITICAL badge with color coding
- **4 Preloaded Scenarios** — Urban Assault, Border Incursion, Cyber + Physical, Coastal Threat
- **Zero Backend** — runs entirely in the browser, no server or install required

---

## 🚀 Quick Start

### 1. Open the app
Just double-click `sitrep-generator.html` in Chrome or Firefox. No server, no `npm install`, no setup.

### 2. Get an API Key

**Anthropic (Claude)**
1. Go to [console.anthropic.com](https://console.anthropic.com)
2. Sign up / log in
3. Navigate to **API Keys** → **Create Key**
4. Copy the key — it starts with `sk-ant-api03-...`
5. Add billing credits if prompted (~$5 minimum, costs ~$0.01 per generation)

**Google Gemini**
1. Go to [aistudio.google.com](https://aistudio.google.com)
2. Click **Get API Key** → **Create API Key**
3. Copy the key — it starts with `AIzaSy...`
4. Free tier available with rate limits

### 3. Generate a SITREP
1. Select your provider (**ANTHROPIC** or **GEMINI**) at the top
2. Paste your API key
3. Click a **Quick Load Scenario** or enter your own raw field data
4. Hit **▶ GENERATE SITREP** (or `Ctrl+Enter`)

---

## 🗂 Project Structure

```
sitrep-generator.html     ← Entire app in a single file
README.md                 ← This file
```

Everything lives in one self-contained HTML file:

| Section | What it does |
|---|---|
| CSS `:root` variables | Color theme — edit here to restyle |
| `<div class="apibar">` | Provider toggle + API key input |
| `setProvider()` | Switches between Anthropic / Gemini |
| `callAnthropic()` | Handles Claude API request/response |
| `callGemini()` | Handles Gemini API request/response |
| `generateSITREP()` | Main orchestrator — builds prompt, calls AI, parses JSON |
| `renderSITREP()` | Renders structured SITREP HTML output |
| `plotCoordinates()` | Plots map markers via Leaflet.js |
| `const presets` | Preloaded scenario data |

---

## 📥 Input Format

The app accepts free-form raw field data. Include any combination of:

```
0630Z: Enemy BTG spotted at 33.8547N, 35.8623E moving south
0645Z: IED detonated at 33.8201N, 35.9012E, 2 casualties
0700Z: Friendly unit BRAVO-3 at 33.9100N, 35.7800E holding position
0715Z: UAV confirms armored column of 12 vehicles at 33.7800N, 36.0200E
```

The AI will automatically extract:
- **Coordinates** → plotted on map
- **Timestamps** → event timeline
- **Unit designations** → friendly/enemy classification
- **Events** → situation summary

---

## 📤 Output — SITREP Structure

| Field | Description |
|---|---|
| `DTG` | Date-Time Group (e.g. `221430ZAPR26`) |
| `Classification` | UNCLASSIFIED / CONFIDENTIAL / SECRET |
| `Situation` | Enemy and friendly force assessment |
| `Mission` | Clear mission statement |
| `Execution` | How the mission will be carried out |
| `Threat Level` | LOW / MEDIUM / HIGH / CRITICAL |
| `Threat Assessment` | Detailed threat analysis paragraph |
| `Recommended Actions` | 5 prioritized action items |
| `Intel Gaps` | Missing information needed |
| `Event Timeline` | Chronological event log |
| `Coordinates` | Lat/lon array → plotted on map |

---

## 🗺 Map

Powered by [Leaflet.js](https://leafletjs.com) with a CartoDB dark tile layer.

| Marker Color | Meaning |
|---|---|
| 🔴 Red | Enemy position |
| 🔵 Blue | Friendly unit |
| 🟡 Amber | Event / Unknown |

The map auto-fits bounds to all plotted markers after generation.

---

## 🔌 API Details

### Anthropic (Claude)
- **Endpoint:** `https://api.anthropic.com/v1/messages`
- **Model:** `claude-sonnet-4-20250514`
- **Auth:** `x-api-key` header (handled automatically)
- **Docs:** [docs.anthropic.com](https://docs.anthropic.com)

### Google Gemini
- **Endpoint:** `https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-pro:generateContent`
- **Model:** `gemini-1.5-pro`
- **Auth:** `?key=` query parameter
- **Docs:** [ai.google.dev](https://ai.google.dev)

---

## 🎨 Customization

### Change the color theme
Edit CSS variables at the top of the `<style>` block:
```css
:root {
  --bg:       #060b06;   /* Main background */
  --green:    #39ff4a;   /* Primary accent */
  --amber:    #ffb300;   /* Warning color */
  --red:      #ff3a3a;   /* Enemy / danger */
  --blue:     #00d4ff;   /* Friendly / Gemini */
}
```

### Add a new preset scenario
Add an entry to the `presets` object in the `<script>` block:
```javascript
const presets = {
  myScenario: {
    unit: 'UNIT NAME',
    raw: `0600Z: Event at 33.00N, 35.00E ...`,
    mission: 'Mission objective here.'
  },
  // ... existing presets
};
```
Then add a button in the HTML presets section:
```html
<button class="preset-btn" onclick="loadPreset('myScenario')">🎯 MY SCENARIO</button>
```

### Modify the SITREP format
Edit the `systemPrompt` string inside `generateSITREP()` to add/remove fields from the JSON output, then update `renderSITREP()` to display them.

---

## ⌨️ Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl + Enter` | Generate SITREP |

---

## 🛠 Tech Stack

| Technology | Purpose |
|---|---|
| HTML / CSS / JS | Single-file frontend, zero dependencies to install |
| [Leaflet.js](https://leafletjs.com) v1.9.4 | Interactive tactical map |
| [CartoDB Dark Matter](https://carto.com/basemaps/) | Dark map tile layer |
| [Google Fonts](https://fonts.google.com) | Share Tech Mono, Barlow Condensed, Courier Prime |
| Anthropic Claude API | AI SITREP generation (provider option 1) |
| Google Gemini API | AI SITREP generation (provider option 2) |

---

## ⚠️ Security Notes

- **API keys are entered client-side** — safe for demos and workshops, not for production deployment
- For production, proxy API calls through a backend server so keys are never exposed in the browser
- Never commit API keys to version control
- The app sends field data to external AI APIs — do not use with real classified information

---

## 📋 Limitations

- Coordinate extraction accuracy depends on AI model and data formatting — always verify plotted coordinates
- Gemini free tier has rate limits; Anthropic requires billing credits
- Internet connection required (API calls + map tiles + Google Fonts)
- No data persistence — reports are not saved between sessions

---

## 📄 License

Built for educational and demonstration purposes at an AI workshop.
For training and simulation use only.

---

*SITREP·GEN // AI TACTICAL ANALYSIS SYSTEM // EXERCISE USE ONLY*
