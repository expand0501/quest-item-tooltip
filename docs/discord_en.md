## Description
Item tooltips now tell you which of your active tasks wants the item, how many you need, and where the task giver is.

```
Needed for tasks:
 * Sidorovich (Cordon): bring 3 - you have 1
 * Barkeep (Rostok): bring 1, condition at least 60% - you have 0
 * Deliver to Wolf (Cordon)
```

## Features
- **Task giver name and map** (map name only, no exact position)
- **Amount needed vs. amount you carry**, coloured: green = enough, yellow = some, red = none
- **Minimum condition** when the task asks for one (e.g. warfare weapon tasks)
- Supports fetch tasks, tasks with several items (plus **optional extra items**), DRX storyline "retrieve item" tasks and **delivery packages** (shows whom to deliver to)
- Artefacts in containers count for artefact tasks
- Works in inventory, stashes, trade and loot windows; only accepted tasks are shown
- Each part (giver, map, task title, carried count, optional items, packages) can be turned off, MCM configurable
- Fixes **Crafting Info in Tooltips** (Utjan) icon mode: captions no longer slide below their icons or out of tall tooltips. The fix is MCM configurable
- Nothing runs every frame: tasks are read only when a tooltip is shown

## Install / remove
- Install the zip with MO2 as a new mod. No conflicts known, works next to QoL Bundle's quest icon.
- **Safe to install or remove mid-playthrough**: the mod saves nothing to your save.

## Bug reports
Post them **here** with:
- `xray_*.log` from `Anomaly\appdata\logs\` (turn on *Debug log* in the mod's MCM first)
- mod version, GAMMA version, engine (modded exes or Quark)
- what happened vs. what you expected, steps to reproduce
- a screenshot of the tooltip

## Links
Download and source: <https://github.com/expand0501/quest-item-tooltip/releases>
