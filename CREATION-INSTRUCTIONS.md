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

1. Open **VS Code**.
2. Clone this repo:
   - Press **Ctrl+Shift+P** to open the Command Palette
   - Type **Git: Clone** and select it
   - Paste the repo URL and choose a local folder
   - When prompted, click **Open** to open the cloned folder in VS Code
3. Install the **Fabric Skills** so Copilot can create Fabric items:
   - Open **GitHub Copilot Chat** (Ctrl+Shift+I)
   - Switch to **Agent** mode (click the mode dropdown at the top of the chat panel)
   - Ask Copilot to install the skills by typing:
     ```
     Install the Fabric skills from https://github.com/microsoft/skills-for-fabric
     ```
   - Follow any prompts to confirm the installation
4. The `AGENTS.md` file in this folder is the game blueprint — it tells Copilot how to structure escape room games. As long as you have this folder open in VS Code, Copilot will automatically use it as context.

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

Build the complete game following the escape room framework:
- Create a Warehouse with all puzzle data and 4 AuthModule tables (10 codes each, 1 correct + 9 decoys)
- Create an Eventhouse with time-series data for the pattern gap puzzle (~150-200 activity rows across 50+ time windows with one gap, and ~50 assessment windows with decoy codes)
- Create a Semantic Model (DirectLake TMDL) with explicit average measures and a victory/denied DAX measure
- Create a Notebook with themed diagnostic output (4-6 sections, one showing the code)
- Create a Data Agent with the AI character personality
- Create an OrgApp as the game portal

After creating everything, generate:
1. A SETUP GUIDE with: theme JSON (with Notepad save instructions), detailed report visual specs (Get Data flow, theme import, slicer styles, drillthrough setup), RTI Dashboard KQL queries, Data Agent configuration (data sources + AI instructions), OrgApp configuration
2. A PLAY GUIDE for players (no spoilers, just hints and code formats)
3. An ANSWER KEY with all 4 codes and how to find them
```

---

## Step 3: Follow the Setup Guide

After Copilot creates the Fabric items, it generates a **Setup Guide** with detailed instructions for the remaining manual configuration:

1. **Save the report theme** — Open Notepad, paste the JSON, save as `.json`
2. **Build Module 1 report** — Connect to semantic model via Get Data → Power BI semantic models, apply theme, add visuals with specific slicer/gauge/drillthrough settings
3. **Build Module 2 RTI Dashboard** — Create tiles with the provided KQL queries
4. **Configure the Data Agent** — Add data sources (Warehouse + KQL Database), paste AI instructions
5. **Build Module 5 report** — Final escape with 4 code slicers and status card
6. **Set up the OrgApp** — Configure the game portal with all modules
7. **Test end-to-end** — Verify all 4 codes and the victory condition

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
