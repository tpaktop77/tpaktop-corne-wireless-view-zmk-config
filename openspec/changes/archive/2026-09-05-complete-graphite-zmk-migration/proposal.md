## Why

Текущий ZMK keymap уже содержит перенесённые вспомогательные слои и combo, но базовый слой всё ещё QWERTY, системные действия зафиксированы под Windows, а Bluetooth, Auto Shift и независимый OS profile не соответствуют исходной Graphite-конфигурации Oryx. Изменение завершает английскую часть миграции в одной воспроизводимой конфигурации для беспроводной Corne, сохраняя nice!view и ZMK Studio.

## What Changes

- **BREAKING**: заменить текущий базовый QWERTY слоем `GRAPHITE` с индексом 0 и перестроить индексы остальных активных слоёв через именованные константы.
- Добавить минимальный альтернативный `QWERTY` и прямое переключение `GRAPHITE ↔ QWERTY` правой нижней body-клавишей.
- Удалить ставшие ненужными общие `Lower` и `Raise`, заменив их специализированным `SYSTEM_BT` на левом внешнем thumb.
- Добавить BT profile selection 0–4, защищённый удержанием Bluetooth Clear и независимый выбор Windows/macOS/Linux.
- Добавить локальный out-of-tree ZMK v0.3 module с behavior API `&os_set <profile>` и `&os_action <action>`; OS profile по умолчанию Windows, не хранится во flash и не представлен слоем.
- Централизовать 13 OS-aware системных действий и перевести соответствующие combo с Windows-only keycodes на `&os_action`.
- Сделать буквенные combo позиционными для `GRAPHITE` и `QWERTY`, числовые — только для `NUMBERS`; сохранить вложенные combo и физические позиции бывших `F22/F23/F24` без фиктивных keycodes.
- Добавить `slow-release` к восьми modifier combo и momentary Smiles combo.
- Добавить отдельный параметризованный Auto Shift hold-tap с тайм-аутом 250 мс для всех английских букв Graphite и QWERTY, не меняя `&lt.quick-tap-ms` thumb-клавиш.
- Сохранить текущие Numbers, Navigation, Function, Vim, Symbols и Smiles функционально эквивалентными с поправкой именованных индексов; Navigation не адаптировать под OS profile.
- Сохранить build matrix для обеих половин, nice!view, Studio RPC центральной половины и три reserved layer для ZMK Studio.
- Добавить полную migration/layout/OS/test документацию и зафиксировать ручные USB/BLE/hardware проверки, которые нельзя подтвердить CI.
- Использовать результат смерженного PR #2 (`Migrate compatible Oryx combos`) как исходную базу: определения не дублировать, а преобразовать и расширить на месте.

## Capabilities

### New Capabilities

- `keyboard-layout`: Graphite layer 0, минимальный QWERTY, именованная структура слоёв, thumb bindings и сохранение существующих вспомогательных слоёв.
- `system-bluetooth-layer`: специализированный System/Bluetooth layer, BT0–BT4, защищённый BT Clear и OS selectors.
- `os-profile`: независимое volatile-состояние Windows/macOS/Linux и централизованные `os_set`/`os_action` behaviors.
- `positional-combos`: одинаковые физические combo для Graphite/QWERTY, Numbers-only combo, overlap и release-семантика удерживаемых combo.
- `auto-shift`: Auto Shift 250 мс только для английских букв двух раскладок, независимо от thumb layer-tap.
- `firmware-build`: сохранение Corne split, nice!view, ZMK Studio и проверяемая сборка обеих половин.

### Modified Capabilities

Существующих OpenSpec capability specs в репозитории нет.

## Impact

- `config/corne.keymap`: базовые раскладки, именованные слои, thumb bindings, System layer, Auto Shift и combo.
- `config/corne.conf`, `config/west.yml`, `build.yaml`: только минимальные изменения, необходимые для локального ZMK-модуля и сборки; аппаратная матрица и Studio snippet сохраняются.
- Новый repo-local Zephyr/ZMK module: `zephyr/module.yml`, `CMakeLists.txt`, `Kconfig`, devicetree bindings/includes, dt-binding header и C-реализация behaviors.
- Новые документы `docs/migration-matrix.md`, `docs/test-matrix.md`, `docs/layout.md`, `docs/os-profile.md`.
- Источник QMK/Oryx используется только read-only на удалённом HEAD `5d674ed4d866b66e34389262092cc5347da90bc7`; целевая база — смерженный PR #2 на `8e885d4d28117703abbc0b027bd9686bb9c7a84e`.
- Русские слои, старый QMK layer 2, mouse/trackpad/navigation extensions и прочие явно исключённые функции не затрагиваются.
