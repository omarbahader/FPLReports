# FPL Weekly Strategic Review Prompt

## How to Use This Prompt Each Gameweek

**One-time setup:**
1. Open `fpl_data_collector.js` in your FPL folder
2. Copy the entire script
3. Go to https://fantasy.premierleague.com/my-team (logged in)
4. Press F12 → Console → Paste & Enter → JSON file downloads automatically

**Each gameweek (takes ~10 seconds):**
1. Run the collector script → get `fpl_snapshot_GW{XX}.json`
2. Drop the file into your FPL folder (or drag into Claude chat)
3. Paste the prompt below into Claude

---

## The Analysis Prompt

> Copy everything below this line and paste it to Claude, with the JSON file attached or in your folder.

---

Act as an expert Fantasy Premier League (FPL) manager and data analyst for the 2025/26 season. I have provided a structured JSON snapshot of my team and all relevant league data. **Do not navigate any webpages** — all data you need is in the attached file.

The snapshot file is located at: `/FPL/fpl_snapshot_GW{XX}_{date}.json`
*(Replace with actual filename, or just say: "the FPL snapshot I just dropped in")*

**Read the JSON file and then produce a complete strategic review with the following sections:**

---

### Section 1 — Team Snapshot
Extract and display:
- Team name, manager, overall points & rank
- Bank balance, squad value, free transfers available
- Available chips vs used chips (with GW used)
- Flag any upcoming BGWs or DGWs from `gameweek_overview`

---

### Section 2 — Player-by-Player Analysis
For **every player in `squad`**, produce a table with columns:

| Player | Pos | Team | Sell £ | TP | Form | PPG | xG | xA | GW{current} Fixture | Next 5 FDR | News/Flag | Verdict |

**Verdict logic:**
- **HOLD** — good form (≥5.0), clean fixtures ahead, minutes secure
- **MONITOR** — form 3.0–5.0, or fixture swing coming, or ownership risk
- **SELL** — form <3.0, injured/doubt, or better option available for similar price
- **SELL (HIT)** — critical issue; worth taking a –4 points penalty to move

For FDR colour-coding in your response: use 🟢 for 1–2, ⬜ for 3, 🔴 for 4–5, ⬛ for BLANK.

---

### Section 3 — Transfer Recommendations

**Primary transfer:** OUT → IN with:
- Exact selling price and buying price
- Points cost (0 FT = –4, 1 FT = free, etc.)
- Fixture-based justification for the next 5 GWs
- xG/form comparison

**Alternative transfer:** same format

**Do NOT recommend a transfer if:**
- The squad has 0 FTs and no urgent injury/BLANK issue
- The budget delta is negative

---

### Section 4 — Chip Strategy

For each chip in `chips.available`:
- Recommended deployment gameweek
- Reasoning: how many of my 15 players have fixtures that GW? Are any doubles?
- What team composition changes are needed to maximize the chip?

Use `blanks_by_gw` and `doubles_by_gw` from the snapshot for accuracy.

---

### Section 5 — Captaincy Pick

Recommend captain and vice-captain for the **current gameweek** only.
- Must come from the starting XI
- Show: fixture FDR, form, xG, historical big-game returns
- Include one "differential" captain option (TSB < 15%)

---

### Section 6 — Optimal Starting XI & Bench Order

Recommend the best 11 + bench order, accounting for:
- Yellow flags (`chance_next < 100`) → move to bench, promote cover
- Formation that maximizes expected points given current fixtures
- Bench ordered by: most likely to play first → least likely

---

### Section 7 — Risk Flags Before Deadline

List any items to monitor before the deadline:
- Players with `news` set or `chance_next < 75`
- Players whose team has a BGW in GW+1 (relevant for early chip decisions)
- Any player overperforming xG by >5 goals (regression risk)
- Any player underperforming xG by >4 goals (bounce candidate)

---

**Output format:** Use tables where possible. Bold key recommendations. Keep verdict justifications to one line. Highlight the single most important action in a "Bottom Line" box at the end.
