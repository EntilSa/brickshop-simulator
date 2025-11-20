# Brickshop – Planning Overview

This document summarises the current planning state of my solo game project **“Brickshop Simulator”**.

For the full interactive boards, see the public Notion page:  
👉 [[Brickshop – Planning & Roadmap](HIER-DEIN-NOTION-LINK)](https://held-spice-b90.notion.site/Brickshop-Planning-Roadmap-2b12669e635880e9b86cd8df066a4c02)

---

## 1. Project Vision (short)

- **Theme:** Outlet shop for fictional brick sets (Lego-like, but legally safe)  
- **Gameplay:** Buy rest stock and returns, sort and repair sets, manage shelves and prices  
- **Economy:** Some sets go **Out of Print (OOP)** and can become rare and valuable over time  
- **Target feel:** Mix of cozy shop sim and deeper tycoon mechanics  
- **Audience:** Players who enjoy chill shop-management games *plus* people who like deeper economic systems.

---

## 2. Game Pillars

1. **Rest stock & risk**  
   Deliveries are messy: mixed pallets, damaged boxes, incomplete sets. The player has to *assess* and *process* stock instead of just clicking “buy 100 units”.

2. **Condition & repair**  
   Items have different conditions (A/B/C). The player can clean, re-box and repair sets to improve their value – or sell them as cheaper B- or C-stock.

3. **Out-of-Print value**  
   Sets have a lifecycle. Once they go **Out of Print**, they gradually become rare and more valuable, which enables long-term speculation.

4. **Cozy vs. Tycoon**  
   Separate modes or difficulty sliders: relaxed, low-pressure “cozy” mode vs. more demanding tycoon mode with tighter margins and real risk of going bankrupt.

5. **Modular systems**  
   Data-driven sets, themes and suppliers (ScriptableObjects / JSON) so future content and possible modding are easy to add without rewriting core systems.

---

## 3. Current Milestone – “First Playable”

**Goal:**  
A very simple, ugly but working 3D prototype that shows the **core economic loop**:

> One small shop, a few test sets, money going up and down, and a basic day summary.

No customers or advanced UI yet – just the internal systems.

### Planned epics for First Playable

- **Base project & greybox gameloop**
  - Create the Unity 2022 LTS project  
  - Set up a simple `Shop_Prototype` scene  
  - Add a basic `GameManager` handling start and end of a day  

- **3D shop room**
  - Build a minimal room (floor, walls, entrance) with primitives  
  - Add one simple shelf as a greybox model  
  - Position camera so shelf and room are clearly visible  

- **Set & part data model**
  - Implement a simple `BrickSet` data structure (name, category, base price, condition)  
  - Define 3–5 test sets directly in code or ScriptableObjects  

- **Shelf & stock management**
  - Create a shelf that holds multiple slots referencing `BrickSet` entries  
  - Maintain an internal list tracking which sets are placed on which shelf  

- **Money system & day summary**
  - Implement a `StoreEconomy` script with a `balance` variable  
  - Add a basic UI text element showing the current money and day  
  - At day end: show a simple summary (start balance, purchases, sales, end balance)

---

## 4. Tools & Organisation

- **Notion**
  - Kanban board for project epics (e.g. base project, economy, sets & conditions)  
  - Feature roadmap with phases (Prototype, Vertical Slice, V1.0, Longterm)  
  - Public page: *“Brickshop – Planning & Roadmap”* (link above)

- **GitHub**
  - This repository documents the planning phase first  
  - Later commits will add the Unity project and C# implementation  
  - Documentation will be updated as milestones are reached

---

## 5. Next Steps

Short-term next steps for the project:

1. Finalise the First Playable epics in Notion  
2. Set up the Unity project and basic folder structure in this repository  
3. Implement the internal economy loop (sets, shelves, money) without customers  
4. Start a lightweight devlog in `docs/devlog.md` to track progress and decisions
