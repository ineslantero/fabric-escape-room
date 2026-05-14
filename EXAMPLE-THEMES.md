# 🎲 Example Themes for Escape Room Games

Pick a theme below or invent your own! Each theme includes a setting, story hook, and suggested module ideas. When filling in the [CREATION-INSTRUCTIONS.md](CREATION-INSTRUCTIONS.md), use these as inspiration.

---

## 🏚️ The Haunted Mansion

**Setting:** A crumbling Victorian manor on a stormy night.

**Story hook:** You're paranormal investigators trapped inside Ravencrest Manor after the front door sealed itself shut. The mansion's ghost, Lady Ravencrest, has scattered four enchantment codes across the house. Find them all to break the binding spell and unlock the front door before midnight.

**Module ideas:**
| Module | Puzzle | Fabric Item |
|--------|--------|-------------|
| 🕯️ The Library | Analyze a catalog of cursed books — one has an abnormal energy reading with a spell code | Report (table + gauge) |
| 🪞 The Mirror Gallery | Study ghostly apparition sighting times — find the one moment with zero activity (the portal opens) | RTI Dashboard |
| 🔮 The Séance Room | Talk to Lady Ravencrest's spirit (AI agent) to decode her cryptic messages | Data Agent |
| ⚗️ The Alchemy Lab | Read a potion recipe notebook — one ingredient is marked as the key | Notebook |
| 🚪 The Front Door | Enter all four spell codes to break the binding and escape | Report (slicers + status) |

---

## 🔬 Biolab Outbreak — Code Red

**Setting:** A high-security biolab has gone into lockdown after a containment breach.

**Story hook:** Pathogen X-47 has escaped containment in Lab 5. The facility AI (NEXUS) has initiated emergency lockdown. You have 60 minutes before the decontamination protocol floods the entire building with UV sterilization — lethal to humans. Find the four override codes to reach the emergency elevator.

**Module ideas:**
| Module | Puzzle | Fabric Item |
|--------|--------|-------------|
| 🧬 Containment Lab | Analyze pathogen samples — one vial has abnormal mutation rate, its ID is the code | Report (table + slicer) |
| 🌡️ Climate Control | Monitor temperature sensors across rooms — find the one room where cooling failed (the breach point) | RTI Dashboard |
| 🤖 NEXUS Terminal | Chat with the facility AI about the contamination timeline and patient zero | Data Agent |
| 🔬 Research Notes | Read Dr. Chen's experiment log notebook — one entry contains the security override | Notebook |
| 🛗 Emergency Elevator | Enter all four override codes to activate the elevator and escape | Report (slicers + status) |

---

## 🏴‍☠️ The Sinking Ship — Blackbeard's Revenge

**Setting:** A pirate ship taking on water after a naval battle.

**Story hook:** Captain Blackbeard's ship "The Crimson Tide" has been hit by cannonballs. The hull is breached and water is rising. The lifeboat is locked with four combination codes hidden across the ship. Find them before the ship sinks!

**Module ideas:**
| Module | Puzzle | Fabric Item |
|--------|--------|-------------|
| ⚓ The Cargo Hold | Inventory of cargo barrels — one is leaking gunpowder (abnormal weight), its tag number is the code | Report (table + gauge) |
| 🗺️ The Navigation Room | Study the ship's course through reef zones — find the one safe passage with zero reef contacts | RTI Dashboard |
| 🦜 The Captain's Parrot | Talk to Polly (AI agent) who repeats fragments of the captain's secret orders | Data Agent |
| 📜 The Captain's Log | Read the captain's journal entries — one page reveals where treasure (and codes) are buried | Notebook |
| 🚣 The Lifeboat | Enter all four combination codes to unlock the lifeboat and escape | Report (slicers + status) |

---

## 🏦 The Vault — Last Heist

**Setting:** Inside a high-tech bank vault during a heist gone wrong.

**Story hook:** Your crew broke into Meridian National Bank, but the vault door sealed behind you and the silent alarm triggered. You have 45 minutes before the police arrive. The emergency exit requires four security codes from the vault's subsystems. Crack them and get out clean.

**Module ideas:**
| Module | Puzzle | Fabric Item |
|--------|--------|-------------|
| 💰 Safety Deposit Boxes | Analyze the box registry — one box has an anomalous insurance value, its ID is the code | Report (table + slicer) |
| 📹 Security Camera Feed | Review camera activity logs — find the one blind spot window with zero footage | RTI Dashboard |
| 🤖 ATLAS (Vault AI) | Chat with the vault's AI about security protocols and override procedures | Data Agent |
| 💻 The Server Room | Read the IT diagnostic terminal — one firewall port is open, its ID is the code | Notebook |
| 🚪 Emergency Exit | Enter all four security codes to unlock the emergency tunnel | Report (slicers + status) |

---

## 🌋 Volcano Base — Operation Meltdown

**Setting:** A villain's secret base inside an active volcano.

**Story hook:** You're a spy trapped in Dr. Scorpio's volcanic lair. The self-destruct sequence has been activated — lava is rising through the lower levels. Find four abort codes scattered across the base's control systems to reach the escape helicopter on the roof.

**Module ideas:**
| Module | Puzzle | Fabric Item |
|--------|--------|-------------|
| 🔥 Lava Monitoring | Analyze thermal sensors — one vent shows abnormal cooling (a hidden passage), its code unlocks the next level | Report (table + gauge) |
| 🗺️ Base Schematics | Study the ventilation system — find the one shaft with zero heat (safe crawl path) | RTI Dashboard |
| 🦹 Dr. Scorpio's AI | Talk to VIPER, the base AI, who reluctantly reveals clues under interrogation | Data Agent |
| 💣 Weapons Lab | Read the munitions inventory notebook — one prototype has a defect code that doubles as a key | Notebook |
| 🚁 Helipad | Enter all four abort codes to disable self-destruct and reach the helicopter | Report (slicers + status) |

---

## 🚂 The Orient Express — Murder at Midnight

**Setting:** A luxury train crossing Europe in the 1930s.

**Story hook:** A passenger has been found dead in Car 7. The train will reach the next station in 60 minutes, where the murderer will escape into the crowd. You're the detective — find four evidence codes from different carriages to identify and confront the killer before the train stops.

**Module ideas:**
| Module | Puzzle | Fabric Item |
|--------|--------|-------------|
| 🍽️ The Dining Car | Analyze dinner orders and alibis — one passenger's timeline doesn't add up | Report (table + slicer) |
| 🚂 Ticket Records | Review passenger boarding times and car movements — find the suspicious gap | RTI Dashboard |
| 🎩 The Porter | Chat with Jacques the porter (AI) who overheard conversations and saw suspicious behavior | Data Agent |
| 📋 Forensic Report | Read the coroner's preliminary notes — cause of death reveals a key clue | Notebook |
| 🔍 The Accusation | Select the murderer, weapon, motive, and opportunity from four evidence slicers | Report (slicers + status) |

---

## 💡 Create Your Own Theme

Any theme works as long as you can map it to the 5-module pattern:

| Module # | Role | Fabric Item | Puzzle Pattern |
|----------|------|-------------|---------------|
| 1 | **Data anomaly** — find the outlier | Power BI Report | Slicer → gauge/table → drillthrough reveals code |
| 2 | **Pattern gap** — find what's missing | RTI Dashboard | Bar chart with one gap → table reveals code |
| 3 | **AI conversation** — extract info through dialog | Data Agent | Ask the right questions to get the code |
| 4 | **Read & find** — locate the code in output | Notebook | Scan diagnostic output for the code |
| 5 | **Final escape** — combine all 4 codes | Power BI Report | 4 slicers → status measure → victory! |
