# INIT - Encounter Tracker for Starfinder 2e

A self-contained, browser-based combat initiative tracker for **Starfinder 2e**.

No server, no install, no accounts, just one HTML file.

## Contents

- [Overview](#overview)
- [Getting Started](#getting-started)
- [GM View](#gm-view)
- [Player View](#player-view)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Community Use Notice](#community-use-notice)

---

## Overview

The tracker is a single file (`init-tracker.html`) that contains both the GM control panel and the player view. The player view opens as a popup window directly from the GM interface, with both windows staying in sync via the browser's `localStorage`.

| View        | How to access                                                          |
| ----------- | ---------------------------------------------------------------------- |
| GM view     | Open `init-tracker.html` in your browser                               |
| Player view | Click the **⧉ Player View** button in the GM header to open as a popup |

The player view is designed to be shown on a secondary display facing the table (it includes a **Flip** button to rotate 180° for across-the-table use).

---

## Getting Started

### Requirements

- Any modern browser (Chrome, Firefox, Edge, Brave, Safari)
- I use it with Obsidian using plug-ins like Surfing & Second Window

### Setup

1. Download `init-tracker.html` from this repository
2. Open `init-tracker.html` in your browser — this is your GM screen
3. Click **⧉ Player View** in the header to open the player view as a popup
4. Drag the popup to a secondary monitor or display facing your players
5. That's it — no configuration required

> **Note:** If the popup is blocked, allow popups for the file in your browser settings. Both windows must be open in the same browser on the same device for sync to work.

---

## GM View

<img width="1481" height="1259" alt="image" src="https://github.com/user-attachments/assets/54a576c7-f07f-4b3d-aff7-eb0725dc3a2c" />

### Adding Combatants

Use the **Add Combatant** panel on the right sidebar. Fill in:

- **Name**: displayed on both views (mandatory)
- **Type**: PC, Enemy, or Ally (controls card colour coding)
- **Initiative**: enter the roll result or the modifier, entering modifier will auto-roll initiative when added
- **Max HP**: enables the HP tracker and health glow indicator
- **AC**: displays armour class beneath the HP tracker
- **Notes**: short freeform note shown on the GM card

Press **Add** or hit **Enter** to add the combatant to the list.

### Running Combat

Click **Start Combat** to begin. Combatants are automatically sorted by initiative. Use the combat controls bar at the top of the list:

| Control                        | Action                                                       |
| ------------------------------ | ------------------------------------------------------------ |
| **Next Turn ▶**                | Advance to the next combatant                                |
| **◀ Prev Turn**                | Step back to the previous combatant                          |
| **+** / **−** (Round)          | Manually increment or decrement the round counter            |
| **👁 Show All** / **Hide All** | Toggle all combatants' visibility on the player view at once |

### Managing HP

Each combatant card with a Max HP set displays:

- Current HP and Max HP
- A colour-coded health bar (green → amber → red)
- An **amt** input field for entering damage or healing amounts

| Input                           | Action                             |
| ------------------------------- | ---------------------------------- |
| Type amount → **Enter**         | Deal damage                        |
| Type amount → **Shift+Enter**   | Heal                               |
| **DMG** button                  | Deal damage using the amount field |
| **HEAL** button                 | Heal using the amount field        |
| **Escape** (while in amt field) | Clear the amount field             |

When a PC or Ally's HP is reduced to 0, the Dying mechanic triggers automatically (see below). Enemies are marked as **Defeated** directly.

### Dying Mechanic (PC & Ally)

When a PC or Ally reaches 0 HP, or when the GM clicks the **skull** button on their card, the dying mechanic activates:

- HP is set to 0 and the **Dying** and **Unconscious** conditions are applied automatically
- A `DYING [−] ○ ○ ○ ○ [+]` indicator appears above the CONDITIONS header, showing the current dying level as filled pills
- The starting dying value is **1 + Wounded**
- **Doomed N** disables the last N circles, making death trigger sooner
- If the starting dying value would immediately fill all active circles, the combatant dies instantly

Use the **[−]** and **[+]** buttons to adjust the dying level each round:

| Action                    | Result                                                               |
| ------------------------- | -------------------------------------------------------------------- |
| **[+]** fills all circles | Combatant dies — card is marked defeated                             |
| **[−]** reduces to 0      | Dying removed — Wounded incremented (or applied at 1 if not present) |
| Heal above 0 HP           | Dying and Unconscious removed — Wounded incremented automatically    |

While a combatant is dying, the skull button is replaced by a **Heroic Recovery** button (heart icon). Clicking it removes the Dying condition without applying or incrementing Wounded.

### Restoring Defeated Combatants

When any combatant (PC, Ally, or Enemy) is marked as defeated, the skull button is replaced by a **Restore** button (circular arrow icon). Clicking Restore clears the defeated state:

- **PC / Ally**: defeated flag is cleared; HP and conditions remain as-is for the GM to manage
- **Enemy**: HP is restored to the value it held at the moment they were marked defeated

### Death-Warning Indicators

When a PC or Ally's **Wounded** and **Doomed** conditions combine such that applying Dying would cause instant death (i.e. `1 + Wounded ≥ 4 − Doomed`), those condition pills are highlighted in **bold red** as a warning to the GM. The same warning appears on the player view.

### Defeated Card Lockout

When a combatant is defeated, all interactive controls on their card are disabled and dimmed, leaving only the **Restore** button and the **Remove** button active. This prevents accidental edits to a dead combatant's state.

### Conditions

Click **+ condition** on any combatant card to open the condition picker. All standard SF2e conditions are listed. Clicking a valued condition (e.g. Frightened, Sickened) pre-fills the value field - adjust and press **Add** or **Enter** to apply.

- **Bundled conditions** (e.g. Grabbed applies Off-Guard and Immobilised) are shown as attached rows beneath their parent pill
- **Overridden conditions** (e.g. Blinded overriding Dazzled) are shown with strikethrough and can still be removed with ✖
- **Persistent Damage** is the only condition that can be stacked multiple times
- Valued conditions can be stepped up or down with the **−** / **+** buttons on the pill
- Clicking any listed condition will pop-up description of what that condition does with links to aonsrd for the conditions with complex rules

### Effects Summary

When active conditions have stat modifiers (e.g. Off-Guard applies −2 AC), a compact **effects summary** bar appears beneath the HP block, listing each affected stat with its net modifier. AC adjustments will automatically apply against the AC captured for that combatant.

### Visibility

Each combatant card has an eye icon button to toggle whether that combatant appears on the player view. Hidden combatants are shown on the GM view with a dimmed indicator but are completely absent from the player view. Enemies are added to the list hidden by default.

### Saving & Loading

- **Save Encounter** - downloads a `.json` file of the full session state
- **Load Encounter** - restores a previously saved `.json` file

Session state is also **auto-saved** in the browser so accidental refreshes automatically restore your last session.

### Undo

The GM view supports single-level undo. Every state-mutating action (adding/removing combatants, HP changes, condition changes, turn advances, etc.) can be reversed with:

- The **↩ Undo** button in the header
- **Ctrl+Z** from anywhere on the page

### Combat Log

The combat log records all significant events during the encounter. Damage and healing entries use an arrow format to show the transition clearly:

- **Damage**: `Billy took 15 DMG (50 → 35)`
- **Healing**: `Bobby healed 12 HP (18 → 30)`

---

## Player View

<img width="1050" height="466" alt="image" src="https://github.com/user-attachments/assets/c4d63a1a-969d-4037-b3bd-ca68d36fdada" />

The player view opens as a resizable popup window when the GM clicks **⧉ Player View** in the header. It updates automatically whenever the GM makes a change. It shows:

- A horizontal scrolling initiative track, one card per visible combatant
- Each card displays: combatant name (colour-coded by type), initiative badge, HP health glow, active conditions, and the effects summary
- The **active combatant** is highlighted with an **▶ Active** marker
- The view automatically scrolls to keep the active combatant in view
- A round counter and **Live** / **Waiting** status indicator in the top-left corner

When a PC or Ally is dying, a **pulsing skull icon** appears to the right of their name. The numeric dying value is intentionally hidden from players — only the GM sees the exact level. The **death-warning** highlight on Wounded and Doomed pills is visible on the player view so players can see the danger without GM intervention.

### Flip Window

The **Flip** button in the corner tile rotates the entire view 180°, useful when the display is facing players across the table.

---

## Keyboard Shortcuts

| Shortcut      | Action                                                    |
| ------------- | --------------------------------------------------------- |
| `N` / `→`     | Next turn                                                 |
| `P` / `←`     | Previous turn                                             |
| `Enter`       | Deal damage (in HP field) / Add combatant (in name field) |
| `Shift+Enter` | Heal combatant (in HP field)                              |
| `Escape`      | Clear HP field / close modal / cancel edit                |
| `Ctrl+Z`      | Undo last action                                          |

Shortcuts `N`, `P`, and arrow keys are disabled when an input field is focused so they don't conflict with typing.

---

## Community Use Notice

This tool uses trademarks and/or copyrights owned by Paizo Inc., used under [Paizo's Community Use Policy](https://paizo.com/licenses/communityuse). This tool is not published, endorsed, or specifically approved by Paizo. For more information about Paizo Inc. and Paizo products, visit [paizo.com](https://paizo.com).
