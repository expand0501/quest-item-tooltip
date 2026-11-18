# Quest Item Tooltip by Expand

Goal: the item tooltip shows which active task wants the item, how many are needed, and on which map the task giver
is (map name only, no exact position). Shipped readmes: `src/README_EN.txt`, `src/README_RU.txt` (cp1251).

## Files
- `src/gamedata/scripts/quest_item_tooltip.script`: all logic (log tag `[QuestItemTooltip]`).
- `src/gamedata/scripts/quest_item_tooltip_mcm.script`: MCM (id `quest_item_tooltip`, one page). Defaults must match
  `DEFAULTS` in the main script.
- `src/gamedata/scripts/quest_item_tooltip_craft_layout.script` (0.1.2): fix for "279- Crafting Info in Tooltips -
  Utjan" icon mode. When the tooltip frame is > 768 high, Crafting Info calls `UIInfoItem:MoveRecipesRightVertical`,
  which is broken (caption right-aligned on the first stats line, scroll view stretched to frame height so icons
  drop below the tooltip, then `Sync_Finale(true, nil, desc, frame)` cuts the frame to the description). Its normal
  layout is broken too: `SyncRecipesText` gives the caption window the description's height and X-Ray centres text
  vertically (CUILines default), so the caption slides below its icons on long descriptions (0.1.3, artefact "Zuda").
  We make that method a no-op, record each container's item list in a wrapped `AddRecipesContainer` (also marks
  "built this update"), and after Crafting Info's `Update` lay out all shown blocks (`recipes`, `cooking`) ourselves:
  under desc/stats/ammo/parts first; only if that is taller than 768, in one column right of the visible stats
  content (skipped if < 90 wide or it would hit ammo/parts below the stats); then fit the frame. Installed on the first `actor_on_first_update` (Crafting Info is `zzz_*`, loads after us). MCM
  `fix_craft_layout`, default true (own default in that script). Crafting Info is identical on GAMMA `main` and
  `dev2`. Reported by Expand 2026-11-15 (blind dog meat tooltip, our 3 lines pushed it over 768); 0.1.2 confirmed fixing
  that tooltip in game. 0.1.3: "Zuda" fixed, but the first tooltip after loading a save still
  showed Crafting Info's own layout (cause unknown; screenshots 2026-11-15). 0.1.4: layout triggers on new
  `self.CC.recipes/cooking` objects (identity, not our add wrapper) and once per built block when the frame height
  changed after us; below-layout only moves the grid. Debug log lines `craft layout: ...` show what happened.
  0.1.4 tested in game by Expand (2026-11-15) and released unchanged as 1.0.0.
  1.0.1 (2026-11-17, profile `v31.1 - radiation rework`): caption was below its icons again (Zuda, screenshot).
  Cause (from code, not yet confirmed in game): DOGMA weapon tooltips (`zzzz_dogma_gui_weapon_tooltips_main`, active)
  re-installs its own `UIInfoItem.Update`/`Show` on top in `actor_on_update` ("reclaim"), dropping our Update wrapper.
  Never fight that (recursion). Hooks that survive: `SyncRecipesText` (caption one line high, top aligned) and
  `Sync_Finale` (the frame cut after our no-op `MoveRecipesRightVertical` becomes "fit frame to all content").
  Column mode needs the Update wrapper, so with DOGMA tall tooltips show icons under the stats. The winning
  Crafting Info copy in this profile is `UI Rework G.A.M.M.A. Style` (offsets 30, same method names).
  1.0.2 (2026-11-18): crash `No such operator [__eq] defined in class [UICellContainer]` at `update` (released as
  1.0.0 line 234). Lua-defined luabind classes (`UICellContainer`, `UIInfoItem` widgets) have no `__eq`: `==`/`~=`
  between two different instances of the same class is a Lua error (nil vs object is fine, same object is fine).
  **Always compare UI objects with `rawequal`.** Not yet confirmed in game.
- `src/gamedata/configs/text/{eng,rus}/ui_st_quest_item_tooltip.xml`: tooltip words (`st_qit_*`) + MCM strings
  (rus = Windows-1251).

## Version
In `mod.json`, both readme titles + history, main script header, both text files (comment line + MCM credits).

## How it works (keep)
- Hook: wraps `ui_item.build_desc_footer(obj, sec, str)` in `on_game_start`, like "279- Crafting Info in Tooltips -
  Utjan". Every tooltip variant (GAMMA UI, UI Rework, HD Icons `utils_ui.UIInfoItem:Update`) goes through
  `ui_item.get_obj_desc` / `get_sec_desc` -> `build_desc_footer`. `ui_item.item_description` caches the text per
  item id until any key is released, so our block can lag until then.
- Active tasks: `task_manager.get_task_manager().task_info` filtered by `db.actor:get_task(tid, true) ~= nil`
  (finished repeatable tasks stay in `task_info` with stage 255; `<tid>_fetch` vars are set when the job is only
  described in dialog or shown on the PDA taskboard, before the player accepts it; 278 of 380 GAMMA fetch configs
  clear them with `pstor_reset` in `on_complete`, the rest leave them).
  Task objects are luabind `class` instances: `type(tsk)` is `"userdata"`, never `"table"` (0.1.0 skipped every task
  because of a table check). QoL Bundle's quest icon loops over `task_info` too (accepted tasks only) and skips
  `last_check_task` "complete"/"fail", so it marks the same items as we do; offers only heard in dialog are not marked.
- Pstor key: keep reading `<tid>_fetch`. `fetch_func`'s first argument is where setup writes, but the game's own
  status check (`actor_has_fetch_item`) and PDA text (`general_fetch_task`) read `<tid>_fetch`, so that is what
  completes the task. They differ only in Job_to_Done's `[simulation_task_14d]` (copy of 14c with every name left at
  14c, only in `v31.1 - radiation rework`): that task can't complete by bringing items at all, so showing 14c's item
  for it would mislead. Decided 2026-11-14, don't switch to parsing `fetch_func`.
- Sources per task:
  - fetch: pstor `<tid>_fetch` (section string) + `<tid>_fetch_count` (Tasks QoL Pack `tasks_fetch.script` wins in
    both profiles; same storage on `dev2`). A number in `_fetch` = `drx_sl_money_task_status`, skipped.
    `actor_has_fetch_weapon_warfare` needs condition >= 0.6. Artefact containers (`_af_aam/_af_iam/_af_aac/_lead_box`)
    count, as in `actor_has_fetch_item`.
  - multifetch: `required_item_N[_num|_cond]` / `optional_item_N...` read from `task_manager.task_ini` (the module's
    `_M` is local; `m_data.multifetch_data` is only the same table after a load). Cached per task id.
  - DRX retrieve: `status_functor = drx_sl_retrieve_item_task_status`, items in `status_functor_params`.
  - delivery: pstor `<tid>` = table `{ item_1_sec, target_id }` (`tasks_delivery.script`).
- Giver: `<tid>_target_id` (warfare return target) -> `tsk.task_giver_id` -> story object from the task id prefix
  (same order as `general_fetch_task`'s target). Map: `game_graph():vertex(se.m_game_vertex_id):level_id()` ->
  `alife():level_name` -> translate.
- Index sec -> entries rebuilt at most once per `INDEX_TTL` (1000 ms) and only while a tooltip is being built.
  Inventory counting (`count_have`) only for wanted items; multiuse items count uses for fetch tasks (like
  `utils_item.get_amount(..., 1)`).
- Base GAMMA "337- QoL Bundle - Utjan" `fetch_item_icon.script` draws a quest icon on the same items (fetch,
  delivery, multifetch); we complement it, no conflict.

## Profiles
- Same relevant mods in `v31.1 - radiation rework` and `G.A.M.M.A`: Tasks QoL Pack, QoL Bundle (Utjan), Crafting Info
  in Tooltips all enabled. `Job_to_Done_-_Redone_ver._1.9.1` (only in `v31.1 - radiation rework`) changes one nil
  check in `task_manager.script`, no effect here.

## Open items
- Not covered yet: other GAMMA task types with item requirements (intercept artefact, urgent orders, NTA stash,
  `special_steps_for_lostzone_tasks` psy helmet / Jupiter documents, story quests driven by dialogs/infoportions).
- Public repository `expand0501/quest-item-tooltip` since 1.0.0 (2026-11-15).
