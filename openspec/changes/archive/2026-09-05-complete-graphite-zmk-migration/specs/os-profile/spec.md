## Purpose

Определяет независимый volatile OS profile и единый behavior API для системных действий с корректными HID press/release на Windows, macOS и Linux.

## ADDED Requirements

### Requirement: OS profile является независимым volatile-состоянием
Система SHALL хранить один из профилей Windows, macOS или Linux независимо от keymap layers и Bluetooth profiles. После каждого reboot профиль SHALL быть Windows. Профиль MUST NOT сохраняться во flash и MUST NOT определяться автоматически хостом.

#### Scenario: Значение после reboot
- **WHEN** клавиатура запускается или перезапускается
- **THEN** текущий OS profile равен Windows независимо от ранее выбранного значения

#### Scenario: Независимость от раскладки и Bluetooth
- **WHEN** пользователь меняет Graphite/QWERTY или BT0–BT4
- **THEN** текущий OS profile не меняется

### Requirement: os_set выбирает профиль без переключения слоёв
Behavior `&os_set <profile>` SHALL принимать именованные значения `OS_WINDOWS`, `OS_MACOS`, `OS_LINUX` и при press менять только текущий OS profile.

#### Scenario: Последовательный выбор профилей
- **WHEN** вызываются `&os_set OS_MACOS`, затем `&os_set OS_LINUX`, затем `&os_set OS_WINDOWS`
- **THEN** внутреннее состояние последовательно принимает macOS, Linux и Windows без изменения active layer

### Requirement: os_action централизованно отображает действия на HID
Behavior `&os_action <action>` SHALL поддерживать COPY, PASTE, CUT, UNDO, REDO, SELECT_ALL, WORD_PREVIOUS, WORD_NEXT, WORD_BACKSPACE, WORD_DELETE, APP_SWITCH, TASK_VIEW и VOICE по следующей таблице. OS-specific сочетания MUST быть определены в одном централизованном месте, а keymap MUST ссылаться только на именованные action values.

| Action | Windows | macOS | Linux |
|---|---|---|---|
| COPY | Ctrl+C | GUI+C | Ctrl+Insert |
| PASTE | Ctrl+V | GUI+V | Shift+Insert |
| CUT | Ctrl+X | GUI+X | Shift+Delete |
| UNDO | Ctrl+Z | GUI+Z | Ctrl+Z |
| REDO | Ctrl+Shift+Z | GUI+Shift+Z | Ctrl+Shift+Z |
| SELECT_ALL | Ctrl+A | GUI+A | Ctrl+A |
| WORD_PREVIOUS | Ctrl+Left | Alt+Left | Ctrl+Left |
| WORD_NEXT | Ctrl+Right | Alt+Right | Ctrl+Right |
| WORD_BACKSPACE | Ctrl+Backspace | Alt+Backspace | Ctrl+Backspace |
| WORD_DELETE | Ctrl+Delete | Alt+Delete | Ctrl+Delete |
| APP_SWITCH | Alt+Tab | GUI+Tab | Alt+Tab |
| TASK_VIEW | GUI+Tab | Ctrl+Up | GUI+Tab |
| VOICE | GUI+H | Voice Command consumer usage | GUI+H |

#### Scenario: Выполнение действия для каждого профиля
- **WHEN** пользователь выбирает профиль и вызывает любое поддерживаемое `&os_action`
- **THEN** отправляется соответствующее таблице keyboard или consumer HID действие

### Requirement: os_action симметрично обрабатывает press и release
Каждый action SHALL нажимать выбранный HID usage при behavior press и отпускать тот же usage при behavior release. Смена OS profile между press и release MUST NOT оставлять клавишу или модификатор зажатыми.

#### Scenario: Смена профиля при удерживаемом action
- **WHEN** action был нажат в одном OS profile, профиль изменился, затем action был отпущен
- **THEN** отпускается код, выбранный при press, и HID report не содержит зависших модификаторов

### Requirement: API использует собственный dt-binding contract
OS profiles и actions SHALL экспортироваться keymap через version-controlled dt-binding header с именованными значениями; отдельные OS layers MUST NOT создаваться.

#### Scenario: Проверка keymap API
- **WHEN** keymap определяет OS selectors и OS-aware combo
- **THEN** он использует `&os_set OS_*` и `&os_action OS_ACTION_*` без числовых magic values
