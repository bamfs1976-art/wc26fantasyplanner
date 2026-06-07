# WC26 Fantasy Planner

Single-file fantasy planner for the 2026 FIFA World Cup. Squad management,
fixture-by-fixture player picks, captain log, transfers, projections, and a
data-led Fixture Difficulty / Team Form workbench.

- **One `index.html`**, vanilla JavaScript, no build step, no API at runtime.
- Open it locally, double-click, or drag to Netlify Drop.
- Auto-saves to `localStorage` under `wc26_fantasy_v1`.

## Tabs

- **Overview** – ranking-gap summary, top mismatches across all rounds, sort options.
- **My Squad** – build a 15-man squad from a 1,163-player pool, filter by team/position/price.
- **Squad News** – injury and form notes.
- **Optimum XI** – auto-suggested line-up given your squad.
- **Projection** – per-player projected points across the group stage.
- **Rules** – 2026 fantasy scoring reference.
- **Fixture Difficulty** – the data engine:
  - 1–5 FDR per team (FIFA April 2026 rankings)
  - 12 group cards with each team's three group-stage opponents colour-coded
  - All 48 teams ranked by tournament FDR
  - **Captain Picks per round** with composite score (form × FDR), xG breakdown, and **booking-risk warnings**
  - **Clean Sheet Watch** per round — best defensive blocks
- **Team Form** – sortable table of all 48 teams: GF/90, GA/90, goals/g, Over 2.5%, BTTS%, clean sheets, shots, cards, fouls, tier badges.
- **Round 1 / 2 / 3** – every fixture with inline FDR badges, attack/defence player picks (autosave).
- **Boosters** – chip planner.
- **Transfers** – swap calculator.
- **Captain Log** – running record of weekly captains + results.
- **Strategy** – roadmap notes.

## Data sources

All ratings and per-player risk scores are baked in from
[bamfs1976-art/wcstats](https://github.com/bamfs1976-art/wcstats):

- `wc2026_team_form.json` — country form per 90 (cards, fouls, goals, clean sheets,
  shots, Over 2.5%, BTTS%, etc.).
- `intl_array.js` — 2026 international-form booking risk (top 3 risk players per team
  with risk ≥ 0.8).
- FIFA April 2026 men's world rankings.

Both files are harvested in-browser from ScoutingStats AI (which sits on top of
Sportmonks data), then validated by the build scripts in `wcstats/data/`. No
runtime API calls. No keys needed.

## Scoring formulae

### Fixture Difficulty Rating (FDR)
- FDR 5: world rank 1–8 (very hard opponent)
- FDR 4: 9–18
- FDR 3: 19–30
- FDR 2: 31–45
- FDR 1: 46+ (very easy opponent)

### Captain composite score
```
xG          = (team GF/90 + opponent GA/90) / 2
formScore   = xG × (team SoT/90 / 4.5) × 5
fdrScore    = team_FDR × (6 − opponent_FDR)         (range 1–25)
composite   = formScore × 0.6 + fdrScore × 1.2 × 0.4 (range 0–30)
```
- ≥18 elite, 12–17 strong, 7–11 decent, <7 avoid.

### Clean Sheet Watch
```
score = (team's CS / games) × (3 / opponent's GF/90) × 10
```
- ≥12 elite, 7–11 strong, 4–6 decent, <4 avoid.

## Deploy

### Netlify Drop
1. Drag `index.html` onto https://app.netlify.com/drop.
2. Done. Netlify gives you an HTTPS URL.

### Local
Open `index.html` directly in any modern browser.

## Updating

Edit `index.html` and re-drag to Netlify. All user state (picks, captain log,
transfers) is stored client-side in the player's browser — your re-deploys
don't reset their data.

## Credits

Built by Anthony Bamford with help from Claude. ScoutingStats data harvested per
the catalogue in [wcstats/data/scoutingstats_api_notes.md](https://github.com/bamfs1976-art/wcstats/blob/main/data/scoutingstats_api_notes.md).
