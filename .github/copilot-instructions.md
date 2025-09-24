# Copilot Instructions for Lili Battle Game

## Project Overview
Single-file (`index.html`) browser-based turn-based battle simulator. Lili fights 10 bosses + minions with real-time battle predictions, equipment system, and debug tools. All HTML/CSS/JS embedded in one file.

## Core Architecture
- **Lili Object:** `{atk, def, maxHp, hp, prayUsed}` with custom arrow controls
- **Battle Functions:**
  - `battleResultForBoss()` - Pure prediction for UI indicators  
  - `battleResult()` - Actual boss battle with state changes
  - `minionBattleResult()` - Minion fights preserving HP between battles
- **Enhancement System:** 16 visible checkboxes (2 hidden legacy) controlling equipment/abilities
- **Debug Panel:** Fixed overlay (`position: fixed; top: 20px; right: 10px`) with attribute offsets and logging options

## Key Battle Mechanics
- **Turn Order:** Lili attacks → Prayer check → Enemy attacks → Repeat
- **Prayer Timing:** Triggers AFTER Lili's attack, BEFORE enemy attack (changed from round start)
- **Prayer Trigger:** Fixed at 30% max HP regardless of equipment
- **Damage Formula:** `max(1, attacker.atk - defender.def)`
- **Prayer Tracking:** Use `prayUsed` counter (NOT deprecated `lili.pray`)
- **Death Checking:** Immediate checks after any HP change during battle
- **HP State:** Boss battles reset to max; minion battles preserve current HP

## Critical Patterns

### Equipment System (面板同步)
```javascript
// Reading enhancement state
const optPrayAmount = document.getElementById('opt-pray-amount').checked;

// Equipment toggle functions modify lili object + trigger UI update
function toggleGlove() {
  // Modify lili.atk, call updateUI(), trigger animations
}
```

### Debug System
- **Attribute Offsets:** `getDebuggedBoss()` and `getDebuggedMinion()` apply debug offsets to battle calculations
- **Debug Options:** "显示每回合血量" and "完整战斗记录(999轮)" checkboxes in debug panel
- **State Management:** Debug offsets stored in global variables, applied in all battle functions

### UI Updates
```javascript
// Always call after any stat/enhancement change
updateUI(); 

// Trigger blue flash animation on value changes
animateInput(element);

// Update boss win/loss predictions
updateBossIndicators();
```

## Project-Specific Conventions
- **Single-file architecture:** All logic in `index.html`, no build process needed
- **面板同步 (Panel Sync):** Equipment effects immediately update UI with animations
- **Custom Controls:** Arrow buttons (`◀▶`) for stat adjustment instead of native number inputs
- **Color-coded Equipment:** Orange/purple/blue/green backgrounds for visual hierarchy
- **Immediate Death Checks:** Check boss/enemy death after every damage source (attack, prayer statue, reflection)

## Recent Critical Updates
- **Prayer System Overhaul:** Changed from `lili.pray` to `prayUsed` counter with proper remaining calculation
- **Debug Panel Integration:** Added comprehensive debugging with attribute offsets and battle logging
- **Death Check Enhancement:** Immediate death checking after any HP change (attacks, prayer statue damage, reflection)
- **Boss4 反伤 Fix:** Boss4 HP removal no longer triggers ring reflection damage

## Essential Functions
- `updateUI()` - Synchronizes all UI elements with game state
- `animateInput(element)` - Blue flash animation on stat changes  
- `getDebuggedBoss(boss)` / `getDebuggedMinion(minion)` - Apply debug offsets
- `challengeBoss()` - Main boss battle function called by UI
- `updateBossIndicators()` - Updates 2x5 grid of boss win/loss predictions

---
**For AI Agents:**
- Always call `updateUI()` after any game state change
- Use `prayUsed` counter, never `lili.pray` (deprecated) 
- Implement death checks immediately after any damage
- Apply debug offsets in all battle calculations
- Maintain single-file architecture with embedded styles/scripts
- Test changes manually in browser - no automated testing framework