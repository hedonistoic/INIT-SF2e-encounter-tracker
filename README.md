# INIT - Encounter Tracker for Starfinder 2e

A self-contained, browser-based combat initiative tracker for **Starfinder 2e**.

## Community Use Notice

This tool uses trademarks and/or copyrights owned by Paizo Inc., used under [Paizo's Community Use Policy](https://paizo.com/licenses/communityuse). This tool is not published, endorsed, or specifically approved by Paizo. For more information about Paizo Inc. and Paizo products, visit [paizo.com](https://paizo.com).

## Disclaimer: 
Generative AI technologies assisted in development of this tool.

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
| Player view | Click the **Open** button in the side pabnel to open as a popup        |

The player view is designed to be shown on a secondary display facing the table.

---

## Getting Started

### Requirements

- Any modern browser (Chrome, Firefox, Edge, Brave, Safari)
- I use it with Obsidian using plug-ins like Surfing & Second Window

### Setup

1. Download `init-tracker.html` from this repository
2. Open `init-tracker.html` in your browser — this is your GM screen
3. Click **Open** from the player controls section of the side panel to open the player view as a popup
4. Drag the popup to a secondary monitor or display facing your players
5. That's it — no configuration required

> **Note:** If the popup is blocked, allow popups for the file in your browser settings. Both windows must be open in the same browser on the same device for sync to work.

---

## GM View

<img width="1552" height="1265" alt="image" src="https://github.com/user-attachments/assets/624d919a-e172-4b71-9545-3528fa9642bc" />


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

### Managing HP

Each combatant card with a Max HP set displays:

- Current HP and Max HP
- A colour-coded health bar (green → amber → red)
- An **amt** input field for entering damage or healing amounts

| Input                           | Action                             |
| ------------------------------- | ---------------------------------- |
| Type amount → **Enter**         | Deal damage                        |
| Type amount → **Shift+Enter**   | Heal                               |
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
| Heroic Recovery           | Dying and Unconscious removed — Wounded not incremented              |

### Restoring Defeated Combatants

When any combatant (PC, Ally, or Enemy) is marked as defeated, the skull button is replaced by a **Restore** button (circular arrow icon). Clicking Restore clears the defeated state:

### Death-Warning Indicators

When a PC or Ally's **Wounded** and **Doomed** conditions combine such that applying Dying would cause instant death (i.e. `1 + Wounded ≥ 4 − Doomed`), those condition pills are highlighted in **bold red** as a warning to the GM. The same warning appears on the player view.

### Defeated Card Lockout

When a combatant is defeated, all interactive controls on their card are disabled and dimmed, leaving only the **Restore** button and the **Remove** button active. This prevents accidental edits to a dead combatant's state.

### Conditions

Click **+ condition** on any combatant card to open the condition picker. All standard SF2e conditions are listed. Clicking a valued condition (e.g. Frightened, Sickened) pre-fills the value field - adjust and press **Add** or **Enter** to apply.

- Valued conditions can be stepped up or down with the **−** / **+** buttons on the pill
- Round Remaining can be applied after the condition is added by clicking on it
- Clicking on any condition will allow notes to be captured and also explains full rules for that condition
- Bundled conditions (e.g. Grabbed applies Off-Guard and Immobilised) are shown with layer icon and clicking the condition will explain what conditions are included
- Overridden conditions (e.g. Blinded overriding Dazzled) are shown with strikethrough and can still be removed with ✖

### Effects Summary

When active conditions have stat modifiers (e.g. Off-Guard applies −2 AC), a compact **effects summary** bar appears beneath the HP block, listing each affected stat with its net modifier. AC adjustments will automatically apply against the AC captured for that combatant.

Hover over the stat to see which condition/s are causing it

### Visibility

Each combatant card has an eye icon button to toggle whether that combatant appears on the player view. Hidden combatants are shown on the GM view with a dimmed indicator but are completely absent from the player view. Enemies are added to the list hidden by default.

### Saving & Loading

- **Save Encounter** - downloads a `.json` file of the full session state
- **Load Encounter** - restores a previously saved `.json` file

Session state is also **auto-saved** in the browser so accidental refreshes automatically restore your last session.

### Undo

The GM view supports single-level undo. Every state-mutating action (adding/removing combatants, HP changes, condition changes, turn advances, etc.) can be reversed with:

- The **Undo** button in the header
- **Ctrl+Z** from anywhere on the page

### Combat Log

The combat log records all significant events during the encounter with prior values displayed in case you need to revert
---

## Player View

https://github.com/user-attachments/assets/b10fe550-d42f-4ebe-bf5d-35e8b1b3a21a

The player view stays in sync with the GM view and show:
- Auto-scrolling 'infinite' initiative track, one card per visible combatant
- Each card displays: combatant name (colour-coded by type), initiative badge, HP health glow, active conditions, and the effects summary
- The **active combatant** is highlighted with an **Active** marker
- The view automatically scrolls to keep the active combatant in view

When a PC or Ally is dying, a **pulsing skull icon** appears over their card. The numeric dying value is intentionally hidden from players, only the GM sees the exact level. The **death-warning** highlight on Wounded and Doomed pills is visible on the player view so players can see the danger.

### Player view controls
-  **Rotate** button will rotate the  entire view 180°
-  **Stretch** the transform the view along Y axis, helpful if the screen is at a sharp angle
-  **Zoom** a discrete zoom control which can offer more granularity than browser zoom

---

## Keyboard Shortcuts

| Shortcut      | Action                                                    |
| ------------- | --------------------------------------------------------- |
| `→`           | Next turn                                                 |
| `←`           | Previous turn                                             |
| `Enter`       | Deal damage (in HP field) / Add combatant (in name field) |
| `Shift+Enter` | Heal combatant (in HP field)                              |
| `Escape`      | Clear HP field / close modal / cancel edit                |
| `Ctrl+Z`      | Undo last action                                          |

---


