# OS profile

OS profile — независимое внутреннее состояние custom ZMK behavior module. Это не keymap layer и не Bluetooth profile.

## Свойства состояния

- Допустимые значения: `OS_WINDOWS`, `OS_MACOS`, `OS_LINUX`.
- Начальное значение после каждого reboot: `OS_WINDOWS`.
- Состояние живёт только в RAM и не сохраняется во flash.
- Нет автоматического определения ОС.
- Нет отдельных Graphite/QWERTY вариантов для ОС.
- Нет связи OS profile с BT0–BT4 или текущим USB/BLE endpoint.
- Переключение Graphite/QWERTY не меняет OS profile.

## Behavior API

Именованные значения экспортируются из repo-local dt-binding header.

```dts
&os_set OS_WINDOWS
&os_set OS_MACOS
&os_set OS_LINUX

&os_action OS_ACTION_COPY
&os_action OS_ACTION_PASTE
```

`&os_set` меняет профиль на press и не генерирует HID event. `&os_action` при press выбирает HID keycode из централизованной таблицы и делегирует press штатному ZMK `&kp`; при release делегирует release того же keycode.

Выбранный keycode хранится до release. Поэтому даже искусственная смена OS profile между press и release не может переключить release на другой modifier и оставить старый зажатым.

## Таблица действий

| Значение | Windows | macOS | Linux |
|---|---|---|---|
| `OS_ACTION_COPY` | Ctrl+C | GUI+C | Ctrl+Insert |
| `OS_ACTION_PASTE` | Ctrl+V | GUI+V | Shift+Insert |
| `OS_ACTION_CUT` | Ctrl+X | GUI+X | Shift+Delete |
| `OS_ACTION_UNDO` | Ctrl+Z | GUI+Z | Ctrl+Z |
| `OS_ACTION_REDO` | Ctrl+Shift+Z | GUI+Shift+Z | Ctrl+Shift+Z |
| `OS_ACTION_SELECT_ALL` | Ctrl+A | GUI+A | Ctrl+A |
| `OS_ACTION_WORD_PREVIOUS` | Ctrl+Left | Alt+Left | Ctrl+Left |
| `OS_ACTION_WORD_NEXT` | Ctrl+Right | Alt+Right | Ctrl+Right |
| `OS_ACTION_WORD_BACKSPACE` | Ctrl+Backspace | Alt+Backspace | Ctrl+Backspace |
| `OS_ACTION_WORD_DELETE` | Ctrl+Delete | Alt+Delete | Ctrl+Delete |
| `OS_ACTION_APP_SWITCH` | Alt+Tab | GUI+Tab | Alt+Tab |
| `OS_ACTION_TASK_VIEW` | GUI+Tab | Ctrl+Up | GUI+Tab |
| `OS_ACTION_VOICE` | GUI+H | Voice Command consumer usage | GUI+H |

## macOS Voice

Для macOS выбран официальный ZMK keycode `C_VOICE_COMMAND`, то есть HID Consumer Voice Command usage `0xCF`. Он совпадает с исходным QMK вызовом `host_consumer_send(0xCF)`. Официальная таблица совместимости ZMK не гарантирует одинаковую реакцию всех версий macOS, поэтому Siri/voice остаётся обязательным аппаратным тестом.

## System layer selectors

На System positions `19`, `20`, `21` находятся:

```text
&os_set OS_WINDOWS
&os_set OS_MACOS
&os_set OS_LINUX
```

Это три соседние клавиши правого home row; position `18` остаётся пустой, а Windows на position `19` находится под указательным пальцем. Behavior работает на central half и не меняет active keymap layer.

## Ограничения текущего этапа

- Профиль не переживает reboot.
- Профиль не определяется хостом автоматически.
- Профиль не выбирается вместе с Bluetooth profile.
- Navigation layer пока не использует `&os_action`: `migrated, OS adaptation deferred`.
- Только перечисленные combo используют OS-aware API.
