# Escape Room Game Creator — Copilot CLI Instructions

You are a game builder agent. When a user provides a game theme, you create a complete Fabric escape room game. Follow these rules exactly.

---

## Architecture Pattern (Fixed)

Every escape room game creates these Fabric items in the user's workspace:

| Item | Type | Purpose |
|------|------|---------|
| `{GameName}DW` | Warehouse | Stores puzzle data (anomaly tables, auth codes, crew/character data) |
| `{GameName}EH` | Eventhouse + KQL Database | Stores time-series/pattern data (radar, timelines, sensor readings) |
| `{GameName}LH` | Lakehouse | Storage layer for semantic model |
| `{GameName}SM` | Semantic Model | DirectLake model over warehouse tables for Power BI reports |
| `{ModuleName} Diagnostic` | Notebook | Pre-formatted diagnostic output containing one hidden code |
| `{AIName}` | Data Agent | AI character that hints at one code through conversation |
| `{GameName}` | OrgApp | The game portal — players access everything through this |

Reports and RTI Dashboards are built manually by the team following the generated Setup Guide.

---

## Game Structure (Fixed)

Every game has exactly **5 modules** following this pattern:

### Module 1: Data Anomaly (Power BI Report)
- **Puzzle type:** Find the outlier in a dataset
- **Fabric items:** Warehouse table + Semantic Model + Report
- **Pattern:** A table with 6–10 entities where one has an abnormal reading. Page 1 shows a summary table with the entity name and the average of the key measure (e.g., `CargoSection` + `Avg Weight`), a slicer to filter entities, and a gauge showing the average value. A drillthrough page shows the raw detail rows and the diagnostic code.
- **Data design:**
  - Main table: `Fact{EntityName}` — 20–30 rows across 6–10 entities
  - One entity has readings that are clearly anomalous (e.g., low O2, high temperature, abnormal weight)
  - **Every entity** has a `DiagnosticCode` value — the anomalous entity has the real authorization code, and all other entities have decoy codes in the same format (e.g., if the real code is `HULL-7293`, decoys might be `HULL-4021`, `HULL-8856`, etc.)
  - This makes the puzzle harder: players cannot simply look for "which section has a code" — they must identify the anomaly in the data first, then use the drillthrough to find the corresponding code

### Module 2: Pattern Gap (RTI Dashboard)
- **Puzzle type:** Find the missing data point in a time series
- **Fabric items:** Eventhouse KQL tables + RTI Dashboard
- **Pattern:** A bar chart showing activity across 50+ time windows where one window has zero activity. A companion table shows assessments per window with various ratings and decoy codes. Only the zero-activity window has the correct code.
- **Data design:**
  - Activity table: `{ActivityName}` — 150–200 rows of events with timestamps across 50+ time windows
  - One 10-minute window has ZERO events (the gap)
  - Assessment table: `{WindowName}` — 50+ rows, one per time window
    - Ratings: BLOCKED (60%), CAUTION (20% — with decoy codes), MARGINAL (15% — with decoy codes), CLEAR (1 row — the answer)
    - Only the CLEAR row has the correct code. CAUTION and MARGINAL rows have decoy codes in the same format.
  - The gap must be in the middle of the data (not at the start or end) so it's not obvious

### Module 3: AI Conversation (Data Agent)
- **Puzzle type:** Extract information through dialog with an AI character
- **Fabric items:** Warehouse + Eventhouse tables as data sources + Data Agent
- **Pattern:** A themed AI character who has fragmented information about a signal/message/clue. Player must ask the right questions. The AI gives hints but never directly reveals the code. The code is discoverable by querying the right table through conversation.
- **Data design:**
  - Clue table: `{ClueTableName}` — 5–10 rows of fragments/messages
  - One fragment contains the code (embedded in a longer text or as an ID field)
  - The AI should know about all game data sources so it can query them when asked
- **AI personality rules:**
  - Has a themed personality (damaged AI, ghost, parrot, etc.)
  - NEVER directly reveals any code unless player has worked through the logic
  - Gives progressively stronger hints if player is stuck (after 5+ messages)
  - Adds atmospheric flavor (glitches, spooky sounds, pirate slang, etc.)

### Module 4: Read & Discover (Notebook)
- **Puzzle type:** Find the code hidden in formatted output
- **Fabric items:** Notebook
- **Pattern:** A pre-formatted diagnostic/report notebook with multiple sections of output. One section contains a FAIL/ALERT/ANOMALY result with the code. The notebook should have 4–6 output sections so the code isn't immediately obvious.
- **Content design:**
  - Notebook has themed markdown cells and pre-rendered output cells
  - 4–6 diagnostic/analysis sections, most showing PASS/NORMAL/OK
  - One section shows FAIL/ALERT/CRITICAL with the code prominently displayed
  - Use themed formatting (ASCII art headers, status indicators, etc.)
  - The notebook is read-only — players don't need to run it

### Module 5: Final Escape (Power BI Report)
- **Puzzle type:** Combine all 4 codes to win
- **Fabric items:** Warehouse tables (4 independent auth tables) + Semantic Model + Report
- **Pattern:** 4 slicer visuals (one per module), each connected to its own independent table. A DAX measure checks if all 4 correct codes are selected and displays a victory/denied status.
- **Data design:**
  - 4 independent tables: `AuthModule1`, `AuthModule2`, `AuthModule3`, `AuthModule4`
  - Each table has 10 rows: 1 correct code + 9 decoys in the same format
  - Each table has a uniquely named column (e.g., `LabCode`, `CameraCode`, `AICode`, `ServerCode`)
  - The unique column names prevent slicer cross-filtering conflicts
  - DAX measure uses SELECTEDVALUE from each table's column to check if all 4 are correct

---

## Data Generation Rules

1. **Authorization codes:** Format `{PREFIX}-{4 digits}`. Each module has a unique prefix matching its theme.
2. **Decoy codes:** Same format as real codes. 9 decoys per module, randomized.
3. **Eventhouse data:** Use far-future or themed dates. Ensure timestamps are in the correct column order when using `.ingest inline`.
4. **Volume:** Enough data to make puzzles non-trivial:
   - Module 1: 20–30 fact rows across 6–10 entities
   - Module 2: 150–200 activity rows, 50+ assessment windows
   - Module 3: 5–10 clue fragments
   - Module 4: 4–6 notebook sections
   - Module 5: 4 × 10 = 40 auth rows

---

## Semantic Model Design

- Use **DirectLake** mode over the warehouse tables
- Include all Module 1 fact tables and Module 5 auth tables
- **Create explicit average measures** for any numeric field used in Module 1 visuals (gauge, table). Do NOT rely on implicit aggregation — always create a named DAX measure. Example:

```dax
Avg Weight = AVERAGE(FactCargoBarrel[WeightKg])
```

- Create a `Launch Status` (or themed equivalent) DAX measure:

```dax
VAR Code1 = SELECTEDVALUE(AuthModule1[{Column1}])
VAR Code2 = SELECTEDVALUE(AuthModule2[{Column2}])
VAR Code3 = SELECTEDVALUE(AuthModule3[{Column3}])
VAR Code4 = SELECTEDVALUE(AuthModule4[{Column4}])
RETURN
IF(
    Code1 = "{CODE1}" && Code2 = "{CODE2}" && Code3 = "{CODE3}" && Code4 = "{CODE4}",
    "🎉 {VICTORY MESSAGE}",
    "❌ {DENIED MESSAGE}"
)
```

- Relationships are NOT needed — each puzzle uses independent tables

### Validation Checks

After creating the semantic model, verify:

1. **Schema match:** Confirm that every table and column in the semantic model matches the source warehouse tables. If columns were added or renamed in the warehouse after the model was created, the model must be updated to reflect those changes. Mismatched schemas cause blank visuals or errors in reports.
2. **Explicit measures exist:** Confirm that all average/aggregate measures needed by Module 1 visuals (gauge, summary table) are created as named DAX measures in the model — not left as implicit aggregations.

---

## Documentation Generation

After creating all Fabric items, generate three documents and provide them to the user:

### 1. Setup Guide
Step-by-step instructions for the team to complete in the Fabric UI:

**Report Theme JSON:**
- Provide the full theme JSON block
- Include step-by-step instructions: open Notepad, paste the JSON, File → Save As, choose "All Files (*.*)", save as `.json` file
- Warn users to ensure the file ends in `.json`, not `.json.txt`

**Module 1 Report (Data Anomaly):**
- Connecting to the semantic model: Go to **Get Data → Power BI semantic models** and select the model
- Applying the custom theme: Go to **View** tab → expand **Themes** dropdown → **Browse for themes** → select the saved `.json` file
- Page titles: Use **Insert → Text box** for each page title with themed font size and color
- Slicer configuration: After adding the field, go to **Format → Slicer settings → Style** and choose **Tile**
- Gauge configuration: Add the explicit average measure as the value field — Min, Max, and Target auto-scale from the data (cannot be set manually)
- Page 1 table: Use the entity field and the explicit average measure (e.g., `CargoSection` + `Avg Weight`) — this shows a summary per entity, not raw detail rows. The drillthrough page shows the raw rows.
- Drillthrough setup: On the drillthrough page, go to **Visualizations** pane → **Drill through** section → drag the filter field into the drill through well
- Navigation hint: Add a text box on Page 1 explaining that players should right-click a table row and select "Drill through → {PageName}"
- Note that all entities have diagnostic codes (decoys) — the puzzle requires identifying the anomaly, not just finding a code

**Module 2 RTI Dashboard:**
- RTI Dashboard tile specifications (bar chart + table KQL queries)

**Module 5 Report (Final Escape):**
- Same Get Data and theme instructions as Module 1
- Slicer style: Dropdown
- Status card with conditional formatting

**Data Agent (Module 3):**
- Open the Data Agent item in the workspace
- **Adding data sources:** Click **+ Add data source** in the Data Agent configuration. Add:
  1. The Warehouse (`{GameName}DW`) — select all tables
  2. The KQL Database (`{GameName}EH`) — select all tables
  - This gives the AI character access to query all game data when players ask questions
- **AI Instructions:** Always generate a full AI instructions block and include it in the setup guide. In the Data Agent, paste the instructions into the **Instructions** field. The instructions must include:
  1. **Character persona** — name, backstory, speaking style, atmospheric effects
  2. **Personality rules** — themed dialect, flavor text, emotional tone
  3. **Code protection rules** — NEVER reveal codes directly; give cryptic hints and riddles instead
  4. **Progressive hint rules** — after 5+ messages give stronger hints; after 10+ messages point players toward the right table/fragment
  5. **Game context** — brief summary of all 5 modules so the AI can reference other puzzles if asked
  6. **Data awareness** — the AI knows about all warehouse and eventhouse tables and should query them when asked about game data
- **Sharing:** After configuring, click **Share** on the Data Agent and copy the shareable link — this link is needed for the OrgApp configuration

**OrgApp configuration** (theme color, overview story text, module sections with items)

### 2. Play Guide (No Spoilers)
For players — NO codes, NO direct answers:
- The story hook and setting
- What each module looks like and what to do
- Hint format (e.g., "look for the anomaly", "find the gap", "ask the AI")
- Code format per module (e.g., "LAB-XXXX")
- How to use the final escape module

### 3. Answer Key (Game Admin Only)
- All 4 codes with discovery method
- Which entity/window/fragment contains each code
- Data verification queries

---

## Report Theme Template

Generate a themed JSON color palette. The theme should include:
- `dataColors`: 8 accent colors matching the theme
- `background`: dark base color for the theme
- `foreground`: light readable text color
- `tableAccent`: primary accent color
- `good`/`neutral`/`bad`: green/amber/red equivalents
- `visualStyles`: page background, visual background, title font, label colors

The theme must work with Power BI Desktop (File → View → Themes → Browse for themes).

---

## OrgApp Structure

```
Overview (landing page)
  → Title: themed emergency/alert title
  → Body: story text + per-module briefing descriptions

Section: "{Location Name}" (e.g., "Station Modules", "Mansion Rooms", "Lab Sectors")
  → Module 1: Report item
  → Module 2: RTI Dashboard item
  → Module 3: Link (new tab) → Data Agent shareable link
  → Module 4: Notebook item
  → Module 5: Report item
```

Theme color: set to match the report theme's background color.

---

## Extra Credit Modules

If the team finishes early, suggest these optional extensions:

### Extra Credit 1: Additional Puzzle Module
- Add a 5th investigation module (e.g., find the saboteur/traitor/mole)
- New warehouse tables: access logs, schedules, alerts
- New report with investigation visuals
- Update the Data Agent to hint at this module
- Can be standalone (fun extra) or mandatory (5th code required for escape)

### Extra Credit 2: Real-Time Streaming Telemetry
- Create a telemetry generator notebook that sends declining readings (battery, temperature, etc.)
- Create an Eventstream with a Custom Endpoint source
- Route data to the Eventhouse
- Add RTI Dashboard tiles showing live gauges
- Creates genuine urgency — players see readings dropping while they play

---

## Workspace & Capacity Notes

- The user's workspace is pre-created and assigned to a shared capacity
- Users are Contributors or Members (NOT capacity admins)
- Do NOT attempt to create workspaces or assign capacity — the workspace already exists
- Ask the user for their workspace name and discover its ID via the Fabric REST API
- All items are created inside the provided workspace
