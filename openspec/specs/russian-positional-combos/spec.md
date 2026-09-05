## Purpose

Определяет перенос совместимых русских combo по физическим позициям без привязки к буквенным HID-кодам и без изменения отложенной настройки overlap.

## Requirements

### Requirement: Совместимые базовые combo на русском слое
Совместимые Enter, Tab, Caps Word, Caps Lock, восемь modifier combo и тринадцать OS-aware action combo SHALL работать по тем же `key-positions` на `RUSSIAN`, что и на `GRAPHITE`/`QWERTY`. Их bindings MUST NOT дублироваться по буквенным keycode и OS-aware действия SHALL оставаться централизованными через `&os_action`.

#### Scenario: Одинаковый физический chord на разных базовых слоях
- **WHEN** пользователь нажимает позиции совместимого combo на `GRAPHITE`, `QWERTY` или `RUSSIAN`
- **THEN** combo выполняет одно и то же назначенное действие независимо от букв под этими позициями

### Requirement: Release-семантика удерживаемых combo
Все восемь modifier combo SHALL сохранять `slow-release`. Русский momentary Smiles combo SHALL также использовать `slow-release`, чтобы слой оставался активным до отпускания последней клавиши chord.

#### Scenario: Частичное отпускание modifier chord
- **WHEN** modifier combo сработало и пользователь отпустил только одну входящую клавишу
- **THEN** модификатор остаётся нажатым
- **WHEN** отпущена последняя входящая клавиша
- **THEN** модификатор отпускается и не остаётся зажатым

### Requirement: Раздельные English и Russian Smiles combo
Позиции 5 и 17 SHALL сохранять существующее combo `&mo SMILES` только для `GRAPHITE`/`QWERTY` и SHALL иметь отдельное combo `&mo RUSSIAN_SMILES` только для `RUSSIAN`. Оба combo SHALL быть позиционными и MUST NOT иметь пересекающиеся layer scopes.

#### Scenario: Smiles chord на русском слое
- **WHEN** позиции 5 и 17 удерживаются на `RUSSIAN`
- **THEN** активируется `RUSSIAN_SMILES`, а не английский `SMILES`

### Requirement: Исключения области combo
Vim combo позиций 19 и 22 MUST NOT работать на `RUSSIAN`, поскольку исходный русский QMK-слой не содержит такого combo. Арифметические combo SHALL оставаться только на `NUMBERS`, а языковые combo SHALL иметь отдельные непересекающиеся scopes `GRAPHITE` и `RUSSIAN`.

#### Scenario: Vim chord на русском слое
- **WHEN** пользователь нажимает позиции 19 и 22 на `RUSSIAN`
- **THEN** слой `VIM` не активируется

### Requirement: Отложенная настройка overlap
Миграция русских слоёв MUST NOT удалять вложенные короткие и длинные combo и MUST NOT менять текущий стартовый timeout 50 мс ради коррекции поведения новых физических switches. Исследование overlap SHALL оставаться отдельным change и отдельной веткой.

#### Scenario: Проверка отсутствия несвязанных изменений
- **WHEN** сравнивается конфигурация combo до и после русской миграции
- **THEN** существующие overlap-пары и их тайм-ауты не изменены, кроме добавления обоснованных layer scopes для `RUSSIAN`
