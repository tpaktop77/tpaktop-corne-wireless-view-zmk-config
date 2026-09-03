# Test matrix

Статусы:

- `manual pending` — должен выполнить владелец физической клавиатуры; Codex не помечает её выполненной.
- `pass` / `fail` — подтверждённый результат с evidence.

## Static и OpenSpec

| Проверка | Метод | Ожидаемый результат | Статус | Evidence |
|---|---|---|---|---|
| OpenSpec planning | `openspec status --change complete-graphite-zmk-migration` | 4/4 artifacts complete | pass | proposal/design/6 specs/tasks созданы |
| Russian OpenSpec planning | `openspec status --change migrate-russian-layers` | 4/4 artifacts complete | pass | proposal/design/4 specs/tasks созданы |
| OpenSpec strict | `openspec validate migrate-russian-layers --strict` | valid, 0 issues | pass | `Change 'migrate-russian-layers' is valid` |
| Thumb priority OpenSpec planning | `openspec status --change fix-russian-thumb-layer-priority` | 4/4 artifacts complete | pass | proposal/design/spec/tasks созданы в отдельном follow-up change |
| Thumb priority OpenSpec strict | `openspec validate fix-russian-thumb-layer-priority --strict` | valid, 0 issues | pass | `Change 'fix-russian-thumb-layer-priority' is valid` |
| Whitespace | `git diff --check` | no output, exit 0 | pass | exit 0, no output |
| Layer sizes | structural script | 42 bindings на каждый активный слой | pass | Graphite–Russian Smiles: 12 × 42 |
| Named layer refs | `rg` audit | нет необъяснимых layer numbers | pass | layer behaviors/scopes используют константы |
| Layer priority | structural audit | `GRAPHITE 0`, `QWERTY 1`, `RUSSIAN 2`; все thumb overlays выше base; node order совпадает с defines | pass | defines/nodes совпали; все topological invariants pass |
| Exclusions | `rg` audit | нет layer2/mouse/OS-layer/language persistence/custom Russian Caps Word | pass | implementation-only search; no new matches |

## Build matrix

| Конфигурация | Ожидаемый результат | Статус | Evidence |
|---|---|---|---|
| `nice_nano_v2 + corne_left + nice_view_adapter + nice_view + studio-rpc-usb-uart` | успешная UF2 сборка | pass | Russian migration commit `6f2c898`; [job 100582310443](https://github.com/tpaktop77/tpaktop-corne-wireless-view-zmk-config/actions/runs/33734651351/job/100582310443), West Build/Kconfig/Devicetree/artifact pass |
| `nice_nano_v2 + corne_right + nice_view_adapter + nice_view` | успешная UF2 сборка | pass | Russian migration commit `6f2c898`; [job 100582310482](https://github.com/tpaktop77/tpaktop-corne-wireless-view-zmk-config/actions/runs/33734651351/job/100582310482), West Build/Kconfig/Devicetree/artifact pass |

Run `33734651351` относится к change `migrate-russian-layers`; обе UF2 включены в объединённый firmware artifact.

Для `fix-russian-thumb-layer-priority` требуется отдельная повторная сборка: предыдущий CI подтверждает синтаксис исходной миграции, но не новое расположение nodes.

## Layout и переключение

| Проверка | Transport | Ожидаемый результат | Статус |
|---|---|---|---|
| Reboot default | USB и BLE | Graphite активен, QWERTY не выбран | manual pending |
| Graphite 26 letters | USB и BLE | три ряда соответствуют Oryx, Num Lock на position 6 | manual pending |
| Graphite Escape | USB и BLE | position 0 отправляет Escape | manual pending |
| Graphite empty positions | USB и BLE | positions 11,12,23,32–34 не печатают keycodes | manual pending |
| Graphite → Russian | USB и BLE | position 24 отправляет Ctrl+Shift+2 и прямо выбирает Russian | manual pending |
| Graphite → QWERTY | USB и BLE | position 35 прямо выбирает QWERTY | manual pending |
| QWERTY 26 letters | USB и BLE | только стандартные английские буквы | manual pending |
| QWERTY forbidden body keys | USB и BLE | нет Tab/Esc/Ctrl/Shift/punctuation | manual pending |
| QWERTY → Graphite | USB и BLE | position 35 прямо возвращает Graphite | manual pending |
| Reboot from QWERTY | USB и BLE | после reboot снова Graphite | manual pending |

## Russian layout и язык хоста

Precondition для каждого Windows/macOS/Linux хоста: `Ctrl+Shift+1` прямо выбирает English, `Ctrl+Shift+2` — Russian–PC. Повторить функциональные строки по USB и BLE; для split отдельно вводить клавиши обеих половин.

| Проверка | Transport/OS | Ожидаемый результат | Статус |
|---|---|---|---|
| Russian 33 letters | USB/BLE, Windows | позиции 1–11, 13–23, 25–35 печатают точные строчные `йцукенгшщзх / фывапролджэ / ячсмитьбюёъ` | manual pending |
| Russian 33 letters | USB/BLE, macOS | тот же positional output при Russian–PC | manual pending |
| Russian 33 letters | USB/BLE, Linux | тот же positional output при Russian–PC | manual pending |
| Russian position 0 fallback | USB/BLE | прозрачная клавиша отправляет Graphite Escape | manual pending |
| Russian position 12 fallback | USB/BLE | прозрачная клавиша падает в Graphite none и ничего не отправляет | manual pending |
| Graphite language combo | USB/BLE | positions 7+8+9 отправляют Ctrl+Shift+2 и активируют Russian | manual pending |
| Russian language combo | USB/BLE | positions 7+8+9 отправляют Ctrl+Shift+1 и активируют Graphite | manual pending |
| Russian → Graphite | USB/BLE | position 24 выполняет тот же направленный выход | manual pending |
| QWERTY independence | USB/BLE | language combo не включено на QWERTY; русский выход всегда ведёт в Graphite | manual pending |
| External host language change | USB/BLE | после ручной рассинхронизации направленный вход/выход снова задаёт ожидаемую пару layer+language | manual pending |
| Reboot from Russian | USB/BLE | base layer снова Graphite, OS profile снова Windows; host language до macro не заявляется синхронным | manual pending |

## Russian Symbols и Smiles

| Проверка | Transport | Ожидаемый результат | Статус |
|---|---|---|---|
| Russian Symbols row 0 | USB/BLE | positions 3,4,6–11 печатают `( ) № _ - / \\ -` при Russian–PC | manual pending |
| Russian Symbols row 1 | USB/BLE | positions 19–23 печатают `. , ! ? /` при Russian–PC | manual pending |
| Russian Symbols row 2 | USB/BLE | positions 31–35 печатают `" : ; = *` при Russian–PC | manual pending |
| Symbols top-row exclusions | USB/BLE | `%`, `₽`, `+` не были перенесены на новые позиции | manual pending |
| Symbols transparent thumbs | USB/BLE | positions 36–39/41 наследуют Russian, position 40 tap = Space | manual pending |
| Russian Smiles thumb hold | USB/BLE | удержание position 40 на Symbols открывает Russian Smiles до release | manual pending |
| Russian smile happy | USB/BLE | Russian Smiles position 19 печатает `:)` | manual pending |
| Russian smile sad | USB/BLE | Russian Smiles position 20 печатает `:(` | manual pending |
| Russian Smiles combo | USB/BLE | Russian positions 5+17 держат Russian Smiles до отпускания последней клавиши | manual pending |

## Thumb layer-tap

Проверить отдельно на Graphite, QWERTY и Russian, по USB и BLE. На Russian position 37 открывает Russian Symbols; остальные назначения совпадают.

| Position | Tap | Hold | Ожидаемый результат | Статус |
|---:|---|---|---|---|
| 36 | — | System | слой активен только до release | manual pending |
| 37 | Backspace | Symbols | tap повторяется через quick-tap; hold открывает Symbols | manual pending |
| 38 | Delete | Numbers | tap удаляет; hold открывает Numbers | manual pending |
| 39 | Tab | Function | tap Tab; hold Function | manual pending |
| 40 | Space | Navigation | tap Space; hold Navigation | manual pending |
| 41 | none | none | ничего не происходит | manual pending |

### Регрессия thumb-кластера на Russian

PR #5 собирался успешно, но аппаратно `RUSSIAN = 9` маскировал более низкий `NAVIGATION = 3`. После исправления повторить каждую строку отдельно по USB и BLE; проверки на Graphite и QWERTY служат контролем, что их прежнее поведение сохранилось.

| Position | Hold target | Проверка на Russian | До исправления | USB после исправления | BLE после исправления |
|---:|---|---|---|---|---|
| 36 | System | видны BT0–BT4 и OS selectors; release возвращает Russian | не проверено | manual pending | manual pending |
| 37 | Russian Symbols | видны русские символы; tap остаётся Backspace | работало | manual pending | manual pending |
| 38 | Numbers | видны цифры; tap остаётся Delete | не проверено | manual pending | manual pending |
| 39 | Function | видны F-клавиши; tap остаётся Tab | не проверено | manual pending | manual pending |
| 40 | Navigation | стрелки/навигация перекрывают русские буквы; tap остаётся Space | fail: оставался Russian | manual pending | manual pending |
| 41 | none | ничего не происходит | не проверено | manual pending | manual pending |

Дополнительно на `RUSSIAN_SYMBOLS` удержание position 40 SHALL открывать `RUSSIAN_SMILES` и возвращать Symbols после release; проверить по USB и BLE (`manual pending`).

## Auto Shift

| Проверка | Layout | Transport | Ожидаемый результат | Статус |
|---|---|---|---|---|
| tap <300 ms каждой буквы | Graphite | USB/BLE | lowercase/plain keycode | manual pending |
| hold >300 ms каждой буквы | Graphite | USB/BLE | Shift + та же буква | manual pending |
| tap <300 ms каждой буквы | QWERTY | USB/BLE | lowercase/plain keycode | manual pending |
| hold >300 ms каждой буквы | QWERTY | USB/BLE | Shift + та же буква | manual pending |
| быстрый обычный набор | Graphite/QWERTY/Russian | USB/BLE | ложные заглавные буквы, наблюдавшиеся при 250 мс, больше не возникают | manual pending |
| digits/NumLock/thumbs/function/symbols | все | USB/BLE | Auto Shift не применяется | manual pending |
| quick-tap Backspace | Graphite/QWERTY/Russian | USB/BLE | `&lt.quick-tap-ms = 250` и auto-repeat удаления не изменились | manual pending |
| tap <300 ms каждой из 33 букв | Russian | USB/BLE | строчная Russian–PC буква | manual pending |
| hold >300 ms каждой из 33 букв | Russian | USB/BLE | Shift + тот же usage формирует заглавную букву | manual pending |
| Russian symbols/smiles/thumbs/language actions | Russian | USB/BLE | Auto Shift не применяется | manual pending |

## Combo — обычные и layer actions

Каждая совместимая базовая строка проверяется на Graphite, QWERTY и Russian по одинаковым физическим positions; arithmetic — только Numbers. Обычный быстрый набор на всех трёх layout не должен вызывать ложные combo.

| Combo | Positions | Scope | Ожидаемый результат | USB | BLE |
|---|---|---|---|---|---|
| Enter | 19 20 21 | Graphite/QWERTY/Russian | Enter | manual pending | manual pending |
| Tab | 14 15 16 | Graphite/QWERTY/Russian | Tab | manual pending | manual pending |
| Caps Word | 13 16 | Graphite/QWERTY/Russian | toggle Caps Word | manual pending | manual pending |
| Caps Lock | 13 17 | Graphite/QWERTY/Russian | Caps Lock | manual pending | manual pending |
| Smiles | 5 17 | Graphite/QWERTY | momentary Smiles до полного release | manual pending | manual pending |
| Russian Smiles | 5 17 | Russian | momentary Russian Smiles до полного release | manual pending | manual pending |
| Vim | 19 22 | Graphite/QWERTY | one-shot Vim | manual pending | manual pending |
| Vim positions | 19 22 | Russian | Vim не активируется | manual pending | manual pending |
| To Russian | 7 8 9 | Graphite | Ctrl+Shift+2, затем Russian | manual pending | manual pending |
| To English | 7 8 9 | Russian | Ctrl+Shift+1, затем Graphite | manual pending | manual pending |
| Minus | 7 8 9 | Numbers only | `-` | manual pending | manual pending |
| Equal | 19 20 21 | Numbers only | `=` | manual pending | manual pending |
| Plus | 31 32 33 | Numbers only | `+` | manual pending | manual pending |
| Slash | 19 20 21 22 | Numbers only | `/` | manual pending | manual pending |
| Asterisk | 31 32 33 34 | Numbers only | `*` | manual pending | manual pending |
| arithmetic positions outside Numbers | same | other layers | combo не срабатывает | manual pending | manual pending |

## Combo — slow-release modifiers

Для каждой строки: chord нажимает modifier; отпускание одной входящей клавиши сохраняет modifier; отпускание последней снимает modifier; после теста нет stuck modifier.

| Combo | Positions | Modifier | Graphite | QWERTY | Russian | USB/BLE |
|---|---|---|---|---|---|---|
| Left Alt | 13 25 | LALT | manual pending | manual pending | manual pending | manual pending |
| Left GUI | 14 26 | LGUI | manual pending | manual pending | manual pending | manual pending |
| Left Shift | 15 27 | LSHIFT | manual pending | manual pending | manual pending | manual pending |
| Left Ctrl | 16 28 | LCTRL | manual pending | manual pending | manual pending | manual pending |
| Right Ctrl | 19 31 | RCTRL | manual pending | manual pending | manual pending | manual pending |
| Right Shift | 20 32 | RSHIFT | manual pending | manual pending | manual pending | manual pending |
| Right GUI | 21 33 | RGUI | manual pending | manual pending | manual pending | manual pending |
| Right Alt | 22 34 | RALT | manual pending | manual pending | manual pending | manual pending |

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
| быстрый обычный текст | Russian | USB/BLE | нет ложных combo | manual pending |

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

Сначала выбрать профиль на System, затем вызвать каждое positional combo на Graphite, QWERTY и Russian. Для release-sensitive проверки удержать action, искусственно сменить профиль, отпустить action и убедиться в отсутствии stuck modifiers.

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
| language macro | OS и BT profiles не меняются | manual pending |
| OS selector while Russian active | Russian layer и host language не меняются | manual pending |
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
| Studio Russian layers | видны display names Russian, Russian Symbols, Russian Smiles; reserved entries остаются тремя | manual pending |
| Studio source-of-truth | перезагрузка возвращает version-controlled behaviors/combo | manual pending |

## Временный Russian Caps Word

Это намеренно ограниченный этап на встроенном ZMK v0.3 `continue-list`, не тест полного custom behavior.

| Проверка | Transport | Ожидаемый результат | Статус |
|---|---|---|---|
| Caps Word + обычные HID A–Z Russian letters | USB/BLE | соответствующие русские буквы автоматически заглавные | manual pending |
| Caps Word + Х | USB/BLE | `х` остаётся строчной, но режим не завершается | manual pending |
| Caps Word + Ъ | USB/BLE | `ъ` остаётся строчной, но режим не завершается | manual pending |
| Caps Word + Ж | USB/BLE | `ж` остаётся строчной, но режим не завершается | manual pending |
| Caps Word + Э | USB/BLE | `э` остаётся строчной, но режим не завершается | manual pending |
| Caps Word + Б | USB/BLE | `б` остаётся строчной, но режим не завершается | manual pending |
| Caps Word + Ю | USB/BLE | `ю` остаётся строчной, но режим не завершается | manual pending |
| Caps Word + Ё | USB/BLE | `ё` остаётся строчной, но режим не завершается | manual pending |
| Caps Word + exception + A–Z-backed letter | USB/BLE | следующая поддерживаемая буква всё ещё автоматически заглавная | manual pending |
| Caps Lock + все 33 буквы | USB/BLE | все буквы формируются заглавными через host Caps Lock | manual pending |
| Custom Russian Caps Word | source audit | отсутствует; следующий change `implement-russian-caps-word` | deferred |
