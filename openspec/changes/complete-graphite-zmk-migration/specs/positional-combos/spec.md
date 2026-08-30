## Purpose

Определяет перенос Oryx combo по физическим позициям с одинаковым поведением на Graphite/QWERTY, корректной вложенностью и release-семантикой.

## ADDED Requirements

### Requirement: Буквенные combo работают по одинаковым позициям двух раскладок
Все базовые английские combo SHALL использовать `key-positions` и `layers = <GRAPHITE QWERTY>`. Они MUST NOT зависеть от букв, keycodes или фиктивных F22/F23/F24 и MUST NOT быть активны на вспомогательных слоях.

#### Scenario: Одинаковый chord на Graphite и QWERTY
- **WHEN** пользователь нажимает одну и ту же физическую комбинацию на Graphite и QWERTY
- **THEN** срабатывает одно и то же combo-действие

#### Scenario: Позиции бывших F22/F23/F24
- **WHEN** Graphite содержит `&none` на позициях 32–34
- **THEN** combo, включающие эти позиции, продолжают распознаваться по `key-positions`

### Requirement: Числовые combo ограничены Numbers
Пять arithmetic combo SHALL сохранять физические позиции и результаты `-`, `=`, `+`, `/`, `*`, но SHALL иметь только `layers = <NUMBERS>`.

#### Scenario: Arithmetic combo на Numbers
- **WHEN** пользователь вводит соответствующий трёх- или четырёхклавишный chord на Numbers
- **THEN** выводится заданный арифметический оператор

#### Scenario: Arithmetic chord вне Numbers
- **WHEN** те же позиции нажаты на другом слое
- **THEN** arithmetic combo не срабатывает

### Requirement: Системные combo используют os_action
Combo copy, paste, cut, undo, redo, voice, word_backspace, word_delete, word_previous, word_next, app_switch, task_view и select_all SHALL вызывать соответствующие `&os_action OS_ACTION_*`. Их имена MUST NOT содержать `windows`.

#### Scenario: OS-aware combo после смены профиля
- **WHEN** пользователь меняет OS profile и активирует системное combo
- **THEN** физическая комбинация не меняется, а HID действие соответствует новому профилю

### Requirement: Удерживаемые combo используют slow-release
Восемь modifier combo и momentary Smiles combo SHALL иметь `slow-release`. Модификатор или Smiles layer SHALL оставаться активным после отпускания одной входящей клавиши и SHALL отпускаться после последней. Обычные command combo MUST NOT получать `slow-release` автоматически.

#### Scenario: Modifier release
- **WHEN** modifier combo сработало и пользователь отпускает chord по одной клавише
- **THEN** модификатор остаётся нажат до отпускания последней клавиши и затем полностью отпускается

#### Scenario: Smiles release
- **WHEN** Smiles combo сработало и часть chord ещё удерживается
- **THEN** Smiles остаётся активным до полного отпускания chord

### Requirement: Вложенные combo сохраняются
Copy SHALL сосуществовать с Cut, `4+5+6` с `4+5+6+Dot`, а `1+2+3` с `1+2+3+Equal`. Более длинное combo MUST распознаваться без ошибочного выполнения вложенного короткого combo. Начальный timeout SHALL быть 50 мс.

#### Scenario: Распознавание длинного combo
- **WHEN** все клавиши длинного chord нажаты в пределах 50 мс
- **THEN** выполняется длинное действие, а вложенное короткое действие не выполняется

#### Scenario: Распознавание короткого combo
- **WHEN** нажаты только клавиши короткого chord
- **THEN** после разрешения overlap выполняется короткое действие
