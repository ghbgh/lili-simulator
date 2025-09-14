# Copilot Instructions for Lili Turn-Based Battle Game

## Project Overview
This project is a browser-based turn-based battle game featuring a protagonist (Lili) fighting a sequence of 10 bosses and their corresponding minions. The game logic, UI, and data are all managed in a single-page HTML/JavaScript application with real-time battle prediction, visual feedback, corruption level progression system, and comprehensive equipment mechanics with 面板同步 (panel synchronization).

## Core Architecture
- **Main Components:**
  - Lili (player): Has `atk`, `def`, `maxHp`, and `hp` attributes. All stats are editable via input fields with real-time 面板同步.
  - Bosses: Array of 10 objects, each with `atk`, `def`, `hp`, `special`, and `drop` attributes. Special rules affect battle logic.
  - Minions: Array of 10 objects corresponding to each boss, with `atk`, `def`, `hp`, and `special` attributes.
  - Enhancement System: 14 checkbox-based options that modify Lili's abilities and battle mechanics with immediate UI feedback.
  - Boss Indicator System: Visual circular dots showing win/loss predictions for all bosses with color-coded feedback.
  - Corruption Level System: Progressive difficulty tracking with automatic updates and remaining corruption display.
  - UI: Displays current stats, boss info, minion info, battle predictions, corruption levels, and provides interactive controls.

- **Battle Flow:**
  - Lili always attacks first each round.
  - Damage = max(1, attacker.atk - defender.def).
  - Special boss/minion rules modify attack/defense/hp or battle sequence.
  - Prayer system allows healing when HP drops below 30% of max HP.
  - After boss battles, Lili's HP is restored to max, but minion battles preserve current HP.
  - Real-time battle prediction shows outcome without actually fighting.
  - Corruption increases by 5 points per boss defeated and 1 point per minion defeated.
  - Boss圆点指示器 provides visual feedback for all boss battle outcomes.

## Enhancement System (Checkbox-based with 面板同步)
- **Prayer Modifiers:**
  - 祈祷恢复量+10% (儿泣精灵的戒指)
  - 祈祷恢复量+5%
  - 祈祷恢复量-10%
  - 祈祷次数上限+1 (白巫女娃娃：与守护者战斗开始时祈祷次数回复至上限)
  - 祈祷次数上限+1 (白巫女耳环)
  - 白巫女的雕像：祈祷使用次数上限+1，祈祷恢复生命时会对敌人造成恢复量一半的伤害（伤害计算防御）
  - 不能使用祈祷

- **Combat Modifiers:**
  - 芙莉蒂雅的戒指：受到攻击时反弹防御力×1点伤害，防御+3 (with 面板同步)
  - 执行人的手套：每回合攻击两次，攻击-5 (with 面板同步)
  - 对守护者（boss）攻击力提高2点
  - 对小怪攻击力提高3点
  - 魔线脚链：首轮禁锢敌方，不能发动攻击，防御+1 (with 面板同步)
  - 雅若拉的戒指：免疫非攻击类伤害，防御+3 (with 面板同步)
  - 交换攻击和防御，攻击+2，防御+2 (with 面板同步)

## UI Features
- **Editable Stats:** All of Lili's stats (attack, defense, max HP, current HP) are editable via number input fields with real-time 面板同步
- **Visual Feedback:** Stat changes trigger blue flash animations on affected input fields and prayer count displays
- **Boss Indicators:** Color-coded circular dots showing win/loss predictions for all 10 bosses (green=win, red=loss, highlighted=selected)
- **Corruption Tracking:** Display current corruption level and remaining corruption before next increase
- **Real-time Updates:** Battle predictions and boss indicators update automatically when stats or enhancements change
- **Minion Challenges:** Separate system for fighting minions that preserves HP state and increases corruption
- **Reset Functions:** 
  - "重置游戏" resets everything to initial state including all enhancements
  - "重置属性" resets only current HP and prayer count

## Developer Workflows
- **No build step required:** Directly edit HTML/JS/CSS files and open in browser.
- **Testing:** Manual, via UI interactions. No automated tests present.
- **Debugging:** Use browser dev tools (console, inspector).

## Project-Specific Patterns
- **Enhancement Options:**
  - All enhancement effects are implemented as checkbox-controlled boolean flags
  - Enhancement states are read in real-time during battle calculations
  - Equipment enhancements with 面板同步 use toggle functions for immediate stat updates
  - No persistent state storage - enhancements reset on page reload

- **Battle Calculations:**
  - `battleResult()` function handles boss battles with full HP
  - `minionBattleResult()` function handles minion battles with current HP
  - `battleResultForBoss()` function provides predictions for boss indicator system
  - Both functions support all enhancement options via options parameter
  - Special boss rules implemented as conditional logic based on `bossIdx`

- **面板同步 System:**
  - Toggle functions like `toggleGlove()`, `toggleMagicChain()`, `toggleMoliliRing()` provide real-time stat updates
  - Equipment effects immediately reflect in UI input fields with animation feedback
  - Attack/defense swap functionality with `toggleSwapAtkDef()` includes stat bonuses

- **Animation System:**
  - CSS keyframe animations for visual feedback
  - `animateInput()` function triggers blue flash effect
  - Animations triggered on stat changes from any source (manual input, battle results, resets, equipment)

- **Boss Indicator System:**
  - `updateBossIndicators()` function updates all boss prediction dots
  - Color-coded feedback system with CSS classes (.boss-dot-win, .boss-dot-lose, .boss-dot-active)
  - Real-time updates when any stat or enhancement changes

- **Corruption Level System:**
  - Automatic tracking with boss defeats (+5) and minion defeats (+1)
  - Remaining corruption calculation for strategic planning
  - Maximum corruption level of 64 for 10 boss progression

- **State Management:**
  - `lili` object holds current player state
  - `bossIdx` tracks current boss selection
  - `updateUI()` function synchronizes all display elements including indicators and corruption

## Key Files/Directories
- `index.html`: Complete game implementation with embedded CSS/JS
- `.github/copilot-instructions.md`: This file, for AI agent guidance

## Example Patterns
- Enhancement option reading: `const optPrayAmount = document.getElementById('opt-pray-amount').checked;`
- Animation triggering: `if (oldValue !== newValue) animateInput(targetElement);`
- Battle calculation: `battleResult(liliStats, bossStats, enhancementOptions)`
- Equipment toggle function: `function toggleGlove() { /* modify lili.atk and update UI */ }`
- Boss indicator update: `updateBossIndicators()` called after any stat change
- Corruption tracking: Auto-incremented after boss/minion defeats with UI updates

## Integration Points
- No external dependencies or frameworks
- All logic is self-contained in the HTML/JS file
- CSS animations provide immediate visual feedback

## Conventions
- Use clear variable names for stats and enhancement attributes
- Trigger animations when any stat value changes from any source
- Keep battle logic pure functions that accept options parameters
- Document any new enhancement rules directly in code comments
- Maintain real-time UI updates for immediate feedback
- Implement equipment effects with 面板同步 for immediate visual feedback
- Update boss indicators and corruption display whenever game state changes

## Recent Updates
- Replaced relic grid system with checkbox-based enhancements
- Added editable current HP input field
- Implemented visual animation system for stat changes
- Added minion battle system with HP preservation
- Updated prayer display from "祈祷次数" to "使用祈祷"
- Added comprehensive animation triggers for all stat modifications
- Implemented Boss圆点指示器 system with color-coded win/loss predictions
- Added corruption level progression system with remaining corruption display
- Created 面板同步 equipment system with real-time stat updates
- Expanded to 10 bosses with balanced difficulty progression
- Added multiple equipment items with defense bonuses and attack modifications
- Implemented toggle functions for equipment effects (gloves, rings, chains)
- Enhanced attack/defense swap functionality with stat bonuses

---
**For AI agents:**
- Always update both UI and game state after any stat or enhancement change
- Implement new enhancement options as explicit logic in battle functions
- Trigger animations when stats change from any source (battles, resets, manual input)
- Keep code readable and maintainable; avoid unnecessary abstraction
- Reference this file for project-specific patterns and update as the game evolves
- Use 面板同步 for equipment effects to provide immediate visual feedback
- Update boss indicators whenever battle predictions might change
- Maintain corruption level tracking for strategic gameplay progression
