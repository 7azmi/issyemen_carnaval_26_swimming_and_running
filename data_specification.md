# Google Sheets Data Specification: Swimming & Running

This document details the structure, fields, and sample values extracted from the public Google Sheets for the Swimming and Running events.

---

## 1. Data Sources

The following public URLs export the event data in CSV format:

*   **SWIMMING:** [Google Sheet CSV Link](https://docs.google.com/spreadsheets/d/e/2PACX-1vS-13Ijo8MGALhB5farVGJq2I2I9cKEL570IYw89tVJ8IbPX_rKOOOCdZV5IbOS7_KLKN4GVofsGAKn/pub?gid=1242237186&single=true&output=csv)
*   **RUNNING:** [Google Sheet CSV Link](https://docs.google.com/spreadsheets/d/e/2PACX-1vS-13Ijo8MGALhB5farVGJq2I2I9cKEL570IYw89tVJ8IbPX_rKOOOCdZV5IbOS7_KLKN4GVofsGAKn/pub?gid=1425156962&single=true&output=csv)

> [!NOTE]
> Currently, both URLs return identical mock/placeholder dataset values. However, the data format and column structure are fully defined below.

---

## 2. CSV Structure Overview

Each sheet exports a side-by-side table layout separated by an empty column (represented by double commas `,` in the CSV). The two logical tables represent:
1.  **Current Heat / Participant Details** (left side)
2.  **Overall Leaderboard / Standings** (right side)

### Column Header Line
```csv
Name,Line,Round,Group,Score in seconds,Status,,Name,Rank,Status
```

---

## 3. Detailed Schema

### Table 1: Heat/Race Details (Columns 1–6)
This table displays information about individual participants in their respective race groups, lanes, and rounds.

| Column Name | Data Type | Description / Allowed Values |
| :--- | :--- | :--- |
| **Name** | String | The name of the participant. |
| **Line** | Integer | Lane or track number (e.g., `1`, `2`, `3`, `4`). |
| **Round** | Integer | The tournament round (e.g., `1`, `2`). |
| **Group** | Integer | The heat/group number within that round (e.g., `1`, `2`). |
| **Score in seconds** | Float | Time duration in seconds (e.g., `30.5`, `63.4`, `122.3`). |
| **Status** | String | The status of the heat/participant. Examples: `Coming`, `Live`, `Completed`, `Canceled`. |

### Table 2: Leaderboard / Standings (Columns 8–10)
This table displays the overall results and rankings across the entire event.

| Column Name | Data Type | Description / Allowed Values |
| :--- | :--- | :--- |
| **Name** | String | The name of the participant. |
| **Rank** | Integer | The participant's current rank in the event (e.g., `1`, `2`, `3`, `4`, `5`). |
| **Status** | String | The standing outcome. Examples: `Winner`, `Qualifier`, `Ready`, `withdrawn`, `Out`. |

---

## 4. Sample Data Reference

Below is the raw text representation of the fetched data rows:

| Left Table (Heat Details) | Right Table (Leaderboard) |
| :--- | :--- |
| `Ali, 1, 1, 1, 63.4, Coming` | `Ali, 5, Ready` |
| `Muhammed, 2, 1, 2, 30.5, Live` | `Muhammed, 2, Qualifier` |
| `Nobody, 3, 2, 1, 122.3, Completed` | `Nobody, 3, withdrawn` |
| `Macron's hasband, 4, 2, 2, 120.0, Canceled` | `Macron's hasband, 4, Out` |
| *(Empty)* | `Salah Alwafi, 1, Winner` |

### Key Observations:
1. **Side-by-Side Separation:** The 7th column of the CSV is completely empty, acting as a visual and logical separator between the two tables.
2. **Participant Discrepancy:** Participants might exist in the Leaderboard but not in the active Heat details (e.g., `Salah Alwafi` is ranked #1 as `Winner` but doesn't have an active heat row listed).
3. **Data Syncing:** The score field uses standard float representations for timing events, while statuses on both tables dictate state transitions (e.g., `Live` vs. `Qualifier`).
