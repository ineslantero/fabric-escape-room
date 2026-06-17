# 🎮 Escape Room Game Creator — Getting Started

This guide walks you through creating a complete Fabric escape room game using GitHub Copilot. The game includes 5 puzzle modules built on Fabric items (Warehouse, Eventhouse, Semantic Model, Notebook, Data Agent, and OrgApp).

**What's in this folder:**

| File | Purpose |
|------|---------|
| [CREATION-INSTRUCTIONS.md](CREATION-INSTRUCTIONS.md) | This file — setup steps and the game creation prompt |
| [AGENTS.md](AGENTS.md) | Blueprint that tells Copilot how to build escape room games |
| [EXAMPLE-THEMES.md](EXAMPLE-THEMES.md) | Theme ideas with story hooks for inspiration |

---

## Prerequisites

- A **Microsoft Fabric workspace** with capacity assigned (F64 or higher recommended)
- **Contributor** or **Member** role on the workspace
- **Power BI Pro** license (required to create and publish reports)
- **Power BI Desktop** installed ([download](https://powerbi.microsoft.com/desktop))
- **GitHub Copilot** license (individual, business, or enterprise)

---

## Step 1: Set Up Your Environment

> ⚠️ **Important:** This repo and [microsoft/skills-for-fabric](https://github.com/microsoft/skills-for-fabric) do **not** auto-combine. Copilot only sees a repo's `AGENTS.md` when that repo is open in VS Code. To build the game you need **both** repos open in the same VS Code window.

### 1. Clone both repos as siblings

Pick a parent folder (e.g. `C:\Repos\` or `~/repos`). You will end up with this layout:

```
repos/
├── fabric-escape-room/      ← this repo (the game blueprint)
└── skills-for-fabric/       ← Microsoft Fabric authoring skills
```

In **VS Code**:

1. Open the **Source Control** view (the branch icon in the left activity bar, or **View → Source Control** in the menu bar).
2. Click **Clone Repository**.
3. Paste `https://github.com/ineslantero/fabric-escape-room` into the URL field at the top and press Enter.
4. In the folder picker, choose your parent folder (e.g. `C:\Repos`) and click **Select as Repository Destination**.
5. When the popup asks "Would you like to open the cloned repository?", click **Cancel** — you'll open both together in the next step.
6. Repeat steps 1–4 for the Fabric skills:
   - Click **Source Control → Clone Repository** again
   - Paste `https://github.com/microsoft/skills-for-fabric`
   - Select the **same** parent folder you used for the first clone
7. Again, click **Cancel** when asked to open the repository.

> **Already cloned `skills-for-fabric`?** Skip step 6. Instead, open your existing copy in VS Code, click the **Synchronize Changes** button at the bottom-left status bar (the circular arrows next to the branch name) to pull the latest skills, then continue.

### 2. Open both repos in a multi-root workspace

So Copilot picks up the game blueprint **and** the Fabric skills at the same time:

1. In VS Code, click **File → Open Folder…** in the top menu bar, then select your `fabric-escape-room` folder and click **Select Folder**.
2. Click **File → Add Folder to Workspace…**, then select your `skills-for-fabric` folder and click **Add**.
3. Click **File → Save Workspace As…**, name it (e.g. `escape-room.code-workspace`) and save it anywhere you like. Open this file next time you want to come back.

Both `AGENTS.md` files (the game blueprint here, and the Fabric authoring skills there) are now in Copilot's context whenever you chat in this workspace.

### 3. Open Copilot Chat in Agent mode

- In the top-right of the VS Code window, click the **Copilot** icon (or click **View → Chat** in the menu bar) to open the Copilot Chat panel.
- At the top of the chat panel, click the **mode dropdown** (it usually says "Ask" by default) and select **Agent**.
- If Copilot offers to "check for skills-for-fabric updates" at session start, click **Cancel** / decline — you don't need it for this exercise.

---

## Step 2: Create Your Game

Copy the prompt below. Replace the `[BRACKETED]` sections with your choices (or leave them blank and let Copilot come up with them), then paste it into GitHub Copilot.

See [EXAMPLE-THEMES.md](EXAMPLE-THEMES.md) for theme ideas and inspiration.

---

```
Create a Fabric escape room game in my workspace called [YOUR WORKSPACE NAME].

THEME: [YOUR THEME — e.g., "haunted mansion", "biolab outbreak", "pirate ship"]

GAME NAME: [SHORT NAME — e.g., "Ravencrest Manor", "Lab Zero", "The Crimson Tide"]

STORY: [2-3 SENTENCES — What happened? Why are players trapped? What's the urgency?
Example: "An asteroid hit our space station. Emergency power is failing. We need to find 4 codes to launch the escape shuttle before power runs out."]

AI CHARACTER NAME: [NAME OF THE AI/GHOST/GUIDE — e.g., "ODIN", "Lady Ravencrest", "NEXUS", "Polly the Parrot"]

AI CHARACTER PERSONALITY: [1-2 SENTENCES — How does the AI talk?
Example: "Damaged space station AI. Clinical but glitchy. Inserts [STATIC] into responses. Cares about crew survival."]

MODULE IDEAS (optional — or let Copilot generate them):
1. [MODULE 1 — the data anomaly puzzle, e.g., "oxygen recyclers failing"]
2. [MODULE 2 — the pattern gap puzzle, e.g., "debris radar with one safe window"]
3. [MODULE 3 — the AI conversation puzzle, e.g., "decode intercepted signal fragments"]
4. [MODULE 4 — the notebook discovery puzzle, e.g., "engine diagnostic terminal"]

Build the complete game following the escape room framework. Use these Fabric skills for each item:

- **Warehouse** (use the `sqldw-authoring-cli` skill) — puzzle data plus 4 AuthModule tables (10 codes each, 1 correct + 9 decoys)
- **Eventhouse + KQL Database** (use the `eventhouse-authoring-cli` skill) — time-series data for the pattern gap puzzle (~150–200 activity rows across 50+ time windows with one gap, and ~50 assessment windows with decoy codes)
- **Lakehouse + Notebook** (use the `spark-authoring-cli` skill) — Lakehouse as storage layer for the semantic model, plus a notebook with themed diagnostic output (4–6 sections, one showing the code)
- **Semantic Model** (use the `semantic-model-authoring` skill) — DirectLake TMDL with explicit average measures and a victory/denied DAX measure

Do **NOT** create the Data Agent, OrgApp, Power BI reports, or RTI Dashboard via Copilot — those are configured manually in the Fabric portal using the Setup Guide you generate.

After creating the items above, generate documentation **split into multiple files** so different team members can work in parallel:
1. A **`setup-guide/`** folder with one markdown file per workstream — index `README.md` (role assignments + dependency order), `00-SHARED-SETUP.md` (sign-in + theme JSON), `01-MODULE1-REPORT.md`, `02-MODULE2-DASHBOARD.md`, `03-MODULE3-DATA-AGENT.md`, `04-MODULE5-REPORT.md`, `05-ORGAPP.md`
2. A top-level **`PLAY-GUIDE.md`** for players (no spoilers, just hints and code formats)
3. A top-level **`ANSWER-KEY.md`** with all 4 codes and how to find them
```

---

## Step 3: Follow the Setup Guide

After Copilot creates the Fabric items, it generates a **`setup-guide/` folder** with one markdown file per workstream, plus a top-level `PLAY-GUIDE.md` and `ANSWER-KEY.md`. Different team members can pick up different files in parallel.

Open `setup-guide/README.md` first — it lists owner roles, dependency order, and a suggested team split. Typical 3-person split:

- **Power BI builder** — `01-MODULE1-REPORT.md`, `02-MODULE2-DASHBOARD.md`, `04-MODULE5-REPORT.md`
- **Data Agent owner** — `03-MODULE3-DATA-AGENT.md` (independent — can run in parallel)
- **OrgApp owner** — `05-ORGAPP.md` (waits for the others to publish their items)

Everyone reads `00-SHARED-SETUP.md` first (sign-in to Power BI Desktop and Fabric, save the theme JSON).

Once all five files are done, end-to-end test the OrgApp link as a player would: enter the four codes in Module 5 and confirm the victory message.

---

## Troubleshooting

| Issue | Solution |
|-------|---------|
| "I can't create items" | Check workspace role is Contributor or Member |
| "Report won't publish" | User needs Power BI Pro license |
| Data Agent not responding | Verify data sources are connected and agent is published |
| Gauge shows no data | Ensure the semantic model has an explicit average measure (not implicit aggregation) |
| Drillthrough page shows all rows | Check that the drillthrough field is set in Visualizations → Drill through |
| Custom theme won't apply | Must use Power BI Desktop → View → Themes → Browse for themes |
| Report fields are blank/error | Semantic model schema may not match warehouse tables — refresh the model |
