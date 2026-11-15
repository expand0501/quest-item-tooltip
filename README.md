# Quest Item Tooltip

A mod for **S.T.A.L.K.E.R. Anomaly G.A.M.M.A.** by Expand.

**[Download the latest version](https://github.com/expand0501/quest-item-tooltip/releases/latest)** · [Русский](#русский)

The item tooltip shows which of your active tasks wants the item, how many are needed, how many you carry and on
which map the task giver is (map name only, no exact position).

```
Needed for tasks:
 * Sidorovich (Cordon): bring 3 - you have 1
 * Barkeep (Rostok): bring 1, condition at least 60% - you have 0
 * Deliver to Wolf (Cordon)
```

## Features

- Task giver name and map, amount needed, amount you carry (green = enough, yellow = some, red = none),
  minimum condition when the task asks for one.
- Supports fetch tasks (including warfare weapon tasks), tasks with several items and optional extra items,
  DRX storyline "retrieve item" tasks and delivery packages. Artefacts in containers count for artefact tasks.
- Works in the inventory, stashes, trade and loot windows. Only accepted tasks are shown.
- Every part can be turned on or off in MCM (MCM > Quest Item Tooltip), settings apply at once.
- Fixes the icon layout of "Crafting Info in Tooltips" (Utjan) in long or tall tooltips (can be turned off).
- Nothing runs every frame: task data is read only while a tooltip is built, at most once per second.

## Installation (Mod Organizer 2)

1. Install the zip from [Releases](https://github.com/expand0501/quest-item-tooltip/releases) as a new mod and
   enable it. No conflicts known; works next to the quest icon of GAMMA's QoL Bundle.
2. Safe to install or remove mid-playthrough: the mod saves nothing, it only reads the game's task data.

The full manual is in [`src/README_EN.txt`](src/README_EN.txt) (also inside the zip).
Changes per version: [`CHANGELOG.md`](CHANGELOG.md).

## Bug reports

Open an [issue](https://github.com/expand0501/quest-item-tooltip/issues): turn on "Debug log" in MCM, reproduce the
problem and attach the lines containing `[QuestItemTooltip]` from `Anomaly\appdata\logs\xray_*.log`.

## License

[CC BY-NC-SA 4.0](LICENSE): you may share and change this mod, including in modpacks, if you credit Expand,
do not use it commercially and keep the same license.

---

## Русский

Мод для **S.T.A.L.K.E.R. Anomaly G.A.M.M.A.**, автор Expand.

**[Скачать последнюю версию](https://github.com/expand0501/quest-item-tooltip/releases/latest)**

В подсказке предмета видно, какое из ваших активных заданий его требует, сколько нужно, сколько у вас с собой и на
какой локации находится заказчик (только название локации, без точного места).

### Возможности

- Имя и локация заказчика, сколько нужно, сколько у вас (зелёный = хватает, жёлтый = не хватает, красный = нет),
  минимальное состояние, если задание его требует.
- Поддерживаются задания «принеси N предметов» (включая оружейные задания warfare), задания с несколькими
  предметами и необязательными дополнительными, сюжетные задания DRX «найти предмет» и посылки для доставки.
  Артефакты в контейнерах учитываются.
- Работает в инвентаре, тайниках, торговле и окне обыска. Показываются только принятые задания.
- Каждую часть можно включить или выключить в MCM (MCM > Quest Item Tooltip), настройки применяются сразу.
- Исправляет раскладку иконок мода «Crafting Info in Tooltips» (Utjan) в длинных подсказках (можно отключить).
- Ничего не выполняется каждый кадр: данные заданий читаются только при построении подсказки, не чаще раза в секунду.

### Установка (Mod Organizer 2)

1. Установите zip из [Releases](https://github.com/expand0501/quest-item-tooltip/releases) как новый мод и включите
   его. Конфликтов не известно; работает вместе со значком задания из QoL Bundle в GAMMA.
2. Можно ставить и удалять посреди прохождения: мод ничего не сохраняет, он только читает данные заданий игры.

Полное описание — в [`src/README_RU.txt`](src/README_RU.txt), оно же лежит в архиве.

### Сообщить об ошибке

Создайте [issue](https://github.com/expand0501/quest-item-tooltip/issues): включите «Debug log» в MCM, повторите
проблему и приложите строки с `[QuestItemTooltip]` из `Anomaly\appdata\logs\xray_*.log`.

### Лицензия

[CC BY-NC-SA 4.0](LICENSE): мод можно распространять и изменять, в том числе включать в сборки, если указать
автора (Expand), не использовать его в коммерческих целях и сохранить ту же лицензию.
