## Purpose

Определяет одинаковое наблюдаемое поведение физического thumb-кластера на всех базовых раскладках и доступность открываемых им overlay-слоёв.

## Requirements

### Requirement: Единая схема thumb-кластера базовых слоёв
Базовые слои `GRAPHITE`, `QWERTY` и `RUSSIAN` SHALL использовать одинаковую семантику на физических позициях 36–41: momentary `SYSTEM_BT`, tap Backspace с hold соответствующего Symbols, tap Delete с hold `NUMBERS`, tap Tab с hold `FUNCTION`, tap Space с hold `NAVIGATION`, затем `none`. На `RUSSIAN` соответствующим Symbols SHALL быть `RUSSIAN_SYMBOLS`; на английских базовых слоях SHALL быть `SYMBOLS`.

#### Scenario: Сравнение физических thumb-позиций
- **WHEN** пользователь переключается между `GRAPHITE`, `QWERTY` и `RUSSIAN`
- **THEN** назначения позиций 36, 38, 39, 40 и 41 остаются одинаковыми
- **AND** позиция 37 отличается только целевым Symbols-слоем согласно языку базового слоя

### Requirement: Доступность общих overlay-слоёв с русского слоя
Каждый общий overlay-слой, открываемый удержанием thumb на `RUSSIAN`, SHALL перекрывать русский базовый слой на время удержания и SHALL переставать его перекрывать после отпускания. Русские буквенные bindings MUST NOT маскировать активный общий overlay.

#### Scenario: System с русского слоя
- **WHEN** активен `RUSSIAN` и удерживается позиция 36
- **THEN** доступен `SYSTEM_BT`, включая BT profile и OS profile bindings
- **AND** после отпускания снова доступен `RUSSIAN`

#### Scenario: Numbers с русского слоя
- **WHEN** активен `RUSSIAN` и позиция 38 удерживается как layer-tap
- **THEN** доступен `NUMBERS`, а не русские буквы на совпадающих физических позициях
- **AND** короткий tap позиции 38 по-прежнему отправляет Delete

#### Scenario: Function с русского слоя
- **WHEN** активен `RUSSIAN` и позиция 39 удерживается как layer-tap
- **THEN** доступен `FUNCTION`, а не русские буквы на совпадающих физических позициях
- **AND** короткий tap позиции 39 по-прежнему отправляет Tab

#### Scenario: Navigation с русского слоя
- **WHEN** активен `RUSSIAN` и позиция 40 удерживается как layer-tap
- **THEN** доступен неизменённый `NAVIGATION`, а не русские буквы на совпадающих физических позициях
- **AND** короткий tap позиции 40 по-прежнему отправляет Space

### Requirement: Доступность языковых Symbols и Smiles
Удержание позиции 37 на `RUSSIAN` SHALL открывать `RUSSIAN_SYMBOLS`, а предусмотренное удержание на `RUSSIAN_SYMBOLS` SHALL открывать `RUSSIAN_SMILES`. Каждый дочерний русский overlay SHALL перекрывать родительский слой только на время удержания.

#### Scenario: Russian Symbols
- **WHEN** активен `RUSSIAN` и позиция 37 удерживается как layer-tap
- **THEN** доступен `RUSSIAN_SYMBOLS`
- **AND** короткий tap позиции 37 по-прежнему отправляет Backspace

#### Scenario: Russian Smiles
- **WHEN** активен `RUSSIAN_SYMBOLS` и удерживается его thumb-переход в `RUSSIAN_SMILES`
- **THEN** доступны русские smiles bindings
- **AND** после отпускания снова доступен `RUSSIAN_SYMBOLS`

### Requirement: Сохранение платформенных функций
Исправление приоритета thumb-слоёв MUST NOT менять содержимое `NAVIGATION`, positional combo, OS profile, Bluetooth actions, набор Auto Shift bindings, language macros, nice!view или ZMK Studio. Изменение тайм-аута Auto Shift определяется отдельным capability `auto-shift-tuning`. `GRAPHITE` SHALL оставаться слоем 0, а три reserved-слоя Studio SHALL оставаться на индексах 12–14.

#### Scenario: Сборка обеих половин
- **WHEN** выполняется штатная build matrix проекта после перестановки слоёв
- **THEN** `corne_left` со Studio snippet и `corne_right` собираются для `nice_nano_v2` с `nice_view_adapter` и `nice_view`
