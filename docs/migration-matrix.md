# Матрица миграции QMK/Oryx → ZMK

Reference source: `tpaktop77/oryx-with-custom-qmk`, remote `main@5d674ed4d866b66e34389262092cc5347da90bc7`. Target base: `master@8e885d4d28117703abbc0b027bd9686bb9c7a84e`, содержащий смерженный combo PR #2.

Основная миграция прошла статический аудит и сборку обеих половин. Follow-up с Escape и сдвигом OS selectors отмечен `implemented; CI pending` до повторной сборки; аппаратные проверки остаются владельцу согласно `docs/test-matrix.md`.

## Слои и общие behaviors

| Source QMK file | Source QMK layer | Source physical position | Source behavior | Target ZMK layer | Target key-position | Target behavior | Status | Validation |
|---|---:|---|---|---|---|---|---|---|
| `6JP4n/keymap.c` + follow-up | 0 | три нижних ряда Voyager | Graphite body + Escape | `GRAPHITE` 0 | 0–35 | Graphite с `&kp ESC` на position 0, верхний ряд отброшен | implemented; CI pending | layout audit + hardware |
| — | new | Corne body | минимальный QWERTY | `QWERTY` 1 | 0–35 | letters/none + `&to GRAPHITE` | implemented; CI pass | layout audit + hardware |
| `6JP4n/keymap.c` | 0 | Graphite bottom-right | `TT(2)` в QMK | `GRAPHITE` | 35 | `&to QWERTY` | implemented; CI pass | manual switch |
| — | new | QWERTY bottom-right | возврат | `QWERTY` | 35 | `&to GRAPHITE` | implemented; CI pass | manual switch |
| `6JP4n/keymap.c` | 0 | thumbs | layer taps | `GRAPHITE`, `QWERTY` | 36–41 | System; BSP/Symbols; DEL/Numbers; TAB/Function; Space/Navigation; none | implemented; CI pass | manual thumb matrix |
| `6JP4n/keymap.c` | 3 | три нижних ряда | Numbers | `NUMBERS` 2 | 0–41 | сохранён, ссылки именованы | implemented; CI pass | build + manual |
| `6JP4n/keymap.c` | 4 | три нижних ряда | Navigation/media | `NAVIGATION` 3 | 0–41 | migrated, OS adaptation deferred | implemented; CI pass | build + regression |
| `6JP4n/keymap.c` | 5 | три нижних ряда | Function | `FUNCTION` 4 | 0–41 | сохранён, VIM ref исправлен | implemented; CI pass | build + manual |
| `6JP4n/keymap.c` | 6 | три нижних ряда | Vim/macros | `VIM` 5 | 0–41 | сохранён | implemented; CI pass | build + manual |
| `6JP4n/keymap.c` | 7 | три нижних ряда | English Symbols | `SYMBOLS` 6 | 0–41 | сохранён, Smiles ref исправлен | implemented; CI pass | build + manual |
| `6JP4n/keymap.c` | 9 | три нижних ряда | English Smiles | `SMILES` 7 | 0–41 | сохранён | implemented; CI pass | build + manual |
| prior ZMK Lower + new | — | left/right body | BT + OS controls | `SYSTEM_BT` 8 | 0–35 | BT0–BT4, protected clear, OS selectors 19–21 | implemented; CI pending | build + manual |
| target Studio | — | reserved nodes | editable capacity | 9–11 | n/a | три `status = "reserved"` | implemented; CI pass | Studio connect |
| `6JP4n/config.h` | global | English letters | QMK Auto Shift 200 ms | `GRAPHITE`, `QWERTY` | all letter positions | ZMK hold-tap Auto Shift 250 ms | implemented; CI pass | manual USB/BLE |

## System/Bluetooth

| Source QMK file | Source QMK layer | Source physical position | Source behavior | Target ZMK layer | Target key-position | Target behavior | Status | Validation |
|---|---:|---|---|---|---:|---|---|---|
| prior target Lower | 1 | left top row | `BT_SEL 0` | `SYSTEM_BT` | 0 | `&bt BT_SEL 0` | implemented; CI pass | manual USB/BLE |
| prior target Lower | 1 | left top row | `BT_SEL 1` | `SYSTEM_BT` | 1 | `&bt BT_SEL 1` | implemented; CI pass | manual USB/BLE |
| prior target Lower | 1 | left top row | `BT_SEL 2` | `SYSTEM_BT` | 2 | `&bt BT_SEL 2` | implemented; CI pass | manual USB/BLE |
| prior target Lower | 1 | left top row | `BT_SEL 3` | `SYSTEM_BT` | 3 | `&bt BT_SEL 3` | implemented; CI pass | manual USB/BLE |
| prior target Lower | 1 | left top row | `BT_SEL 4` | `SYSTEM_BT` | 4 | `&bt BT_SEL 4` | implemented; CI pass | manual USB/BLE |
| prior target Lower | 1 | lower external left | unprotected `BT_CLR` | `SYSTEM_BT` | 24 | tap-preferred 1500 ms hold → BT Clear | implemented; CI pass | short/long manual |
| `modules/switch_cases.c` concept | global | right home index | detected OS | `SYSTEM_BT` | 19 | `&os_set OS_WINDOWS` | implemented; CI pending | state/action test |
| `modules/switch_cases.c` concept | global | right home | detected OS | `SYSTEM_BT` | 20 | `&os_set OS_MACOS` | implemented; CI pending | state/action test |
| `modules/switch_cases.c` concept | global | right home | detected OS | `SYSTEM_BT` | 21 | `&os_set OS_LINUX` | implemented; CI pending | state/action test |

## Positional combo

Все Source chords перечислены буквами Oryx Graphite, но target определяется только `key-positions`. Базовый scope: `<GRAPHITE QWERTY>`. Arithmetic scope: `<NUMBERS>`.

| Source QMK file | Source QMK layer | Source physical position | Source behavior | Target ZMK layer | Target key-position | Target behavior | Status | Validation |
|---|---:|---|---|---|---|---|---|---|
| `modules/combos.c` | 0 | H+A+E | Enter | Graphite/QWERTY | 19 20 21 | `&kp RET` | implemented; CI pass | USB/BLE + overlap |
| `modules/combos.c` | 0 | R+T+S | Tab | Graphite/QWERTY | 14 15 16 | `&kp TAB` | implemented; CI pass | USB/BLE + overlap |
| `modules/combos.c` | 0 | N+S | Caps Word | Graphite/QWERTY | 13 16 | `&caps_word` | implemented; CI pass | manual |
| `modules/combos.c` | 0 | N+G | Caps Lock | Graphite/QWERTY | 13 17 | `&kp CAPSLOCK` | implemented; CI pass | manual |
| `modules/combos.c` | 3 | 7+8+9 | Minus | Numbers | 7 8 9 | `&kp MINUS` | implemented; CI pass | USB/BLE |
| `modules/combos.c` | 3 | 4+5+6 | Equal | Numbers | 19 20 21 | `&kp EQUAL` | implemented; CI pass | overlap USB/BLE |
| `modules/combos.c` | 3 | 1+2+3 | Plus | Numbers | 31 32 33 | `&kp PLUS` | implemented; CI pass | overlap USB/BLE |
| `modules/combos.c` | 3 | 4+5+6+Dot | Slash | Numbers | 19 20 21 22 | `&kp FSLH` | implemented; CI pass | overlap USB/BLE |
| `modules/combos.c` | 3 | 1+2+3+Equal | Asterisk | Numbers | 31 32 33 34 | `&kp ASTRK` | implemented; CI pass | overlap USB/BLE |
| `modules/combos.c` | 0 | G+Z | momentary Smiles | Graphite/QWERTY | 5 17 | `&mo SMILES`, slow-release | implemented; CI pass | release manual |
| `modules/combos.c` | 0 | H+I | one-shot Vim | Graphite/QWERTY | 19 22 | `&sl VIM` | implemented; CI pass | manual |
| `modules/combos.c` | 0 | Q+N | Left Alt | Graphite/QWERTY | 13 25 | `&kp LALT`, slow-release | implemented; CI pass | release manual |
| `modules/combos.c` | 0 | X+R | Left GUI | Graphite/QWERTY | 14 26 | `&kp LGUI`, slow-release | implemented; CI pass | release manual |
| `modules/combos.c` | 0 | M+T | Left Shift | Graphite/QWERTY | 15 27 | `&kp LSHIFT`, slow-release | implemented; CI pass | release manual |
| `modules/combos.c` | 0 | C+S | Left Ctrl | Graphite/QWERTY | 16 28 | `&kp LCTRL`, slow-release | implemented; CI pass | release manual |
| `modules/combos.c` | 0 | P+H | Right Ctrl | Graphite/QWERTY | 19 31 | `&kp RCTRL`, slow-release | implemented; CI pass | release manual |
| `modules/combos.c` | 0 | F22+A | Right Shift | Graphite/QWERTY | 20 32 | `&kp RSHIFT`, slow-release | implemented; CI pass | none-position + release |
| `modules/combos.c` | 0 | F23+E | Right GUI | Graphite/QWERTY | 21 33 | `&kp RGUI`, slow-release | implemented; CI pass | none-position + release |
| `modules/combos.c` | 0 | F24+I | Right Alt | Graphite/QWERTY | 22 34 | `&kp RALT`, slow-release | implemented; CI pass | none-position + release |
| `modules/combos.c` | 0 | R+L | Copy | Graphite/QWERTY | 2 14 | `&os_action OS_ACTION_COPY` | implemented; CI pass | 3 OS + Copy/Cut overlap |
| `modules/combos.c` | 0 | T+D | Paste | Graphite/QWERTY | 3 15 | `&os_action OS_ACTION_PASTE` | implemented; CI pass | 3 OS |
| `modules/combos.c` | 0 | R+L+T+D | Cut | Graphite/QWERTY | 2 3 14 15 | `&os_action OS_ACTION_CUT` | implemented; CI pass | 3 OS + Copy/Cut overlap |
| `modules/combos.c` | 0 | A+O | Undo | Graphite/QWERTY | 8 20 | `&os_action OS_ACTION_UNDO` | implemented; CI pass | 3 OS |
| `modules/combos.c` | 0 | E+U | Redo | Graphite/QWERTY | 9 21 | `&os_action OS_ACTION_REDO` | implemented; CI pass | 3 OS |
| `modules/combos.c` | 0 | Y+H+A | Voice | Graphite/QWERTY | 18 19 20 | `&os_action OS_ACTION_VOICE` | implemented; CI pass | 3 OS; Mac consumer |
| `modules/combos.c` | 0 | N+R+T | Word Backspace | Graphite/QWERTY | 13 14 15 | `&os_action OS_ACTION_WORD_BACKSPACE` | implemented; CI pass | 3 OS + overlap |
| `modules/combos.c` | 0 | A+E+I | Word Delete | Graphite/QWERTY | 20 21 22 | `&os_action OS_ACTION_WORD_DELETE` | implemented; CI pass | 3 OS + overlap |
| `modules/combos.c` | 0 | Q+X+M | Word Previous | Graphite/QWERTY | 25 26 27 | `&os_action OS_ACTION_WORD_PREVIOUS` | implemented; CI pass | 3 OS |
| `modules/combos.c` | 0 | F22+F23+F24 | Word Next | Graphite/QWERTY | 32 33 34 | `&os_action OS_ACTION_WORD_NEXT` | implemented; CI pass | none-positions + 3 OS |
| `modules/combos.c` | 0 | H+A+E+I | App Switch | Graphite/QWERTY | 19 20 21 22 | `&os_action OS_ACTION_APP_SWITCH` | implemented; CI pass | 3 OS + Enter overlap |
| `modules/combos.c` | 0 | N+R+T+S | Task View | Graphite/QWERTY | 13 14 15 16 | `&os_action OS_ACTION_TASK_VIEW` | implemented; CI pass | 3 OS + Tab/word overlap |
| `modules/combos.c` | 0 | N+B | Select All | Graphite/QWERTY | 1 13 | `&os_action OS_ACTION_SELECT_ALL` | implemented; CI pass | 3 OS |

## OS-aware actions

| Source QMK file | Source QMK layer | Source physical position | Source behavior | Target ZMK layer | Target key-position | Target behavior | Status | Validation |
|---|---|---|---|---|---|---|---|---|
| `modules/switch_cases.c` | global | Copy combo | OS-aware copy | Graphite/QWERTY | 2 14 | `OS_ACTION_COPY` | implemented; CI pass | Win/Mac/Linux |
| `modules/switch_cases.c` | global | Paste combo | OS-aware paste | Graphite/QWERTY | 3 15 | `OS_ACTION_PASTE` | implemented; CI pass | Win/Mac/Linux |
| `modules/switch_cases.c` | global | Cut combo | OS-aware cut | Graphite/QWERTY | 2 3 14 15 | `OS_ACTION_CUT` | implemented; CI pass | Win/Mac/Linux |
| `modules/switch_cases.c` | global | Undo combo | OS-aware undo | Graphite/QWERTY | 8 20 | `OS_ACTION_UNDO` | implemented; CI pass | Win/Mac/Linux |
| `modules/switch_cases.c` | global | Redo combo | OS-aware redo | Graphite/QWERTY | 9 21 | `OS_ACTION_REDO` | implemented; CI pass | Win/Mac/Linux |
| `modules/switch_cases.c` | global | Select all combo | OS-aware select | Graphite/QWERTY | 1 13 | `OS_ACTION_SELECT_ALL` | implemented; CI pass | Win/Mac/Linux |
| `modules/switch_cases.c` | global | word-left combo | Ctrl/Alt Left | Graphite/QWERTY | 25 26 27 | `OS_ACTION_WORD_PREVIOUS` | implemented; CI pass | Win/Mac/Linux |
| `modules/switch_cases.c` | global | word-right combo | Ctrl/Alt Right | Graphite/QWERTY | 32 33 34 | `OS_ACTION_WORD_NEXT` | implemented; CI pass | Win/Mac/Linux |
| `modules/switch_cases.c` | global | word-BS combo | Ctrl/Alt Backspace | Graphite/QWERTY | 13 14 15 | `OS_ACTION_WORD_BACKSPACE` | implemented; CI pass | Win/Mac/Linux |
| `modules/switch_cases.c` | global | word-DEL combo | Ctrl/Alt Delete | Graphite/QWERTY | 20 21 22 | `OS_ACTION_WORD_DELETE` | implemented; CI pass | Win/Mac/Linux |
| `modules/switch_cases.c` | global | Alt-Tab combo | app switch | Graphite/QWERTY | 19 20 21 22 | `OS_ACTION_APP_SWITCH` | implemented; CI pass | Win/Mac/Linux |
| `modules/switch_cases.c` | global | Alt-Win combo | task view | Graphite/QWERTY | 13 14 15 16 | `OS_ACTION_TASK_VIEW` | implemented; CI pass | Win/Mac/Linux |
| `modules/switch_cases.c` | global | Voice combo | OS voice | Graphite/QWERTY | 18 19 20 | `OS_ACTION_VOICE` | implemented; CI pass | Win/Mac/Linux |

## Явно исключено

| Source | Функция/слой | Status | Причина/validation |
|---|---|---|---|
| QMK layer 1/8/10 | Russian base, Symbols, Smiles | excluded | scope запрещает русские слои; search audit |
| QMK language combo | `Ctrl+Shift+1/2` | excluded | русский layout отсутствует |
| QMK layer 2 | old TB/numeric macro layer | excluded | явное исключение; не создавать |
| `ST_MACRO_0–10` | numeric text macros | excluded | старый layer 2 |
| `ST_MACRO_35–36` | Russian smiles | excluded | русский scope |
| QMK layer 11 | Mouse/Navigator | excluded | mouse/trackpad/automouse запрещены |
| QMK top row | `/findbynick`, `/birzha`, Auto Shift toggles, Ctrl+Alt+1–4, RGB, bootloader | excluded | верхний ряд Voyager полностью пропущен |
| Navigation OS mapping | OS-aware tabs/workspaces/voice | deferred | `migrated, OS adaptation deferred` |
| OS persistence/detection | flash/USB detection | excluded | profile всегда Windows после reboot |
