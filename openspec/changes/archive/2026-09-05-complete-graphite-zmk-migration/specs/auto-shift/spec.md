## Purpose

Определяет Auto Shift для всех английских букв Graphite и QWERTY как отдельное от thumb layer-tap параметризованное hold-tap поведение.

## ADDED Requirements

### Requirement: Английские буквы используют Auto Shift 250 мс
Каждая буква Graphite и QWERTY SHALL использовать одно параметризованное Auto Shift behavior с `tapping-term-ms = 250`. Tap короче порога SHALL отправлять обычную букву; непрерывное удержание дольше порога SHALL отправлять Left Shift + ту же букву.

#### Scenario: Короткий tap буквы
- **WHEN** пользователь нажимает и отпускает английскую букву раньше 250 мс
- **THEN** хост получает букву без Shift

#### Scenario: Длительное удержание буквы
- **WHEN** пользователь удерживает английскую букву дольше 250 мс
- **THEN** хост получает ту же букву с Left Shift

### Requirement: Auto Shift применяется только к буквам
Auto Shift MUST NOT применяться к цифрам, Num Lock, `&none`, переключателям Graphite/QWERTY, thumb layer-tap, Bluetooth, OS selectors, функциональным клавишам или готовым shifted symbols.

#### Scenario: Небуквенная клавиша
- **WHEN** пользователь удерживает любую небуквенную клавишу дольше 250 мс
- **THEN** Auto Shift не меняет её действие

### Requirement: Auto Shift независим от thumb quick tap
Auto Shift SHALL иметь собственный hold-tap instance и собственный порог 250 мс. Существующий `&lt.quick-tap-ms = 250` SHALL продолжать управлять только thumb layer-tap повтором.

#### Scenario: Повтор Backspace thumb
- **WHEN** пользователь быстро повторно нажимает и удерживает thumb Backspace/Symbols
- **THEN** работает настройка `&lt.quick-tap-ms`, а Auto Shift не участвует
