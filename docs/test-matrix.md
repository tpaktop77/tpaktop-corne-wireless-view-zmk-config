# Test matrix

Статусы:

- `manual pending` — должен выполнить владелец физической клавиатуры; Codex не помечает её выполненной.
- `pass` / `fail` — подтверждённый результат с evidence.

## Static и OpenSpec

| Проверка | Метод | Ожидаемый результат | Статус | Evidence |
|---|---|---|---|---|
| OpenSpec planning | `openspec status --change complete-graphite-zmk-migration` | 4/4 artifacts complete | pass | proposal/design/6 specs/tasks созданы |
| OpenSpec strict | `openspec validate ... --strict` | valid, 0 issues | pass | post-implementation: 1 change passed, 0 failed |
| Whitespace | `git diff --check` | no output, exit 0 | pass | exit 0, no output |
| Layer sizes | structural script | 42 bindings на каждый активный слой | pass | Graphite–System: 9 × 42 |
| Named layer refs | `rg` audit | нет необъяснимых layer numbers | pass | layer behaviors/scopes используют константы |
| Exclusions | `rg` audit | нет Russian/layer2/mouse/OS-layer/persistence | pass | implementation-only search, no matches |

## Build matrix

| Конфигурация | Ожидаемый результат | Статус | Evidence |
|---|---|---|---|
| `nice_nano_v2 + corne_left + nice_view_adapter + nice_view + studio-rpc-usb-uart` | успешная UF2 сборка | pass | follow-up [job 99814284683](https://github.com/tpaktop77/tpaktop-corne-wireless-view-zmk-config/actions/runs/33494759869/job/99814284683) |
| `nice_nano_v2 + corne_right + nice_view_adapter + nice_view` | успешная UF2 сборка | pass | follow-up [job 99814284610](https://github.com/tpaktop77/tpaktop-corne-wireless-view-zmk-config/actions/runs/33494759869/job/99814284610) |

## Layout и переключение

| Проверка | Transport | Ожидаемый результат | Статус |
|---|---|---|---|
| Reboot default | USB и BLE | Graphite активен, QWERTY не выбран | manual pending |
| Graphite 26 letters | USB и BLE | три ряда соответствуют Oryx, Num Lock на position 6 | manual pending |
| Graphite Escape | USB и BLE | position 0 отправляет Escape | manual pending |
| Graphite empty positions | USB и BLE | positions 11,12,23,24,32–34 не печатают keycodes | manual pending |
| Graphite → QWERTY | USB и BLE | position 35 прямо выбирает QWERTY | manual pending |
| QWERTY 26 letters | USB и BLE | только стандартные английские буквы | manual pending |
| QWERTY forbidden body keys | USB и BLE | нет Tab/Esc/Ctrl/Shift/punctuation | manual pending |
| QWERTY → Graphite | USB и BLE | position 35 прямо возвращает Graphite | manual pending |
| Reboot from QWERTY | USB и BLE | после reboot снова Graphite | manual pending |

## Thumb layer-tap

Проверить отдельно на Graphite и QWERTY, по USB и BLE.

| Position | Tap | Hold | Ожидаемый результат | Статус |
|---:|---|---|---|---|
| 36 | — | System | слой активен только до release | manual pending |
| 37 | Backspace | Symbols | tap повторяется через quick-tap; hold открывает Symbols | manual pending |
| 38 | Delete | Numbers | tap удаляет; hold открывает Numbers | manual pending |
| 39 | Tab | Function | tap Tab; hold Function | manual pending |
| 40 | Space | Navigation | tap Space; hold Navigation | manual pending |
| 41 | none | none | ничего не происходит | manual pending |

## Auto Shift

| Проверка | Layout | Transport | Ожидаемый результат | Статус |
|---|---|---|---|---|
| tap <250 ms каждой буквы | Graphite | USB/BLE | lowercase/plain keycode | manual pending |
| hold >250 ms каждой буквы | Graphite | USB/BLE | Shift + та же буква | manual pending |
| tap <250 ms каждой буквы | QWERTY | USB/BLE | lowercase/plain keycode | manual pending |
| hold >250 ms каждой буквы | QWERTY | USB/BLE | Shift + та же буква | manual pending |
| быстрый обычный набор | оба | USB/BLE | нет ложных заглавных букв | manual pending |
| digits/NumLock/thumbs/function/symbols | все | USB/BLE | Auto Shift не применяется | manual pending |
| quick-tap Backspace | оба | USB/BLE | auto-repeat удаления как до Auto Shift | manual pending |

## Combo — обычные и layer actions

Каждая базовая строка проверяется на Graphite и QWERTY по одинаковым физическим positions; arithmetic — только Numbers. Обычный быстрый набор на обоих layout не должен вызывать ложные combo.

| Combo | Positions | Scope | Ожидаемый результат | USB | BLE |
|---|---|---|---|---|---|
| Enter | 19 20 21 | Graphite/QWERTY | Enter | manual pending | manual pending |
| Tab | 14 15 16 | Graphite/QWERTY | Tab | manual pending | manual pending |
| Caps Word | 13 16 | Graphite/QWERTY | toggle Caps Word | manual pending | manual pending |
| Caps Lock | 13 17 | Graphite/QWERTY | Caps Lock | manual pending | manual pending |
| Smiles | 5 17 | Graphite/QWERTY | momentary Smiles до полного release | manual pending | manual pending |
| Vim | 19 22 | Graphite/QWERTY | one-shot Vim | manual pending | manual pending |
| Minus | 7 8 9 | Numbers only | `-` | manual pending | manual pending |
| Equal | 19 20 21 | Numbers only | `=` | manual pending | manual pending |
| Plus | 31 32 33 | Numbers only | `+` | manual pending | manual pending |
| Slash | 19 20 21 22 | Numbers only | `/` | manual pending | manual pending |
| Asterisk | 31 32 33 34 | Numbers only | `*` | manual pending | manual pending |
| arithmetic positions outside Numbers | same | other layers | combo не срабатывает | manual pending | manual pending |

## Combo — slow-release modifiers

Для каждой строки: chord нажимает modifier; отпускание одной входящей клавиши сохраняет modifier; отпускание последней снимает modifier; после теста нет stuck modifier.

| Combo | Positions | Modifier | Graphite | QWERTY | USB/BLE |
|---|---|---|---|---|---|
| Left Alt | 13 25 | LALT | manual pending | manual pending | manual pending |
| Left GUI | 14 26 | LGUI | manual pending | manual pending | manual pending |
| Left Shift | 15 27 | LSHIFT | manual pending | manual pending | manual pending |
| Left Ctrl | 16 28 | LCTRL | manual pending | manual pending | manual pending |
| Right Ctrl | 19 31 | RCTRL | manual pending | manual pending | manual pending |
| Right Shift | 20 32 | RSHIFT | manual pending | manual pending | manual pending |
| Right GUI | 21 33 | RGUI | manual pending | manual pending | manual pending |
| Right Alt | 22 34 | RALT | manual pending | manual pending | manual pending |

Особо проверить Right Shift/GUI/Alt: underlying Graphite positions 32–34 равны `&none`, но combo должны работать.

## Перекрывающиеся combo

| Короткое | Длинное | Transport | Ожидаемый результат | Статус |
|---|---|---|---|---|
| Copy 2+14 | Cut 2+3+14+15 | USB | Cut не вызывает Copy/Paste | manual pending |
| Copy 2+14 | Cut 2+3+14+15 | BLE | Cut не вызывает Copy/Paste | manual pending |
| Equal 19+20+21 | Slash 19+20+21+22 | USB/BLE | Slash не вводит Equal | manual pending |
| Plus 31+32+33 | Asterisk 31+32+33+34 | USB/BLE | Asterisk не вводит Plus | manual pending |
| Enter 19+20+21 | App Switch 19+20+21+22 | USB/BLE | App Switch не отправляет Enter | manual pending |
| Tab/Word BS/Caps Word | Task View 13+14+15+16 | USB/BLE | выполняется только Task View | manual pending |
| только короткий chord | любой pair | USB/BLE | короткий распознаётся после overlap resolution | manual pending |
| быстрый обычный текст | Graphite/QWERTY | USB/BLE | нет ложных combo | manual pending |

## System/Bluetooth

| Проверка | Ожидаемый результат | Статус |
|---|---|---|
| hold left outer thumb | System открывается momentary | manual pending |
| BT0 | выбирается profile 0, OS не меняется | manual pending |
| BT1 | выбирается profile 1, OS не меняется | manual pending |
| BT2 | выбирается profile 2, OS не меняется | manual pending |
| BT3 | выбирается profile 3, OS не меняется | manual pending |
| BT4 | выбирается profile 4, OS не меняется | manual pending |
| BT Clear tap <1500 ms | профиль не очищается | manual pending |
| BT Clear + другая клавиша <1500 ms | профиль не очищается | manual pending |
| BT Clear hold ≥1500 ms | выполняется BT_CLR | manual pending |
| release System thumb | слой закрывается | manual pending |
| OS selector positions | position 18 ничего не делает; positions 19/20/21 выбирают Windows/macOS/Linux | manual pending |

## OS profile и действия

Сначала выбрать профиль на System, затем вызвать каждое positional combo на Graphite и QWERTY. Для release-sensitive проверки удержать action, искусственно сменить профиль, отпустить action и убедиться в отсутствии stuck modifiers.

| Action | Windows | macOS | Linux | USB | BLE |
|---|---|---|---|---|---|
| COPY | Ctrl+C | GUI+C | Ctrl+Insert | manual pending | manual pending |
| PASTE | Ctrl+V | GUI+V | Shift+Insert | manual pending | manual pending |
| CUT | Ctrl+X | GUI+X | Shift+Delete | manual pending | manual pending |
| UNDO | Ctrl+Z | GUI+Z | Ctrl+Z | manual pending | manual pending |
| REDO | Ctrl+Shift+Z | GUI+Shift+Z | Ctrl+Shift+Z | manual pending | manual pending |
| SELECT_ALL | Ctrl+A | GUI+A | Ctrl+A | manual pending | manual pending |
| WORD_PREVIOUS | Ctrl+Left | Alt+Left | Ctrl+Left | manual pending | manual pending |
| WORD_NEXT | Ctrl+Right | Alt+Right | Ctrl+Right | manual pending | manual pending |
| WORD_BACKSPACE | Ctrl+Backspace | Alt+Backspace | Ctrl+Backspace | manual pending | manual pending |
| WORD_DELETE | Ctrl+Delete | Alt+Delete | Ctrl+Delete | manual pending | manual pending |
| APP_SWITCH | Alt+Tab | GUI+Tab | Alt+Tab | manual pending | manual pending |
| TASK_VIEW | GUI+Tab | Ctrl+Up | GUI+Tab | manual pending | manual pending |
| VOICE | GUI+H | `C_VOICE_COMMAND` | GUI+H | manual pending | manual pending |

Дополнительные state tests:

| Проверка | Ожидаемый результат | Статус |
|---|---|---|
| reboot после macOS/Linux | профиль снова Windows | manual pending |
| выбор OS profile | active keymap layer не меняется | manual pending |
| Graphite↔QWERTY | OS profile не меняется | manual pending |
| BT0–BT4 | OS profile не меняется | manual pending |
| action press → OS change → release | освобождается исходный HID code, stuck modifiers нет | manual pending |
| macOS Voice | Siri/voice вызывается consumer usage 0xCF | manual pending |

## Split, nice!view и Studio

| Проверка | Ожидаемый результат | Статус |
|---|---|---|
| левая central half | все behaviors и right-side events работают через split | manual pending |
| правая peripheral half | positions/combo отправляются central, нет reset/disconnect | manual pending |
| nice!view left | отображает понятные display names активных слоёв | manual pending |
| nice!view right | display продолжает работать | manual pending |
| ZMK Studio USB | подключается через central snippet | manual pending |
| Studio reserved layers | доступны три reserved entries | manual pending |
| Studio source-of-truth | перезагрузка возвращает version-controlled behaviors/combo | manual pending |
