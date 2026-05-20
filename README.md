# ISSYEMEN Carnaval 26 — Swimming & Running Spectator

Live spectator page for UTM ISS-YEMEN carnaval swimming and running events. A single static page (`index.html`) pulls published Google Sheets CSV data and displays matches grouped by round alongside an overall leaderboard.

## Quick start

Open `index.html` in a browser, or serve the folder locally:

```bash
python3 -m http.server 8080
# → http://localhost:8080
```

Routes: `#/swimming` (default) and `#/running`. Use **Refresh** to reload data from the sheet.

## Project layout

| File / folder | Purpose |
| :--- | :--- |
| `index.html` | Spectator UI — fetches CSV, renders rounds + leaderboard |
| `digital_identity/` | Event and UTM ISS-YEMEN logos |
| `data_specification.md` | This file — project overview and sheet schema |

**Stack:** HTML, Tailwind CSS (CDN), PapaParse (CDN). No build step.

## Frontend behaviour

- **Rounds (left panel):** Match rows are grouped by `Round` (Round 1, Round 2, …). Each card lists Name, Line, Group, and Score. Rows without a round are ignored (leaderboard-only entries).
- **Leaderboard (right panel):** Sorted by rank. Shows Name, Rank, and average score computed from all match rows for that participant.
- **Filtering:** Click a participant name to filter both panels; click **Clear** to reset.

---

## Data sources

Public CSV exports from Google Sheets:

* **Swimming:** [CSV link](https://docs.google.com/spreadsheets/d/e/2PACX-1vS-13Ijo8MGALhB5farVGJq2I2I9cKEL570IYw89tVJ8IbPX_rKOOOCdZV5IbOS7_KLKN4GVofsGAKn/pub?gid=1242237186&single=true&output=csv)
* **Running:** [CSV link](https://docs.google.com/spreadsheets/d/e/2PACX-1vS-13Ijo8MGALhB5farVGJq2I2I9cKEL570IYw89tVJ8IbPX_rKOOOCdZV5IbOS7_KLKN4GVofsGAKn/pub?gid=1425156962&single=true&output=csv)

> Both sheets use the same column layout. Update the sheet; the page picks up changes on refresh.

---

## CSV structure

Each sheet exports two logical tables side-by-side, separated by an empty column (double comma in CSV):

1. **Match details** (left) — participants per round/group
2. **Leaderboard** (right) — overall standings

### Header line

```csv
Name,Line,Round,Group,Score in seconds,,Name,Rank
```

| CSV index | Table | Column | Type | Description |
| :---: | :--- | :--- | :--- | :--- |
| 0 | Match | Name | String | Participant name |
| 1 | Match | Line | Integer | Lane / track number |
| 2 | Match | Round | Integer | Tournament round |
| 3 | Match | Group | Integer | Heat/group within the round |
| 4 | Match | Score in seconds | Float | Time in seconds (e.g. `30.5`) |
| 5 | — | *(empty)* | — | Separator between tables |
| 6 | Leaderboard | Name | String | Participant name |
| 7 | Leaderboard | Rank | Integer | Overall rank |

---

## Sample rows

| Match details | Leaderboard |
| :--- | :--- |
| `Ali, 1, 1, 1, 63.4` | `Ali, 5` |
| `Muhammed, 2, 1, 2, 30.5` | `Muhammed, 2` |
| `Nobody, 3, 2, 1, 122.3` | `Nobody, 3` |
| `Macron's hasband, 4, 2, 2, 120.0` | `Macron's hasband, 4` |
| *(empty match cells)* | `Salah Alwafi, 1` |

### Notes

1. **Separator column:** Index 5 is always blank — do not put data there.
2. **Leaderboard-only rows:** A participant may appear on the right without match columns filled (e.g. `Salah Alwafi`). The UI shows them on the leaderboard only.
3. **Scores:** Use standard float seconds. The frontend averages scores across all match rows per name for the leaderboard "Average Score" column.
