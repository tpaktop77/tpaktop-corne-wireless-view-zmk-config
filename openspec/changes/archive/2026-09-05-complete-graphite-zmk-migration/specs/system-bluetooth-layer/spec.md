## Purpose

Определяет безопасный специализированный System layer для управления Bluetooth-профилями и независимого выбора текущего профиля операционной системы.

## ADDED Requirements

### Requirement: System layer открывается только momentary thumb-клавишей
Левый внешний thumb на Graphite и QWERTY SHALL активировать `SYSTEM_BT` только пока thumb удерживается. Старые общие Lower и Raise SHALL отсутствовать, если на них больше нет ссылок.

#### Scenario: Удержание System thumb
- **WHEN** пользователь удерживает левый внешний thumb
- **THEN** `SYSTEM_BT` активен до отпускания thumb и закрывается после отпускания

### Requirement: System layer предоставляет пять Bluetooth-профилей
Верхний ряд левой половины System SHALL содержать подряд `BT_SEL 0`, `BT_SEL 1`, `BT_SEL 2`, `BT_SEL 3`, `BT_SEL 4` в понятном порядке от внешней стороны к внутренней.

#### Scenario: Выбор каждого BT profile
- **WHEN** пользователь удерживает System thumb и нажимает одну из пяти клавиш BT0–BT4
- **THEN** ZMK выбирает соответствующий Bluetooth profile без изменения OS profile

### Requirement: Bluetooth Clear защищён длительным удержанием
Bluetooth Clear SHALL находиться в нижнем внешнем левом углу System и MUST NOT выполняться обычным коротким tap. Очистка SHALL выполняться только после непрерывного удержания примерно 1500 мс.

#### Scenario: Короткий tap Bluetooth Clear
- **WHEN** пользователь нажимает и отпускает BT Clear раньше 1500 мс
- **THEN** Bluetooth profile не очищается

#### Scenario: Подтверждённый Bluetooth Clear
- **WHEN** пользователь удерживает BT Clear не менее 1500 мс
- **THEN** выполняется стандартная команда ZMK `BT_CLR`

### Requirement: System layer предоставляет независимые OS selectors
Три соседние клавиши правого home row на positions `19`, `20`, `21` SHALL выбирать Windows, macOS и Linux соответственно. Position `18` SHALL оставаться пустой, чтобы Windows находилась под указательным пальцем на home middle row. Выбор ОС MUST NOT выбирать Bluetooth profile, менять буквенный слой или автоматически связываться с endpoint.

#### Scenario: Выбор ОС
- **WHEN** пользователь удерживает System thumb и нажимает Windows, macOS или Linux
- **THEN** меняется только внутренний OS profile
