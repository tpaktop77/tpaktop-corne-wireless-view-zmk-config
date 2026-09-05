## Purpose

Определяет единый порог Auto Shift, уменьшающий ложные заглавные буквы на текущих физических switches без изменения области применения behavior.

## Requirements

### Requirement: Единый порог Auto Shift 300 мс
Параметризованный Auto Shift behavior SHALL использовать `tapping-term-ms = 300` для всех английских букв `GRAPHITE` и `QWERTY` и всех русских букв `RUSSIAN`. Короткое нажатие SHALL отправлять обычную букву, а удержание дольше 300 мс SHALL отправлять Shift вместе с той же буквой.

#### Scenario: Быстрый обычный набор
- **WHEN** пользователь нажимает букву короче 300 мс на `GRAPHITE`, `QWERTY` или `RUSSIAN`
- **THEN** отправляется обычная буква без Shift

#### Scenario: Намеренный Auto Shift
- **WHEN** пользователь удерживает букву дольше 300 мс на `GRAPHITE`, `QWERTY` или `RUSSIAN`
- **THEN** отправляется Shift вместе с этой буквой

### Requirement: Независимость thumb layer-tap
Настройка Auto Shift MUST NOT менять `&lt.quick-tap-ms = 250` или применять Auto Shift к цифрам, Num Lock, пустым позициям, layer switches, thumb layer-tap, Bluetooth, OS selectors, функциональным клавишам, символам или смайлам.

#### Scenario: Быстрое повторение thumb tap-кода
- **WHEN** пользователь повторно нажимает thumb layer-tap в пределах его quick-tap окна
- **THEN** сохраняется прежняя семантика `&lt.quick-tap-ms = 250`
- **AND** новый порог Auto Shift 300 мс на неё не влияет
