# Матрица миграции QMK/Oryx → ZMK

Reference source: `tpaktop77/oryx-with-custom-qmk`, remote `main@5d674ed4d866b66e34389262092cc5347da90bc7`. Target base: `master@23975abebb15386678195a33b08ee3bf04ec6f75`. Повторная проверка перед русской миграцией: default branches `main`/`master`, открытых PR в обоих репозиториях нет, рабочая ветка `20260901__migrate_russian_layers` не является `master`.

Основная миграция и follow-up с Escape/сдвигом OS selectors прошли статический аудит и сборку обеих половин; аппаратные проверки остаются владельцу согласно `docs/test-matrix.md`.

## Слои и общие behaviors

| Source QMK file | Source QMK layer | Source physical position | Source behavior | Target ZMK layer | Target key-position | Target behavior | Status | Validation |
|---|---:|---|---|---|---|---|---|---|
| `6JP4n/keymap.c` + follow-up | 0 | три нижних ряда Voyager | Graphite body + Escape | `GRAPHITE` 0 | 0–35 | Graphite с `&kp ESC` на position 0, верхний ряд отброшен | implemented; CI pass | layout audit + hardware |
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
| prior ZMK Lower + new | — | left/right body | BT + OS controls | `SYSTEM_BT` 8 | 0–35 | BT0–BT4, protected clear, OS selectors 19–21 | implemented; CI pass | build + manual |
| target Studio | — | reserved nodes | editable capacity | 12–14 | n/a | три `status = "reserved"` после русских слоёв | planned | Studio connect + build |
| `6JP4n/config.h` | global | English letters | QMK Auto Shift 200 ms | `GRAPHITE`, `QWERTY` | all letter positions | ZMK hold-tap Auto Shift 250 ms | implemented; CI pass | manual USB/BLE |

## Русский базовый слой

Все строки ниже происходят из `6JP4n/keymap.c`, QMK layer 1. QMK aliases проверены по `6JP4n/i18n.h`; target использует те же физические keyboard HID usages при Russian–PC на хосте. ZMK `LBKT/RBKT` соответствуют QMK `KC_LBRC/KC_RBRC`; ZMK `LBRC/RBRC` означают фигурные скобки и здесь не используются.

| Source QMK file | Source QMK layer | Source physical position | Source behavior | Target ZMK layer | Target key-position | Target behavior | Status | Validation |
|---|---:|---|---|---|---:|---|---|---|
| `keymap.c` + `i18n.h` | 1 | row 1 col 1 | `RU_SHTI = KC_Q` → Й | `RUSSIAN` 9 | 1 | `AS(Q)` → й/Й | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 1 col 2 | `RU_TSE = KC_W` → Ц | `RUSSIAN` 9 | 2 | `AS(W)` → ц/Ц | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 1 col 3 | `RU_U = KC_E` → У | `RUSSIAN` 9 | 3 | `AS(E)` → у/У | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 1 col 4 | `RU_KA = KC_R` → К | `RUSSIAN` 9 | 4 | `AS(R)` → к/К | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 1 col 5 | `RU_IE = KC_T` → Е | `RUSSIAN` 9 | 5 | `AS(T)` → е/Е | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 1 col 6 | `RU_EN = KC_Y` → Н | `RUSSIAN` 9 | 6 | `AS(Y)` → н/Н | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 1 col 7 | `RU_GHE = KC_U` → Г | `RUSSIAN` 9 | 7 | `AS(U)` → г/Г | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 1 col 8 | `RU_SHA = KC_I` → Ш | `RUSSIAN` 9 | 8 | `AS(I)` → ш/Ш | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 1 col 9 | `RU_SHCH = KC_O` → Щ | `RUSSIAN` 9 | 9 | `AS(O)` → щ/Щ | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 1 col 10 | `RU_ZE = KC_P` → З | `RUSSIAN` 9 | 10 | `AS(P)` → з/З | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 1 col 11 | `RU_HA = KC_LBRC` → Х | `RUSSIAN` 9 | 11 | `AS(LBKT)` → х/Х | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 2 col 1 | `RU_EF = KC_A` → Ф | `RUSSIAN` 9 | 13 | `AS(A)` → ф/Ф | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 2 col 2 | `RU_YERU = KC_S` → Ы | `RUSSIAN` 9 | 14 | `AS(S)` → ы/Ы | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 2 col 3 | `RU_VE = KC_D` → В | `RUSSIAN` 9 | 15 | `AS(D)` → в/В | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 2 col 4 | `RU_A = KC_F` → А | `RUSSIAN` 9 | 16 | `AS(F)` → а/А | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 2 col 5 | `RU_PE = KC_G` → П | `RUSSIAN` 9 | 17 | `AS(G)` → п/П | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 2 col 6 | `RU_ER = KC_H` → Р | `RUSSIAN` 9 | 18 | `AS(H)` → р/Р | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 2 col 7 | `RU_O = KC_J` → О | `RUSSIAN` 9 | 19 | `AS(J)` → о/О | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 2 col 8 | `RU_EL = KC_K` → Л | `RUSSIAN` 9 | 20 | `AS(K)` → л/Л | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 2 col 9 | `RU_DE = KC_L` → Д | `RUSSIAN` 9 | 21 | `AS(L)` → д/Д | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 2 col 10 | `RU_ZHE = KC_SCLN` → Ж | `RUSSIAN` 9 | 22 | `AS(SEMI)` → ж/Ж | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 2 col 11 | `RU_E = KC_QUOT` → Э | `RUSSIAN` 9 | 23 | `AS(SQT)` → э/Э | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 3 col 1 | `RU_YA = KC_Z` → Я | `RUSSIAN` 9 | 25 | `AS(Z)` → я/Я | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 3 col 2 | `RU_CHE = KC_X` → Ч | `RUSSIAN` 9 | 26 | `AS(X)` → ч/Ч | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 3 col 3 | `RU_ES = KC_C` → С | `RUSSIAN` 9 | 27 | `AS(C)` → с/С | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 3 col 4 | `RU_EM = KC_V` → М | `RUSSIAN` 9 | 28 | `AS(V)` → м/М | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 3 col 5 | `RU_I = KC_B` → И | `RUSSIAN` 9 | 29 | `AS(B)` → и/И | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 3 col 6 | `RU_TE = KC_N` → Т | `RUSSIAN` 9 | 30 | `AS(N)` → т/Т | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 3 col 7 | `RU_SOFT = KC_M` → Ь | `RUSSIAN` 9 | 31 | `AS(M)` → ь/Ь | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 3 col 8 | `RU_BE = KC_COMM` → Б | `RUSSIAN` 9 | 32 | `AS(COMMA)` → б/Б | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 3 col 9 | `RU_YU = KC_DOT` → Ю | `RUSSIAN` 9 | 33 | `AS(DOT)` → ю/Ю | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 3 col 10 | `RU_YO = KC_GRV` → Ё | `RUSSIAN` 9 | 34 | `AS(GRAVE)` → ё/Ё | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 1 | row 3 col 11 | `RU_HARD = KC_RBRC` → Ъ | `RUSSIAN` 9 | 35 | `AS(RBKT)` → ъ/Ъ | planned | Russian–PC USB/BLE |

## Русские служебные позиции и thumbs

| Source QMK file | Source QMK layer | Source physical position | Source behavior | Target ZMK layer | Target key-position | Target behavior | Status | Validation |
|---|---:|---|---|---|---:|---|---|---|
| `keymap.c` | 1 | row 1 outer-left | transparent | `RUSSIAN` | 0 | `&trans` → Graphite Escape | planned | fallback manual |
| `keymap.c` | 1 | row 2 outer-left | transparent | `RUSSIAN` | 12 | `&trans` → Graphite none | planned | fallback manual |
| `keymap.c` + `switch_cases.c` concept | 0/1 | row 3 outer-left | `TT(1)` plus language combo | `GRAPHITE`/`RUSSIAN` | 24 | directed host shortcut + `&to` opposite language layer | planned | USB/BLE host sync |
| target + `keymap.c` | 1 | added outer thumb | System access | `RUSSIAN` | 36 | `&mo SYSTEM_BT` | planned | thumb manual |
| `keymap.c` | 1 | left inner thumb | `LT(8, KC_BSPC)` | `RUSSIAN` | 37 | `&lt RUSSIAN_SYMBOLS BSPC` | planned | tap/hold manual |
| target | shared thumb | Delete/Numbers | `RUSSIAN` | 38 | `&lt NUMBERS DEL` | planned | tap/hold manual |
| target | shared thumb | Tab/Function | `RUSSIAN` | 39 | `&lt FUNCTION TAB` | planned | tap/hold manual |
| target | shared thumb | Space/Navigation | `RUSSIAN` | 40 | `&lt NAVIGATION SPACE` | planned | tap/hold manual |
| target | added outer thumb | unused | `RUSSIAN` | 41 | `&none` | planned | manual |

## Русские Symbols и Smiles

Пустые позиции QMK `KC_NO` остаются `&none`; таблица перечисляет переносимые непустые bindings. Символы отображаются хостом только при Russian–PC.

| Source QMK file | Source QMK layer | Source physical position | Source behavior | Target ZMK layer | Target key-position | Target behavior | Status | Validation |
|---|---:|---|---|---|---:|---|---|---|
| `keymap.c` + `i18n.h` | 8 | row 1 col 3–4 | `RU_LPRN`, `RU_RPRN` | `RUSSIAN_SYMBOLS` 10 | 3, 4 | `LS(N9)`, `LS(N0)` → `(`, `)` | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 8 | row 1 right cols 0–5 | `RU_NUM/UNDS/MINS/SLSH/BSLS/MINS` | `RUSSIAN_SYMBOLS` 10 | 6–11 | `LS(N3)`, `LS(MINUS)`, `MINUS`, `LS(BSLH)`, `BSLH`, `MINUS` → `№ _ - / \\ -` | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 8 | row 2 right cols 1–5 | `RU_DOT/COMM/EXLM/QUES/SLSH` | `RUSSIAN_SYMBOLS` 10 | 19–23 | `FSLH`, `LS(FSLH)`, `LS(N1)`, `LS(N7)`, `LS(BSLH)` → `. , ! ? /` | planned | Russian–PC USB/BLE |
| `keymap.c` + `i18n.h` | 8 | row 3 right cols 1–5 | `RU_DQUO/COLN/SCLN/EQL/ASTR` | `RUSSIAN_SYMBOLS` 10 | 31–35 | `LS(N2)`, `LS(N6)`, `LS(N4)`, `EQUAL`, `LS(N8)` → `" : ; = *` | planned | Russian–PC USB/BLE |
| `keymap.c` | 8 | four Voyager thumbs | transparent + `LT(10, KC_SPACE)` | `RUSSIAN_SYMBOLS` 10 | 36–41 | transparent; position 40 = `&lt RUSSIAN_SMILES SPACE` | planned | fallback + tap/hold |
| `keymap.c` | 10 | right home col 1 | `ST_MACRO_35` | `RUSSIAN_SMILES` 11 | 19 | named macro `LS(N6), LS(N0)` → `:)` | planned | Russian–PC USB/BLE |
| `keymap.c` | 10 | right home col 2 | `ST_MACRO_36` | `RUSSIAN_SMILES` 11 | 20 | named macro `LS(N6), LS(N9)` → `:(` | planned | Russian–PC USB/BLE |

## Переключение языка и временный Caps Word

| Source QMK file | Source QMK layer | Source physical position | Source behavior | Target ZMK layer | Target key-position | Target behavior | Status | Validation |
|---|---:|---|---|---|---:|---|---|---|
| `modules/combos.c`, `modules/switch_cases.c` | 0 | F+O+U | toggle layer + `Ctrl+Shift+2` | `GRAPHITE` | 7 8 9 | directed `to_russian`: `Ctrl+Shift+2`, `&to RUSSIAN` | planned | USB/BLE host sync |
| `modules/combos.c`, `modules/switch_cases.c` | 1 | Г+Ш+Щ | toggle layer + `Ctrl+Shift+1` | `RUSSIAN` | 7 8 9 | directed `to_english`: `Ctrl+Shift+1`, `&to GRAPHITE` | planned | USB/BLE host sync |
| `keymap.c` | 0/1 | lower outer-left | `TT(1)` only in source | `GRAPHITE`/`RUSSIAN` | 24 | reuse corresponding directed action | planned | USB/BLE host sync |
| ZMK built-in | global | Caps Word state | Shift only HID A–Z | `GRAPHITE`/`QWERTY`/`RUSSIAN` | combo 13 16 | `continue-list` also keeps `LBKT RBKT SEMI SQT COMMA DOT GRAVE`; seven letters remain lowercase | planned workaround | explicit manual limitation |
| future | — | — | full Russian capitalization | future `implement-russian-caps-word` | — | custom behavior after hardware validation | deferred next OpenSpec | separate proposal required |

## System/Bluetooth

| Source QMK file | Source QMK layer | Source physical position | Source behavior | Target ZMK layer | Target key-position | Target behavior | Status | Validation |
|---|---:|---|---|---|---:|---|---|---|
| prior target Lower | 1 | left top row | `BT_SEL 0` | `SYSTEM_BT` | 0 | `&bt BT_SEL 0` | implemented; CI pass | manual USB/BLE |
| prior target Lower | 1 | left top row | `BT_SEL 1` | `SYSTEM_BT` | 1 | `&bt BT_SEL 1` | implemented; CI pass | manual USB/BLE |
| prior target Lower | 1 | left top row | `BT_SEL 2` | `SYSTEM_BT` | 2 | `&bt BT_SEL 2` | implemented; CI pass | manual USB/BLE |
| prior target Lower | 1 | left top row | `BT_SEL 3` | `SYSTEM_BT` | 3 | `&bt BT_SEL 3` | implemented; CI pass | manual USB/BLE |
| prior target Lower | 1 | left top row | `BT_SEL 4` | `SYSTEM_BT` | 4 | `&bt BT_SEL 4` | implemented; CI pass | manual USB/BLE |
| prior target Lower | 1 | lower external left | unprotected `BT_CLR` | `SYSTEM_BT` | 24 | tap-preferred 1500 ms hold → BT Clear | implemented; CI pass | short/long manual |
| `modules/switch_cases.c` concept | global | right home index | detected OS | `SYSTEM_BT` | 19 | `&os_set OS_WINDOWS` | implemented; CI pass | state/action test |
| `modules/switch_cases.c` concept | global | right home | detected OS | `SYSTEM_BT` | 20 | `&os_set OS_MACOS` | implemented; CI pass | state/action test |
| `modules/switch_cases.c` concept | global | right home | detected OS | `SYSTEM_BT` | 21 | `&os_set OS_LINUX` | implemented; CI pass | state/action test |

## Positional combo

Все Source chords перечислены буквами Oryx Graphite, но target определяется только `key-positions`. Совместимый базовый scope после русской миграции: `<GRAPHITE QWERTY RUSSIAN>`. English/Russian Smiles и language combo используют отдельные scopes; Vim остаётся `<GRAPHITE QWERTY>`. Arithmetic scope: `<NUMBERS>`.

| Source QMK file | Source QMK layer | Source physical position | Source behavior | Target ZMK layer | Target key-position | Target behavior | Status | Validation |
|---|---:|---|---|---|---|---|---|---|
| `modules/combos.c` | 0/1 | H+A+E / О+Л+Д | Enter | Graphite/QWERTY/Russian | 19 20 21 | `&kp RET` | English implemented; Russian planned | USB/BLE + overlap |
| `modules/combos.c` | 0/1 | R+T+S / Ы+В+А | Tab | Graphite/QWERTY/Russian | 14 15 16 | `&kp TAB` | English implemented; Russian planned | USB/BLE + overlap |
| `modules/combos.c` | 0/1 | N+S / Ф+А | Caps Word | Graphite/QWERTY/Russian | 13 16 | `&caps_word` | English implemented; Russian planned | manual + seven exceptions |
| `modules/combos.c` | 0/1 | N+G / Ф+П | Caps Lock | Graphite/QWERTY/Russian | 13 17 | `&kp CAPSLOCK` | English implemented; Russian planned | all 33 letters manual |
| `modules/combos.c` | 3 | 7+8+9 | Minus | Numbers | 7 8 9 | `&kp MINUS` | implemented; CI pass | USB/BLE |
| `modules/combos.c` | 3 | 4+5+6 | Equal | Numbers | 19 20 21 | `&kp EQUAL` | implemented; CI pass | overlap USB/BLE |
| `modules/combos.c` | 3 | 1+2+3 | Plus | Numbers | 31 32 33 | `&kp PLUS` | implemented; CI pass | overlap USB/BLE |
| `modules/combos.c` | 3 | 4+5+6+Dot | Slash | Numbers | 19 20 21 22 | `&kp FSLH` | implemented; CI pass | overlap USB/BLE |
| `modules/combos.c` | 3 | 1+2+3+Equal | Asterisk | Numbers | 31 32 33 34 | `&kp ASTRK` | implemented; CI pass | overlap USB/BLE |
| `modules/combos.c` | 0 | G+Z | momentary Smiles | Graphite/QWERTY | 5 17 | `&mo SMILES`, slow-release | implemented; CI pass | release manual |
| `modules/combos.c` | 1 | П+Е | momentary Russian Smiles | Russian | 5 17 | `&mo RUSSIAN_SMILES`, slow-release | planned | release manual |
| `modules/combos.c` | 0 | H+I | one-shot Vim | Graphite/QWERTY | 19 22 | `&sl VIM` | implemented; CI pass | manual |
| `modules/combos.c` | 0/1 | Q+N / Я+Ф | Left Alt | Graphite/QWERTY/Russian | 13 25 | `&kp LALT`, slow-release | English implemented; Russian planned | release manual |
| `modules/combos.c` | 0/1 | X+R / Ч+Ы | Left GUI | Graphite/QWERTY/Russian | 14 26 | `&kp LGUI`, slow-release | English implemented; Russian planned | release manual |
| `modules/combos.c` | 0/1 | M+T / С+В | Left Shift | Graphite/QWERTY/Russian | 15 27 | `&kp LSHIFT`, slow-release | English implemented; Russian planned | release manual |
| `modules/combos.c` | 0/1 | C+S / М+А | Left Ctrl | Graphite/QWERTY/Russian | 16 28 | `&kp LCTRL`, slow-release | English implemented; Russian planned | release manual |
| `modules/combos.c` | 0/1 | P+H / Ь+О | Right Ctrl | Graphite/QWERTY/Russian | 19 31 | `&kp RCTRL`, slow-release | English implemented; Russian planned | release manual |
| `modules/combos.c` | 0/1 | F22+A / Б+Л | Right Shift | Graphite/QWERTY/Russian | 20 32 | `&kp RSHIFT`, slow-release | English implemented; Russian planned | release manual |
| `modules/combos.c` | 0/1 | F23+E / Ю+Д | Right GUI | Graphite/QWERTY/Russian | 21 33 | `&kp RGUI`, slow-release | English implemented; Russian planned | release manual |
| `modules/combos.c` | 0/1 | F24+I / Ё+Ж | Right Alt | Graphite/QWERTY/Russian | 22 34 | `&kp RALT`, slow-release | English implemented; Russian planned | release manual |
| `modules/combos.c` | 0/1 | R+L / Ы+Ц | Copy | Graphite/QWERTY/Russian | 2 14 | `&os_action OS_ACTION_COPY` | English implemented; Russian planned | 3 OS + Copy/Cut overlap |
| `modules/combos.c` | 0/1 | T+D / В+У | Paste | Graphite/QWERTY/Russian | 3 15 | `&os_action OS_ACTION_PASTE` | English implemented; Russian planned | 3 OS |
| `modules/combos.c` | 0/1 | R+L+T+D / Ы+Ц+В+У | Cut | Graphite/QWERTY/Russian | 2 3 14 15 | `&os_action OS_ACTION_CUT` | English implemented; Russian planned | 3 OS + Copy/Cut overlap |
| `modules/combos.c` | 0/1 | A+O / Л+Ш | Undo | Graphite/QWERTY/Russian | 8 20 | `&os_action OS_ACTION_UNDO` | English implemented; Russian planned | 3 OS |
| `modules/combos.c` | 0/1 | E+U / Д+Щ | Redo | Graphite/QWERTY/Russian | 9 21 | `&os_action OS_ACTION_REDO` | English implemented; Russian planned | 3 OS |
| `modules/combos.c` | 0/1 | Y+H+A / Р+О+Л | Voice | Graphite/QWERTY/Russian | 18 19 20 | `&os_action OS_ACTION_VOICE` | English implemented; Russian planned | 3 OS; Mac consumer |
| `modules/combos.c` | 0/1 | N+R+T / Ф+Ы+В | Word Backspace | Graphite/QWERTY/Russian | 13 14 15 | `&os_action OS_ACTION_WORD_BACKSPACE` | English implemented; Russian planned | 3 OS + overlap |
| `modules/combos.c` | 0/1 | A+E+I / Л+Д+Ж | Word Delete | Graphite/QWERTY/Russian | 20 21 22 | `&os_action OS_ACTION_WORD_DELETE` | English implemented; Russian planned | 3 OS + overlap |
| `modules/combos.c` | 0/1 | Q+X+M / Я+Ч+С | Word Previous | Graphite/QWERTY/Russian | 25 26 27 | `&os_action OS_ACTION_WORD_PREVIOUS` | English implemented; Russian planned | 3 OS |
| `modules/combos.c` | 0/1 | F22+F23+F24 / Б+Ю+Ё | Word Next | Graphite/QWERTY/Russian | 32 33 34 | `&os_action OS_ACTION_WORD_NEXT` | English implemented; Russian planned | positions + 3 OS |
| `modules/combos.c` | 0/1 | H+A+E+I / О+Л+Д+Ж | App Switch | Graphite/QWERTY/Russian | 19 20 21 22 | `&os_action OS_ACTION_APP_SWITCH` | English implemented; Russian planned | 3 OS + Enter overlap |
| `modules/combos.c` | 0/1 | N+R+T+S / Ф+Ы+В+А | Task View | Graphite/QWERTY/Russian | 13 14 15 16 | `&os_action OS_ACTION_TASK_VIEW` | English implemented; Russian planned | 3 OS + Tab/word overlap |
| `modules/combos.c` | 0/1 | N+B / Ф+Й | Select All | Graphite/QWERTY/Russian | 1 13 | `&os_action OS_ACTION_SELECT_ALL` | English implemented; Russian planned | 3 OS |

## OS-aware actions

| Source QMK file | Source QMK layer | Source physical position | Source behavior | Target ZMK layer | Target key-position | Target behavior | Status | Validation |
|---|---|---|---|---|---|---|---|---|
| `modules/switch_cases.c` | global | Copy combo | OS-aware copy | Graphite/QWERTY/Russian | 2 14 | `OS_ACTION_COPY` | Russian scope planned | Win/Mac/Linux |
| `modules/switch_cases.c` | global | Paste combo | OS-aware paste | Graphite/QWERTY/Russian | 3 15 | `OS_ACTION_PASTE` | Russian scope planned | Win/Mac/Linux |
| `modules/switch_cases.c` | global | Cut combo | OS-aware cut | Graphite/QWERTY/Russian | 2 3 14 15 | `OS_ACTION_CUT` | Russian scope planned | Win/Mac/Linux |
| `modules/switch_cases.c` | global | Undo combo | OS-aware undo | Graphite/QWERTY/Russian | 8 20 | `OS_ACTION_UNDO` | Russian scope planned | Win/Mac/Linux |
| `modules/switch_cases.c` | global | Redo combo | OS-aware redo | Graphite/QWERTY/Russian | 9 21 | `OS_ACTION_REDO` | Russian scope planned | Win/Mac/Linux |
| `modules/switch_cases.c` | global | Select all combo | OS-aware select | Graphite/QWERTY/Russian | 1 13 | `OS_ACTION_SELECT_ALL` | Russian scope planned | Win/Mac/Linux |
| `modules/switch_cases.c` | global | word-left combo | Ctrl/Alt Left | Graphite/QWERTY/Russian | 25 26 27 | `OS_ACTION_WORD_PREVIOUS` | Russian scope planned | Win/Mac/Linux |
| `modules/switch_cases.c` | global | word-right combo | Ctrl/Alt Right | Graphite/QWERTY/Russian | 32 33 34 | `OS_ACTION_WORD_NEXT` | Russian scope planned | Win/Mac/Linux |
| `modules/switch_cases.c` | global | word-BS combo | Ctrl/Alt Backspace | Graphite/QWERTY/Russian | 13 14 15 | `OS_ACTION_WORD_BACKSPACE` | Russian scope planned | Win/Mac/Linux |
| `modules/switch_cases.c` | global | word-DEL combo | Ctrl/Alt Delete | Graphite/QWERTY/Russian | 20 21 22 | `OS_ACTION_WORD_DELETE` | Russian scope planned | Win/Mac/Linux |
| `modules/switch_cases.c` | global | Alt-Tab combo | app switch | Graphite/QWERTY/Russian | 19 20 21 22 | `OS_ACTION_APP_SWITCH` | Russian scope planned | Win/Mac/Linux |
| `modules/switch_cases.c` | global | Alt-Win combo | task view | Graphite/QWERTY/Russian | 13 14 15 16 | `OS_ACTION_TASK_VIEW` | Russian scope planned | Win/Mac/Linux |
| `modules/switch_cases.c` | global | Voice combo | OS voice | Graphite/QWERTY/Russian | 18 19 20 | `OS_ACTION_VOICE` | Russian scope planned | Win/Mac/Linux |

## Явно исключено

| Source | Функция/слой | Status | Причина/validation |
|---|---|---|---|
| QMK Voyager top row layer 1 | transparent Russian row | excluded | верхний ряд полностью пропускается |
| QMK Voyager top row layer 8 | `%`, `₽`, `+` и дублирующий `*` | deferred | не перераспределять в текущем change; нижний `*` переносится |
| Alternative input | Unicode Russian | excluded | используется проверенная host Russian–PC модель |
| QWERTY language cycling | возврат в предыдущий English layout | excluded | русский выход всегда направлен в Graphite |
| ZMK custom behavior | полный Russian Caps Word | deferred | следующий OpenSpec `implement-russian-caps-word`; сейчас только `continue-list` |
| Combo tuning | изменение overlap timeout/policy | deferred | отдельная ветка/change после новых switches |
| QMK layer 2 | old TB/numeric macro layer | excluded | явное исключение; не создавать |
| `ST_MACRO_0–10` | numeric text macros | excluded | старый layer 2 |
| `ST_MACRO_35–36` | Russian smiles | excluded | русский scope |
| QMK layer 11 | Mouse/Navigator | excluded | mouse/trackpad/automouse запрещены |
| QMK top row | `/findbynick`, `/birzha`, Auto Shift toggles, Ctrl+Alt+1–4, RGB, bootloader | excluded | верхний ряд Voyager полностью пропущен |
| Navigation OS mapping | OS-aware tabs/workspaces/voice | deferred | `migrated, OS adaptation deferred` |
| OS persistence/detection | flash/USB detection | excluded | profile всегда Windows после reboot |
