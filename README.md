# Boston Hospital Finder 🏥

A web app that helps users find hospitals near any US ZIP code, built with open data from the [City of Boston](https://data.boston.gov). This project is used as a hands-on prompt engineering exercise — you will use AI to add real features to a working codebase. This is also the demo session for final project for anyone who is interested in building an app using open datasets. 

---

## What the app already does

- Accepts a ZIP code and search radius from the user
- Converts the ZIP to geographic coordinates using the free [Zippopotam API](https://api.zippopotam.us)
- Fetches hospital location data from Boston's open data portal
- Calculates distance to each hospital using the Haversine formula
- Displays matching hospitals in a sidebar sorted by distance

---

## How to get started

### Step 1 — Fork the repo
Click **Fork** at the top right of this GitHub page. This creates your own personal copy — you can break things freely without affecting anyone else.

### Step 2 — Open the browser editor
On your forked repo, press the **period key (`.`)** on your keyboard. This opens a full VS Code environment in your browser — no install needed.

### Step 3 — Read the code first
Before asking AI for help, spend 5–10 minutes reading `index.html`. Try to answer these questions:
- What does `parseCSV()` do and why is a custom parser needed?
- What is the Haversine formula and why is it used instead of simple subtraction?
- What does the Zippopotam API return and how is it used?
- Find all the `// TODO` comments — what do they tell you about where work needs to be done?

Understanding the code at a high level before prompting AI will make your prompts dramatically more effective.

---

## Your tasks

### 🟢 Ticket 1 — Add a Leaflet Map (In-class exercise)
**What:** Add an interactive map using [Leaflet.js](https://leafletjs.com) that shows a pin for each hospital result.

**Why it matters:** Leaflet is the most popular open-source mapping library. It requires no API key and is used widely in civic tech, journalism, and public health applications.

**What you need to touch:** HTML (map div + script/link tags), CSS (map sizing), JavaScript (initMap, makeMarker, marker loop, fit bounds)

**Find:** All `// TODO: Ticket 1` comments in the code — they mark every spot you need to change.

<details>
<summary>💡 Show hint</summary>

Try this prompt with your full `index.html` pasted in:

```
I am adding Leaflet.js to my hospital finder app. Here is my full code.
Find all the // TODO: Ticket 1 comments and complete them to:
1. Load Leaflet CSS and JS from cdnjs.cloudflare.com
2. Add a map div that fills the left side of the screen
3. Initialize a Leaflet map centered on Boston
4. Add OpenStreetMap tiles
5. Create circle markers for each hospital and fit the map to show them all
Do not change anything outside the TODO comments.
```
</details>

---

### 🟠 Ticket 2 — Color Pins by Vulnerability Score (Bonus)
**What:** Load a second dataset — [Climate Ready Boston Social Vulnerability](https://data.boston.gov/dataset/climate-ready-boston-social-vulnerability) — and use it to color each hospital pin based on the vulnerability of its neighborhood. Green = low, orange = medium, red = high.

**Why it matters:** This is a real data join — connecting two datasets on a shared field (`NEIGH` in hospitals, `Name` in vulnerability). Joining datasets is one of the most common operations in data analysis.

**How the vulnerability score is calculated:**
```
score = (OlderAdult/POP + TotChild/POP + Low_to_No/POP + TotDis/POP + LEP/POP) / 5
```
Each factor is normalized by total population so neighborhoods of different sizes are fairly compared. `MedIllnes` is excluded because it is a modeled estimate with known double-counting issues.

**Find:** All `// TODO: Ticket 2` comments.

<details>
<summary>💡 Show hint</summary>

Try this prompt:

```
I want to add neighborhood vulnerability scoring to my hospital finder.
Here is my full code. Complete all // TODO: Ticket 2 comments to:
1. Fetch the vulnerability CSV from VULN_CSV
2. Group rows by neighborhood Name and calculate a composite score
3. Color each hospital pin and sidebar card based on the score
Use green for low (<0.07), orange for medium (0.07-0.12), red for high (>0.12).
Do not change anything outside the TODO comments.
```
</details>

---

### 🔴 Ticket 3 — Vulnerability Breakdown Popup (Bonus)
**What:** When a user clicks a hospital pin on the map, show a popup with a detailed breakdown of the neighborhood's vulnerability — percentage of elderly residents, children, low income households, people with disabilities, and limited English speakers.

**Why it matters:** This teaches Leaflet interactivity and the difference between displaying a summary (a color) vs the underlying data (the breakdown). Good data tools always let users go deeper.

**Find:** All `// TODO: Ticket 3` comments.

<details>
<summary>💡 Show hint</summary>

Try this prompt:

```
I want clicking a hospital pin to show a popup with vulnerability details.
Here is my full code. Complete all // TODO: Ticket 3 comments to:
1. Build a makePopupHTML(hospital, vuln) function that shows each factor as a percentage
2. Bind the popup to each Leaflet marker
3. Make sidebar cards clickable to open the popup and pan the map
Do not change anything outside the TODO comments.
```
</details>

---

## Prompt engineering guidelines

**1. Always paste the full file**
AI needs full context. Never paste just a snippet — always paste the entire `index.html`.

**2. Reference the TODO comments directly**
Tell AI exactly which TODO comments to complete. This prevents it from rewriting unrelated parts of the code.

**3. Use this system prompt at the start of every session:**
```
Respond with clean, well-written code. Only add comments that belong in production.
Focus only on the task I describe. Do not make unnecessary changes to other parts of the code.
```

**4. Coach it step by step**
If AI gives you code that almost works but has a bug, describe exactly what's wrong and ask it to fix only that part.

**5. Ask AI to explain before changing**
If you don't understand what the AI plans to do, ask: *"Before you change anything, explain what you plan to do and why."*

---

## Understanding the data

| Dataset | Source | Key fields |
|---|---|---|
| Hospital Locations | [data.boston.gov](https://data.boston.gov/dataset/hospital-locations) | NAME, AD, NEIGH, LOCATION (lat/lng embedded) |
| Climate Vulnerability | [data.boston.gov](https://data.boston.gov/dataset/climate-ready-boston-social-vulnerability) | Name, POP100_RE, OlderAdult, TotChild, Low_to_No, TotDis, LEP |

The two datasets are joined on **neighborhood name** — `NEIGH` in hospitals matches `Name` in vulnerability.

Note that the vulnerability dataset is from 2016 census data and reflects conditions at that time. Real-world analysis would use more recent ACS estimates.

---

## About
Built with plain HTML, CSS, and JavaScript. No frameworks, no build tools.
Uses [Leaflet.js](https://leafletjs.com) for maps and [OpenStreetMap](https://www.openstreetmap.org) tiles.
Hosted on GitHub Pages.
