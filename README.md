# INIT - Encounter Tracker for Starfinder 2e

A self-contained, browser-based combat initiative tracker for **Starfinder 2e**.

No server, no install, no accounts, just two HTML files.

---

## Overview

The tracker is split into two views that sync in real time via the browser's `localStorage`:

| File               | Purpose                                                                             |
| ------------------ | ----------------------------------------------------------------------------------- |
| `gm-view.html`     | Full GM control panel - add combatants, manage HP, apply conditions, advance turns  |
| `player-view.html` | Read-only player display - shows the initiative order with live updates from the GM |

The player view is designed to be shown on a secondary display facing the table (it includes a **Flip Window** button to rotate 180° for across-the-table use).

---

## Getting Started

### Requirements

- Any modern browser (Chrome, Firefox, Edge, Safari)
- Both files must be open **in the same browser** on the same machine (they communicate via `localStorage`)
- I use it with Obsidian using plug-ins like Surfing & Second Window

### Setup

1. Download `gm-view.html` and `player-view.html` from this repository
2. Open `gm-view.html` in one browser tab - this is your GM screen
3. Open `player-view.html` in a second browser tab or window - drag this to a secondary monitor or display facing your players
4. That's it - no configuration required

> **Note:** The two files must be open in the same browser on the same device. They will not sync across different machines or browsers.

---

## GM View

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

When a combatant's HP is reduced to 0 by damage, they are automatically marked as **Defeated**.

### Conditions

Click **+ condition** on any combatant card to open the condition picker. All standard SF2e conditions are listed. Clicking a valued condition (e.g. Frightened, Sickened) pre-fills the value field - adjust and press **Add** or **Enter** to apply.

- **Bundled conditions** (e.g. Grabbed applies Off-Guard and Immobilised) are shown as attached rows beneath their parent pill
- **Overridden conditions** (e.g. Blinded overriding Dazzled) are shown with strikethrough and can still be removed with ✖
- **Persistent Damage** is the only condition that can be stacked multiple times
- Valued conditions can be stepped up or down with the **−** / **+** buttons on the pill
- Clicking any listed condition will pop-up description of what that condition does

### Effects Summary

When active conditions have stat modifiers (e.g. Off-Guard applies −2 AC), a compact **effects summary** bar appears beneath the HP block, listing each affected stat with its net modifier colour-coded green (bonus) or red (penalty). AC adjustments will automatically apply against the AC captured for that combatant

### Visibility

Each combatant card has an eye icon button to toggle whether that combatant appears on the player view. Hidden combatants are shown on the GM view with a dimmed indicator but are completely absent from the player view. Enemies are added to the list hidden by default.

### Saving & Loading

Use the **Encounter** section in the sidebar to:

- **Save Encounter** - downloads a `.json` file of the full session state
- **Load Encounter** - restores a previously saved `.json` file

Session state is also **auto-saved** in the browser so refreshing the GM view restores your last session automatically.

### Undo

The GM view supports single-level undo. Every state-mutating action (adding/removing combatants, HP changes, condition changes, turn advances, etc.) can be reversed with:

- The **↩ Undo** button in the header
- **Ctrl+Z** from anywhere on the page

---

## Player View

The player view updates automatically whenever the GM makes a change. It shows:

- A horizontal scrolling initiative track, one card per visible combatant
- Each card displays: combatant name (colour-coded by type), initiative badge, HP health glow, active conditions, and the effects summary
- The **active combatant** is highlighted with an **▶ Active** marker
- The view automatically scrolls to keep the active combatant in view
- A round counter and **Live** / **Waiting** status indicator in the top-left corner

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

---

## Technical Notes

- **No dependencies** - everything is self-contained in the two HTML files. Fonts are loaded from Google Fonts (requires an internet connection on first load; they cache after that).
- **Sync mechanism** - the GM view writes state to `localStorage` under the key `init_player_view` on every change. The player view listens for the `storage` event and re-renders immediately. This means both files must be open in the same browser on the same device.
- **No build step** - the files can be edited directly in any text editor.
