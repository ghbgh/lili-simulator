# Copilot Instructions for Lili Turn-Based Battle Game

## Project Overview
This project is a browser-based turn-based battle game featuring a protagonist (Lili) fighting a sequence of 10 bosses and their corresponding minions. The game logic, UI, and data are all managed in a single-page HTML/JavaScript application with real-time battle prediction, visual feedback, corruption level progression system, comprehensive equipment mechanics with 面板同步 (panel synchronization), intelligent point allocation suggestions, and enhanced UI/UX.

## Core Architecture
- **Main Components:**
  - Lili (player): Has `atk`, `def`, `maxHp`, `hp`, and `prayUsed` attributes. All stats are editable via custom arrow-controlled input fields with real-time 面板同步.
  - Bosses: Array of 10 objects, each with `atk`, `def`, `hp`, `special`, and `drop` attributes. Special rules affect battle logic.
  - Minions: Array of 10 objects corresponding to each boss, with `atk`, `def`, `hp`, and `special` attributes.
  - Enhancement System: 16 visible checkbox-based options that modify Lili's abilities and battle mechanics with immediate UI feedback (2 legacy options hidden). Includes new equipment options with stat modifications and corruption mechanics.
  - Boss Indicator System: Visual circular dots (2x5 grid) showing win/loss predictions for all bosses with color-coded feedback and numeric displays.
  - Corruption Level System: Progressive difficulty tracking with automatic updates, remaining corruption display, and color-coded warnings.
  - Point Allocation System: Intelligent suggestions for stat improvements when battles fail, supporting up to 50-point allocations.
  - UI: Displays current stats, boss info, minion info, battle predictions, corruption levels, and provides interactive controls.

- **Battle Flow:**
  - Lili always attacks first each round.
  - Prayer system triggers AFTER Lili's attack but BEFORE enemy attack (timing changed from round start).
  - Prayer trigger condition is FIXED at 30% max HP regardless of prayer recovery amount equipment.
  - Damage = max(1, attacker.atk - defender.def).
  - Special boss/minion rules modify attack/defense/hp or battle sequence.
  - Prayer system allows healing when HP drops below 30% of max HP.
  - White Witch Doll: Each prayer permanently increases max HP by 4 points immediately during battle.
  - Restraint effects (禁锢) block both attacks AND special abilities in first round.
  - Damage cap effects (Boss 6) apply to ALL damage sources including prayer statue damage and reflection.
  - After boss battles, Lili's HP is restored to max, but minion battles preserve current HP.
  - Real-time battle prediction shows outcome without actually fighting.
  - Corruption increases by 5 points per boss defeated and 1 point per minion defeated.
  - Boss圆点指示器 provides visual feedback for all boss battle outcomes.

## Enhancement System (Checkbox-based with 面板同步)
- **Prayer Modifiers:**
  - 祈祷恢复量+10% (儿泣精灵的戒指)
  - 祈祷恢复量+5%
  - 祈祷恢复量-10%
  - 祈祷次数上限+1 (白巫女娃娃：每次祈祷永久增加4点最大HP)
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
  - 腐朽的王冠：攻击+4，防御+4，污秽残渣获取率-40% (with 面板同步)
  - 血染的蝴蝶结：攻击+4，生命上限-20 (with 面板同步)
  - 七彩羽毛头饰：攻击+2，防御+2，生命上限+40 (with 面板同步)

- **BUG Fix Options:**
  - （BUG）Boss8迷瑞尔特殊效果仅生效一次：红色装备，勾选时Boss8在HP为0后只需1次攻击死亡，取消勾选时需要5次攻击死亡

## UI Features
- **Enhanced Input Controls:** Custom left/right arrow buttons for stat adjustment with immediate visual feedback and compact layout
- **Visual Feedback:** Stat changes trigger blue flash animations on affected input fields and prayer count displays
- **Boss Indicators:** Color-coded circular dots in 2x5 grid showing win/loss predictions with numeric displays (1-10)
- **Corruption Tracking:** Display with color-coded warnings (yellow ≤3, orange ≤1, red =0) for remaining corruption levels
- **Real-time Battle Predictions:** Enhanced with blood change calculations and remaining prayer counts
- **Point Allocation Suggestions:** Intelligent recommendations for stat improvements when battles fail (up to 50 points)
- **Minion Challenges:** Separate system for fighting minions that preserves HP state and increases corruption
- **Compact Layout:** Optimized spacing and sizing for better visual density with reduced line spacing (5px gap)
- **Battle Log Formatting:** Simplified round indicators (from "第x轮：" to "x:") for cleaner readability
- **Prayer System Tracking:** Records used prayers internally while displaying remaining count to users
- **Equipment Interaction:** Enhanced click targets - both checkbox and equipment text are clickable for easier interaction
- **Equipment Name Styling:** Color-coded equipment names with deeper background colors for better contrast and visual hierarchy (orange, purple, blue, green, red)
- **Debug Panel:** Fixed right-side overlay panel with Boss/Minion attribute offset controls for testing different values
- **Reset Functions:** 
  - "重置血量和祈祷" resets HP to max and prayer usage, respecting current enhancement bonuses
  - Located in Lili attributes section for logical grouping

## Prayer System Mechanics
- **Timing:** Triggers AFTER Lili's attack but BEFORE enemy attack (changed from round start)
- **Tracking:** Internal `prayUsed` counter tracks usage, UI shows remaining prayers
- **Calculation:** Remaining = Max Prayer Count - Used Prayers
- **Reset Behavior:** Reset function properly calculates current max prayer count based on enhancements
- **Enhancement Integration:** Properly integrates with all prayer-related equipment bonuses

## Developer Workflows
- **No build step required:** Directly edit HTML/JS/CSS files and open in browser.
- **Testing:** Manual, via UI interactions. No automated tests present.
- **Debugging:** Use browser dev tools (console, inspector).

## Project-Specific Patterns
- **Enhancement Options:**
  - 16 visible enhancement options in two-column layout with proper text wrapping and compact 5px spacing
  - 2 legacy options hidden but functional for backward compatibility
  - All enhancement effects are implemented as checkbox-controlled boolean flags
  - Enhancement states are read in real-time during battle calculations
  - Equipment enhancements with 面板同步 use toggle functions for immediate stat updates
  - Equipment names with color-coded backgrounds (orange, purple, blue, green) for visual categorization
  - Clickable equipment text and checkbox for improved user interaction

- **Battle Calculations:**
  - `battleResult()` function handles boss battles with current HP and prayer state
  - `minionBattleResult()` function handles minion battles preserving HP/prayer state
  - `battleResultForBoss()` function provides predictions for boss indicator system
  - All functions support comprehensive enhancement options via options parameter
  - Special boss rules implemented with proper restraint and immunity interactions
  - Damage cap mechanics (Boss #6) apply to ALL damage sources including statue and reflection

- **Point Allocation System:**
  - `calculatePointSuggestion()` function provides intelligent stat improvement recommendations
  - Supports up to 50-point allocations with priority: Attack > HP > Defense
  - Assumes full HP and max prayers for optimal battle conditions
  - Returns formatted suggestions like "ATK:3, DEF:1, HP:2" or "50点内加点无解"

- **面板同步 System:**
  - Toggle functions provide real-time stat updates with visual feedback
  - Equipment effects immediately reflect in UI input fields with animation feedback
  - Custom arrow buttons for precise stat adjustments
  - Prayer tracking system with proper enhancement integration

- **Boss Indicator System:**
  - Compact 2x5 grid layout with numbered dots (1-10)
  - Real-time battle prediction updates with proper prayer/HP state consideration
  - Color-coded feedback with numeric displays for easy identification
  - Optimized spacing to prevent UI element overlap

- **Corruption Level System:**
  - Color-coded warnings: Yellow (≤3), Orange (≤1), Red (=0)
  - Automatic tracking with strategic planning information
  - Integrated with boss progression and minion challenges

- **State Management:**
  - `lili` object includes `prayUsed` for proper prayer tracking
  - `bossIdx` tracks current boss selection with compact navigation
  - Enhanced `updateUI()` function with comprehensive synchronization
  - Battle log formatting for improved readability

## Special Boss Interactions
- **Restraint Effects (魔线脚链):** Block both attacks AND special abilities in round 1
- **Damage Caps (Boss #6):** Apply to direct attacks, prayer statue damage, and reflection damage
- **Prayer Timing:** Now triggers after Lili's attack, providing better tactical positioning
- **Immunity Effects:** Properly interact with restraint and special ability timing

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
- **New Equipment Additions:**
  - Added three new equipment pieces: 腐朽的王冠 (blue), 血染的蝴蝶结 (purple), 七彩羽毛头饰 (orange)
  - Implemented toggleCorruptedCrown(), toggleBloodyRibbon(), toggleRainbowFeather() functions
  - Added proper stat modification and visual feedback for all new equipment
- **Equipment Visual Enhancements:**
  - Deepened orange and green background colors for better contrast
  - Enhanced equipment name styling with color-coded backgrounds
  - Improved visual hierarchy and readability
- **UI Interaction Improvements:**
  - Reduced enhancement line spacing from 10px to 5px for compact layout
  - Enabled label text clicking for all enhancement options
  - Removed complex focus management while keeping simple click interactions
- **Battle Section Width:** Expanded to 1.8x for better battle log visibility
- **Battle Log Format:** Simplified from "第x轮：" to "x:" for cleaner readability
- **Prayer System Overhaul:** 
  - Changed timing from round start to after Lili's attack
  - Implemented proper used/remaining prayer tracking
  - Enhanced reset functionality with enhancement consideration
- **Boss Indicators Enhancement:**
  - Redesigned to 2x5 grid with numbered dots (1-10)
  - Optimized spacing and sizing for compact layout
  - Added tooltips and improved visual hierarchy
- **Attribute Input Controls:**
  - Replaced native number inputs with custom left/right arrow system
  - Enhanced visual feedback and compact alignment
  - Added tooltips for HP adjustment buttons
- **Enhancement System Reorganization:**
  - Expanded from 13 to 16 visible enhancement options
  - Hidden 2 legacy options while maintaining functionality
  - Reorganized into left/right column layout with proper text wrapping
  - Improved visual hierarchy and readability
- **Point Allocation Intelligence:**
  - Expanded from 20 to 50-point calculations
  - Optimized priority system: Attack > HP > Defense
  - Enhanced suggestion formatting and failure messaging
- **Corruption Level Warnings:**
  - Added color-coded background warnings (Yellow/Orange/Red)
  - Enhanced visual feedback for strategic planning
- **Boss Combat Enhancements:**
  - Improved restraint effects to block special abilities
  - Enhanced damage cap mechanics for all damage sources
  - Fixed prayer timing interactions with boss special abilities
- **UI/UX Improvements:**
  - Optimized boss navigation spacing and sizing
  - Enhanced visual density and component alignment
  - Improved responsive layout and element positioning
- **Prayer System Fixes:**
  - Prayer trigger condition fixed to 30% max HP regardless of recovery amount equipment
  - White Witch Doll now correctly increases max HP by 4 points per prayer during battle
  - Prayer timing properly implemented after Lili's attack but before enemy attack
- **Boss8 BUG Option:**
  - Added red-colored BUG equipment option for Boss8 special effect control
  - Checked: Boss8 dies after 1 hit when HP reaches 0 (BUG behavior)
  - Unchecked: Boss8 dies after 5 hits when HP reaches 0 (normal behavior)
- **Debug Panel System:**
  - Fixed right-side overlay panel for attribute offset testing
  - Boss and minion attribute offsets (attack, defense, HP) for debugging
  - Real-time battle calculation updates with offset values
  - Fixed defense offset bug where Boss defense offsets were not applied correctly

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
- Respect prayer timing changes (after attack, before enemy action)
- Consider all damage sources when implementing damage caps or limitations
- Maintain UI compactness while ensuring functionality and readability
- Use intelligent point allocation suggestions to guide player strategy
- Implement proper restraint effects that block both attacks and special abilities
