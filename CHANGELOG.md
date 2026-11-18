# Changelog - Quest Item Tooltip by Expand

- 1.0.2: fix crash "No such operator [__eq] defined in class [UICellContainer]" in the Crafting Info layout
  fix (`UIInfoItem:Update`). Lua-defined luabind classes have no `__eq`, so `==`/`~=` between two different
  container objects is a Lua error. The "new containers?" check, the item-list check and the `Sync_Finale` frame check
  now use `rawequal`. Crashed only when Crafting Info built a new container while the old one was still referenced
  (identity check added in 0.1.4).
- 1.0.1: Crafting Info captions were still below their icons when the DOGMA weapon tooltips mod is installed: DOGMA puts
  its own `UIInfoItem.Update` back on top every frame, which removed our layout wrapper. The caption fix now hooks
  `SyncRecipesText` (caption window one line high, top aligned) and the frame cut in tall tooltips is replaced by
  fitting the frame to all content (`Sync_Finale`); neither can be removed that way. The full layout (column right of
  the stats) still needs our `Update` wrapper, so with DOGMA a tall tooltip shows the icons under the stats.
- 1.0.0: first public release. Same code as 0.1.4 (tested in game); version label only.
- 0.1.4: Crafting Info captions were still below their icons on the first tooltip after loading a save (cause not
  found from the code). The layout now runs when Crafting Info's container objects are new (no longer only when our
  `AddRecipesContainer` wrapper saw the call) and once more if another script changes the frame height afterwards
  (at most once per built block). The layout under the description no longer rebuilds the icon grid (only moves it),
  so it works without the item list. Debug log: chosen layout, whether item lists were known, frame-height repairs.
- 0.1.3: Crafting Info captions slid below their icons on items with a long description (its caption window has
  the description's height and X-Ray centres text vertically). The fix now lays out Crafting Info's blocks every
  time it builds them (caption sized to its text, icons under it, under description/stats); the column right of the
  stats is used only when that layout is taller than the screen. Debug log prints the chosen layout.
- 0.1.2: fix for "Crafting Info in Tooltips" (icon mode) in tooltips taller than the screen: its side layout put
  the caption on the first stats line and the icons below the tooltip. New `quest_item_tooltip_craft_layout.script`
  replaces that step and lays the icon blocks out in a column right of the stats (or under everything when there is
  no room). MCM option "Fix Crafting Info icon layout" (default on).
- 0.1.1: fix: the tooltip block never appeared. Task objects are luabind class instances (`type()` = "userdata"),
  and the "is a table" check skipped every task. Debug log now lists task counts and every wanted item with its task.
- 0.1.0: first test build. Item tooltips show which active tasks want the item: task giver name and map, how many
  are needed, how many the player carries, minimum condition, optional extra items. Supports fetch tasks
  (incl. warfare weapon fetch), multifetch, DRX storyline retrieve tasks and delivery packages. MCM options for every
  part, English and Russian text.
