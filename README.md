# 🎮 Fabric Escape Room

Build escape room games on **Microsoft Fabric** using **GitHub Copilot**. Give Copilot a theme, and it creates a fully playable 5-module puzzle game across Fabric services — Warehouse, Eventhouse, Semantic Model, Notebook, Data Agent, and OrgApp.

---

## How It Works

```
You pick a theme ──→ Copilot builds the Fabric items ──→ You configure reports & UI ──→ Players escape!
```

1. **Choose a theme** — haunted mansion, pirate ship, biolab outbreak, bank heist, or anything you invent
2. **Paste a prompt** into GitHub Copilot with your theme, story, and AI character
3. **Copilot creates** all the Fabric items (Warehouse, Eventhouse, Semantic Model, Notebook, Data Agent, OrgApp)
4. **Follow the generated Setup Guide** to build reports, dashboards, and configure the game portal
5. **Share the OrgApp link** — players open it and start solving puzzles

---

## Game Architecture

Every game creates these Fabric items:

```
┌─────────────────────────────────────────────────────────────┐
│                        OrgApp (Game Portal)                 │
│                                                             │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐      │
│  │ Module 1 │ │ Module 2 │ │ Module 3 │ │ Module 4 │      │
│  │  Report  │ │   RTI    │ │  Data    │ │ Notebook │      │
│  │          │ │Dashboard │ │  Agent   │ │          │      │
│  └────┬─────┘ └────┬─────┘ └────┬─────┘ └──────────┘      │
│       │             │            │                          │
│       ▼             ▼            ▼                          │
│  ┌─────────┐  ┌──────────┐  ┌─────────┐                   │
│  │Semantic │  │Eventhouse│  │Warehouse│                    │
│  │ Model   │  │   (KQL)  │  │  (SQL)  │                    │
│  └────┬────┘  └──────────┘  └────┬────┘                    │
│       │                          │                          │
│       └──────────┐  ┌────────────┘                          │
│                  ▼  ▼                                       │
│            ┌──────────────┐                                 │
│            │  Module 5    │                                 │
│            │  Final Escape│                                 │
│            │  (Report)    │                                 │
│            └──────────────┘                                 │
└─────────────────────────────────────────────────────────────┘
```

| Fabric Item | Purpose |
|-------------|---------|
| **Warehouse** | Stores puzzle data — anomaly tables, character data, authorization codes |
| **Eventhouse** | Stores time-series data — activity logs, sensor readings, sighting records |
| **Lakehouse** | Storage layer for the Semantic Model |
| **Semantic Model** | DirectLake model with DAX measures for reports |
| **Notebook** | Pre-formatted diagnostic output with one hidden code |
| **Data Agent** | AI character players chat with to discover clues |
| **OrgApp** | The game portal — players access everything through this |

---

## The 5 Modules

Every escape room follows the same 5-module structure. Players must find 4 authorization codes hidden across the first 4 modules, then combine them in Module 5 to escape.

| Module | Puzzle Type | Fabric Item | What Players Do |
|--------|------------|-------------|-----------------|
| **1. Data Anomaly** | Find the outlier | Power BI Report | Browse a dataset with a slicer — one entity has abnormal readings. Drillthrough reveals the code. All entities have decoy codes, so players must find the anomaly first. |
| **2. Pattern Gap** | Find the missing data point | RTI Dashboard | A bar chart shows activity across 50+ time windows. One window has zero activity — that's the gap. A companion table maps the gap to the correct code. |
| **3. AI Conversation** | Extract info from an AI character | Data Agent | Chat with a themed AI character (ghost, damaged AI, parrot, etc.) who gives cryptic hints. The code is hidden in a data table the AI can query. |
| **4. Read & Discover** | Find the code in formatted output | Notebook | Read through 4–6 diagnostic sections. Most show PASS/OK. One shows CRITICAL FAILURE with the hidden code. |
| **5. Final Escape** | Combine all 4 codes | Power BI Report | Four dropdown slicers (one per module) + a status card. Enter all 4 correct codes → victory message. Wrong codes → denied. |

---

## Getting Started

See [CREATION-INSTRUCTIONS.md](CREATION-INSTRUCTIONS.md) for the full step-by-step guide.

**Quick start:**

1. Clone **both** [this repo](https://github.com/ineslantero/fabric-escape-room) and [microsoft/skills-for-fabric](https://github.com/microsoft/skills-for-fabric) as sibling folders. They don't auto-combine — Copilot only sees a repo's `AGENTS.md` when that repo is open in VS Code.
2. Open both folders in a VS Code multi-root workspace (File → Open Folder, then File → Add Folder to Workspace).
3. Open Copilot Chat in **Agent mode**.
4. Customize the prompt in [CREATION-INSTRUCTIONS.md](CREATION-INSTRUCTIONS.md) with your theme and paste it into the chat.
5. Follow the generated Setup Guide to finish the game (reports, RTI dashboard, Data Agent, OrgApp).

---

## Prerequisites

- **Microsoft Fabric workspace** with capacity (F64+ recommended)
- **Contributor** or **Member** role on the workspace
- **Power BI Pro** license
- **Power BI Desktop** ([download](https://powerbi.microsoft.com/desktop))
- **GitHub Copilot** license
- **VS Code** with GitHub Copilot extension

---

## Files

| File | Purpose |
|------|---------|
| [CREATION-INSTRUCTIONS.md](CREATION-INSTRUCTIONS.md) | Step-by-step guide — environment setup, game creation prompt, and troubleshooting |
| [AGENTS.md](AGENTS.md) | Blueprint for Copilot — architecture, data schemas, puzzle patterns, validation checks |
| [EXAMPLE-THEMES.md](EXAMPLE-THEMES.md) | Theme ideas with story hooks and module suggestions |

---

## Example Themes

| Theme | Setting | AI Character |
|-------|---------|-------------|
| 🏴‍☠️ Pirate Ship | A cursed vessel heading for the reef | Captain Bones (ghost) |
| 🔬 Biolab Outbreak | High-security lab in lockdown | NEXUS (facility AI) |
| 🏚️ Haunted Mansion | A Victorian manor sealed by a spell | Lady Ravencrest (spirit) |
| 🏦 Bank Vault Heist | Trapped inside during a heist gone wrong | ATLAS (vault AI) |
| 🌋 Volcano Base | A villain's lair with rising lava | Dr. Scorpio's mainframe |
| 🚀 Space Station | Station hit by an asteroid, power failing | ODIN (damaged station AI) |

See [EXAMPLE-THEMES.md](EXAMPLE-THEMES.md) for full details with story hooks and module ideas.

---

## What Copilot Generates

After you run the prompt, Copilot creates the data and modeling layer of the game (Warehouse, Eventhouse + KQL Database, Lakehouse, Semantic Model, and the Module 4 Notebook) using the [Fabric authoring skills](https://github.com/microsoft/skills-for-fabric). It then generates documentation **split across multiple files** so the team can work in parallel:

- **`setup-guide/`** — one file per workstream so different team members can pick up reports, dashboard, Data Agent, and OrgApp independently. Includes a `README.md` index with role assignments and the dependency order.
- **`PLAY-GUIDE.md`** — Spoiler-free instructions for players (story, hints, code formats)
- **`ANSWER-KEY.md`** — All 4 codes with exact locations and verification queries (game admin only)
