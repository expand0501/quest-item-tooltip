Quest Item Tooltip 1.0.0 by Expand
For S.T.A.L.K.E.R. Anomaly / G.A.M.M.A.

WHAT IT DOES
When you hover over an item that one of your active tasks wants, its tooltip gets a block like this:

  Needed for tasks:
   * Sidorovich (Cordon): bring 3 - you have 1
   * Barkeep (Rostok): bring 1, condition at least 60% - you have 0
   * Deliver to Wolf (Cordon)

- who wants the item and on which map that person is now (only the map name, not the exact place)
- how many the task needs, and how many you carry (green = enough, yellow = some, red = none)
- minimum condition when the task asks for one
- works in the inventory, stashes, trade and loot windows

Supported tasks: "bring me N items" fetch tasks (most item tasks in GAMMA, including warfare weapon
tasks), tasks with several items and optional extra items, DRX storyline "retrieve item" tasks, and
delivery packages (shows whom to deliver them to). Artefacts in a container count for artefact tasks.
Only tasks you have accepted are shown. Works next to the small quest icon that GAMMA's QoL Bundle draws
on task items.

REQUIREMENTS
- G.A.M.M.A. (or Anomaly 1.5.2 with modded exes)
- MCM (included in GAMMA), optional: without it the defaults below are used

INSTALLATION (MO2)
Install this archive with MO2 ("Install a new mod from an archive") and enable it. No conflicts known.
Safe to install or remove mid-playthrough: the mod saves nothing, it only reads the game's task data.

MCM -> "Quest Item Tooltip" (all settings apply at once)
  Enable                 - default on
  Task giver name        - default on
  Task giver map         - default on
  Task title             - the task title from the PDA, default off
  How many you carry     - default on (items in stashes are not counted)
  Optional extra items   - default on
  Delivery packages      - default on
  Fix Crafting Info icon layout - default on (see COMPATIBILITY)
  Debug log              - default off

COMPATIBILITY
"Crafting Info in Tooltips" (Utjan, in GAMMA) with item icons on draws its "Used for crafting /
cooking" captions in the wrong place: below their icons on items with a long description, and on the
first stats line with the icons below the tooltip when the tooltip is too tall for the screen. This mod
lays these blocks out again: caption, then icons, under the description and stats; only when that does
not fit on the screen, in a column right of the stats. Can be turned off in MCM. Does nothing without
Crafting Info.

PERFORMANCE
Nothing runs every frame. The list of wanted items is read from your active tasks only when a tooltip
is built (at most once per second), and your inventory is counted only for items a task wants.

VERSION HISTORY
1.0.0 - first public release (same as 0.1.4).
0.1.4 - Crafting Info layout fix also on the first tooltip after loading a save (more robust detection).
0.1.3 - Crafting Info captions no longer slide below their icons on items with a long description.
0.1.2 - fix for Crafting Info in Tooltips icons in tall tooltips (were drawn in the wrong place).
0.1.1 - fix: the tooltip block never appeared (all tasks were skipped). Debug log lists the found tasks.
0.1.0 - first test build.

CREDITS
Author: Expand.
Tooltip hook modelled on "Crafting Info in Tooltips" by Utjan.
