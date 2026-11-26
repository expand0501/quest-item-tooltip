# Quest Item Tooltip: in-game test plan (autotest suite `qit`)

Runner: `tools\autotest\mod\gamedata\scripts\zzz_claude_autotest_qit.script` (flag `-claudeqit=`, log
`F:\Anomaly\appdata\logs\claude_autotest_qit.log`). Tests the installed zip unchanged (no test hooks in the mod).
Written 2026-11-26 for 1.0.2.

## Results
- 2026-11-26, 1.0.2 installed zip, profile `Claude Test`, save `claude_test`, run ids `20261126-205154` (basics,
  17/17) and `20261126-205257` (other 12): PASS fetch, warfare, multifetch, retrieve, not_active, giver, options,
  disabled, no_mcm, saveload. `delivery` UNCLEAR: lines right ("Deliver to Shura Executioner (Truck Cemetery)", item
  tooltip too), but `delivery_other_map` skipped (no NPC of the fixed story list on another level; the runner now also
  searches `story_objects.object_id_by_story_id` and logs why each candidate was rejected). `craft_layout` FAIL
  `caption_above_1/2_recipes`: real layout bug with DOGMA weapon tooltips, see `CLAUDE.md` "Open bug". No Lua errors.
- 2026-11-26 rerun `delivery,craft_layout` (`20261126-210512`): `craft_layout` failed identically (caption 267+14 vs
  grid 217, 201+14 vs 189) = the DOGMA bug reproduced twice. `delivery_other_map` still SKIP: the story lookups gave
  "none" for Sidorovich, Barkeep, Beard, Cold, Sakharov and "dead" (`se:alive()` false) for the 7 registered stalkers
  reached, though `get_story_se_object("army_south_mechan_mlr")` gave Seryoga in Cordon in `giver` (that path does not
  test alive). Cause **unknown** (save state or lookup). Runner changed again: story NPCs resolved through
  `story_objects.object_id_by_story_id` -> `alife():object`, every candidate logged with id, section, clsid, stalker,
  `alive()` and level; fallback = the alive `sim_default` stalker with the lowest id on another level (same filter as
  `on_init_delivery_task`'s own target search), so the check no longer depends on story NPCs.
- 2026-11-26 rerun `delivery` (`20261126-210818`): PASS 4/4. The story NPCs of the list are not registered in this
  save (only `esc_m_trader`, not a stalker); the fallback took a `sim_default` stalker in Garbage.
- **Total for 1.0.2: 12 of 13 PASS; `craft_layout` FAIL = real bug (Crafting Info caption with DOGMA, `CLAUDE.md`).**

## How it tests
- **Tasks are real**: picked from `task_manager.task_ini` by their status functor (first sorted section not given and
  not active), given like the task dialog does it (`on_job_descr` effects where needed, then
  `task_manager:give_task(tid, giver_id)`), with a friendly stalker spawned near the player as giver.
- **Task data has known values**: the runner writes `<tid>_fetch` / `<tid>_fetch_count` and the delivery table
  `<tid>` itself, in the shape `setup_fetch_task` / `on_init_delivery_task` write them (those need a dialog speaker).
- **Tooltip text** from `ui_item.build_desc_footer(nil, sec, desc)` (what `get_sec_desc` does), `ui_item.get_sec_desc`
  and `ui_item.get_obj_desc(item)` (engine path, cache busted by asking for another item first).
- **Expected lines** from the runner's own copy of the line format; "you have" from `utils_item.get_amount` (mode 1 for
  fetch = the game's own count, mode 0 for multifetch/retrieve), own count for condition limits. Giver name/map read
  from the server object (`level.name()` for the spawned giver).
- **Settings** per scenario in memory (`ui_mcm.get` wrapper set at file load, before `quest_item_tooltip.on_game_start`
  reads MCM), live changes through `on_option_change`. Debug log on except in `no_mcm`.
- Sections the save's own tasks already want are not used (read from the first debug "index rebuilt" line).
- `stale()` clears the mod's `index_time` upvalue so a tooltip right after a change rebuilds the index (the 1 s
  `INDEX_TTL` would hide it); needed where the check must run in the same step (task without its giver).

## Scenarios (one launch, ~13 loads)
| Scenario | Proves |
|---|---|
| `basics` | hook in `build_desc_footer` chain, internals readable, `DEFAULTS` = runner copy = MCM page defs (+ `fix_craft_layout` true), settings read, version 1.0.2 in script + both text files, rus ids = eng ids and cp1251, every string resolves, craft fix installed (`SyncRecipesText`, `Sync_Finale`, `MoveRecipesRightVertical`), no block for an unwanted item |
| `fetch` | exact line "Name (Map): bring 3 - you have N", colour yellow/green/red, debug index line, real item tooltip, `get_sec_desc`, item change moves the block, artefact + its container (`_lead_box`/`_af_*`), multiuse counts uses, a number in `<tid>_fetch` is ignored |
| `warfare` | `actor_has_fetch_weapon_warfare`: "condition at least 60%", a 50 % weapon is not counted (UNCLEAR if no such task or condition not settable) |
| `multifetch` | every required/optional item of the task section, `_num`, `_cond`, "(optional, extra pay)" |
| `retrieve` | DRX `status_functor_params` items: "bring 1" (UNCLEAR if no such task) |
| `delivery` | "Deliver to Name (Map)" for a story NPC on another map, real package tooltip |
| `not_active` | offer only (`_fetch` set, task not taken): no block; DRX money task not indexed; cancelled task: block gone |
| `giver` | order of the named person: `<tid>_target_id` > task giver > story NPC from the task id |
| `options` | live: giver off, map off, both off, title on, count off, optional off, delivery off, Enable off (no index work), on again, debug off (no log lines) |
| `disabled` | Enable off from the load: no block, no index work; on while running: block |
| `no_mcm` | no MCM values: `cfg` = `DEFAULTS`, craft fix on, default line |
| `saveload` | task taken, `save claude_autotest`, load: task kept, first tooltip after the load right (overwrites the save `claude_autotest`) |
| `craft_layout` | real `utils_ui.UIInfoItem` (owner = a bare `CUIWindow`) with Crafting Info icons on; 3 items in a row (new containers while old referenced = 1.0.2 `__eq` crash), caption above its icons, icons inside the frame, then fix off live. Last in the launch: if building an info box outside the inventory crashes, nothing else is lost |

UNCLEAR = the needed task type/section does not exist in this profile, or a setup step did not happen.

## Not automated (for Expand)
- Looks: colours as seen, line wrapping, tooltip size; the Crafting Info column layout in tooltips taller than 768
  (needs our `Update` wrapper, which DOGMA replaces in `Claude Test`; the runner logs which `UIInfoItem` hooks are ours).
- Hovering with the mouse in inventory / trade / stash / loot windows (the same text functions are called directly).
- `ui_item` description cache lag until a key is released.
- Real task dialog flow (fetch item picked by `setup_fetch_task` with a speaker) and story quests driven by dialogs.
- Base GAMMA profile `G.A.M.M.A`: run `basics,fetch,multifetch,delivery,saveload` there once before a release
  (own save, `-Isolated`).
