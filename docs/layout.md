# Раскладка Corne

Этот документ описывает целевую раскладку changes `complete-graphite-zmk-migration` и `migrate-russian-layers`. `·` означает `&none`, `▽` — `&trans`. `AS(X)` означает Auto Shift: tap выводит `X`, удержание более 250 мс — `Shift+X`.

## Нумерация позиций

ZMK combo используют физические `key-positions`, последовательно слева направо по каждому ряду, затем thumbs:

```text
 0  1  2  3  4  5   |   6  7  8  9 10 11
12 13 14 15 16 17   |  18 19 20 21 22 23
24 25 26 27 28 29   |  30 31 32 33 34 35
          36 37 38   |  39 40 41
```

Позиции `32`, `33`, `34`, соответствовавшие `F22/F23/F24` в Oryx Graphite, на базовом слое имеют `&none`. Combo продолжают использовать эти номера напрямую.

## Структура слоёв

| Индекс | Константа | Display name | Назначение |
|---:|---|---|---|
| 0 | `GRAPHITE` | Graphite | Единственный загрузочный base |
| 1 | `QWERTY` | QWERTY | Минимальная альтернативная раскладка |
| 2 | `NUMBERS` | Numbers | Цифры, скобки и арифметика |
| 3 | `NAVIGATION` | Navigation | Навигация и media; OS adaptation deferred |
| 4 | `FUNCTION` | Function | F1–F20 и modifiers |
| 5 | `VIM` | Vim | Vim macros |
| 6 | `SYMBOLS` | Symbols | Английские символы и path macros |
| 7 | `SMILES` | Smiles | Английские смайлики |
| 8 | `SYSTEM_BT` | System | Bluetooth и OS profile |
| 9 | `RUSSIAN` | Russian | Русская раскладка Russian–PC |
| 10 | `RUSSIAN_SYMBOLS` | Russian Symbols | Русские символы |
| 11 | `RUSSIAN_SMILES` | Russian Smiles | Русские смайлы |
| 12 | `STUDIO_EXTRA_1` | reserved | Резерв ZMK Studio |
| 13 | `STUDIO_EXTRA_2` | reserved | Резерв ZMK Studio |
| 14 | `STUDIO_EXTRA_3` | reserved | Резерв ZMK Studio |

## Общие thumb-клавиши Graphite и QWERTY

```text
Left outer       Left inner          Left center
SYSTEM (hold)    BSPC / SYMBOLS      DEL / NUMBERS

Right center     Right inner         Right outer
TAB / FUNCTION   SPACE / NAVIGATION  ·
```

В порядке keymap positions `36..41`:

```text
&mo SYSTEM_BT | &lt SYMBOLS BSPC | &lt NUMBERS DEL
&lt FUNCTION TAB | &lt NAVIGATION SPACE | &none
```

`&lt.quick-tap-ms = 250` сохраняет быстрое повторение tap-кода layer-tap и не относится к Auto Shift.

## 0 — Graphite

Верхний ряд Voyager игнорируется. Правая нижняя body-клавиша выбирает QWERTY.

```text
ESC     AS(B) AS(L) AS(D) AS(W) AS(Z) | NUMLOCK AS(F) AS(O) AS(U) AS(J) ·
·       AS(N) AS(R) AS(T) AS(S) AS(G) | AS(Y)   AS(H) AS(A) AS(E) AS(I) ·
→RU     AS(Q) AS(X) AS(M) AS(C) AS(V) | AS(K)   AS(P) ·     ·     ·     →QWERTY
              SYSTEM  BSP/SYMB  DEL/NUM | TAB/FN  SPC/NAV  ·
```

После reboot активен именно этот слой.

`→RU` на position 24 отправляет `Ctrl+Shift+2` и затем прямо активирует `RUSSIAN`. То же действие доступно combo positions `7+8+9` (Graphite `F+O+U`).

## 1 — QWERTY

Body содержит только английские буквы, пустые позиции и кнопку возврата.

```text
·       AS(Q) AS(W) AS(E) AS(R) AS(T) | AS(Y) AS(U) AS(I) AS(O) AS(P) ·
·       AS(A) AS(S) AS(D) AS(F) AS(G) | AS(H) AS(J) AS(K) AS(L) ·     ·
·       AS(Z) AS(X) AS(C) AS(V) AS(B) | AS(N) AS(M) ·     ·     ·     →GRAPHITE
              SYSTEM  BSP/SYMB  DEL/NUM | TAB/FN  SPC/NAV  ·
```

## 2 — Numbers

Сохранённый Oryx Numbers. `ESC/VIM` использует именованную ссылку `VIM`.

```text
ENTER <    >    (    )    · | =    7    8    9    ·    -
TAB   LALT LGUI LSFT LCTL · | ·    4    5    6    .    /
·     {    }    [    ]    · | .    1    2    3    =    *
              ▽    ▽    ▽   | ESC/VIM 0 ▽
```

Arithmetic combo активны только здесь.

## 3 — Navigation

Слой перенесён без OS-aware адаптации: `migrated, OS adaptation deferred`.

```text
ENTER PREV NEXT STOP PLAY VOICE | PGUP HOME UP   END  · CTRL-W
TAB   LALT LGUI LSFT LCTL MUTE  | PGDN LEFT DOWN RIGHT · CTRL-T
·     VOLD VOLU ·    ·    ·     | ·    P-TAB N-TAB P-DESK N-DESK INS
                  ▽  ▽  ▽        | ▽  ▽  ▽
```

## 4 — Function

```text
ENTER F1   F2   F3   F4   F5 | F6   F7   F8   F9   F10  ·
TAB   LALT LGUI LSFT LCTL ·  | ·    RCTL RSFT RGUI RALT ·
·     F11  F12  F13  F14  F15| F16  F17  F18  F19  F20  ·
                 ▽  ▽ DEL/VIM | ▽  ▽  ▽
```

## 5 — Vim

`M11–M21` — существующие version-controlled macros с комментариями в keymap.

```text
:  ·  M11 M12 M13 M14 | M18 M19 K M20 · ·
·  ·  ·   M15 M16 M17 | M21 H   J L   · ·
·  ·  ·   ·   ·   ·   | ·   ·   · ·   · ·
              ▽  ▽  ▽  | ▽  ▽  ▽
```

## 6 — Symbols

```text
`   <   >   (   )   · | #  _  -  /  \\ -
M22 M23 M24 M25 /   | | @  .  ,  !  ?  /
M26 {   }   [   ]   · | '  "  :  ;  =  *
              ▽  ▽  ▽ | ▽  SPACE/SMILES  ▽
```

## 7 — Smiles

`M27–M34` — только английские ASCII-смайлики.

```text
· · · · · · | M27 M28 M29 M30 · ·
· · · · · · | M31 M32 M33 M34 · ·
· · · · · · | ·   ·   ·   ·   · ·
        ▽ ▽ ▽ | ▽ ▽ ▽
```

## 8 — System

Слой активен только при удержании левого внешнего thumb (`position 36`). Левая рука управляет Bluetooth, правая — независимым OS profile.

```text
BT0  BT1  BT2  BT3  BT4  · | ·       ·       ·      ·      · ·
·    ·    ·    ·    ·    · | ·       WINDOWS MACOS  LINUX  · ·
BTCLR·    ·    ·    ·    · | ·       ·      ·      · · ·
                 ▽  ▽  ▽    | ▽  ▽  ▽
```

- BT0–BT4: `&bt BT_SEL 0..4`.
- BT Clear: нижний внешний левый угол, position 24. Короткий tap ничего не делает; непрерывное удержание 1500 мс выполняет `&bt BT_CLR`.
- OS selectors: positions 19–21: Windows, macOS, Linux; position 18 остаётся пустой, поэтому Windows находится под указательным пальцем на home middle row.
- Выбор ОС не меняет слой и не выбирает Bluetooth profile.

## 9 — Russian

Слой использует стандартные HID usages и требует активной раскладки Russian–PC на хосте. Он не использует Unicode. Position 0 прозрачен и наследует Graphite Escape; position 12 прозрачен и наследует Graphite `&none`.

```text
▽       AS(Й) AS(Ц) AS(У) AS(К) AS(Е) | AS(Н) AS(Г) AS(Ш) AS(Щ) AS(З) AS(Х)
▽       AS(Ф) AS(Ы) AS(В) AS(А) AS(П) | AS(Р) AS(О) AS(Л) AS(Д) AS(Ж) AS(Э)
→ENG    AS(Я) AS(Ч) AS(С) AS(М) AS(И) | AS(Т) AS(Ь) AS(Б) AS(Ю) AS(Ё) AS(Ъ)
              SYSTEM BSP/RU-SYMB DEL/NUM | TAB/FN SPC/NAV ·
```

Thumb positions `36..41`:

```text
&mo SYSTEM_BT | &lt RUSSIAN_SYMBOLS BSPC | &lt NUMBERS DEL
&lt FUNCTION TAB | &lt NAVIGATION SPACE | &none
```

`→ENG` на position 24 отправляет `Ctrl+Shift+1` и затем прямо активирует `GRAPHITE`. То же действие доступно combo positions `7+8+9` (Russian `Г+Ш+Щ`). QWERTY в языковой цикл не входит.

## 10 — Russian Symbols

Перенесены только три нижних ряда Voyager. Обозначения ниже — ожидаемые символы Russian–PC; в keymap сохранены исходные физические HID usages QMK.

```text
· · · ( ) · | № _ - / \\ -
· · · · · · | · . , ! ? /
· · · · · · | · " : ; = *
        ▽ ▽ ▽ | ▽ SPACE/RU-SMILES ▽
```

Удержание position 40 открывает `RUSSIAN_SMILES`, tap печатает Space. Остальные прозрачные thumbs сначала наследуются из `RUSSIAN`, затем при необходимости из `GRAPHITE`. `%`, `₽`, `+` из пропущенного верхнего ряда Voyager пока никуда не переносились.

## 11 — Russian Smiles

```text
· · · · · · | · ·  ·  · · ·
· · · · · · | · :) :( · · ·
· · · · · · | · ·  ·  · · ·
        ▽ ▽ ▽ | ▽ ▽ ▽
```

- Position 19: исходный `ST_MACRO_35`, `Shift+6` затем `Shift+0` → `:)` при Russian–PC.
- Position 20: исходный `ST_MACRO_36`, `Shift+6` затем `Shift+9` → `:(` при Russian–PC.
- Combo positions `5+17` (`П+Е`) удерживает этот слой с `slow-release`; английский Smiles combo на тех же позициях имеет отдельный scope.

## Настройка и синхронизация языка хоста

На каждом хосте требуется назначить:

- `Ctrl+Shift+1` — выбрать английскую раскладку;
- `Ctrl+Shift+2` — выбрать Russian–PC.

Windows, macOS и Linux настраиваются средствами самой ОС; на macOS допустимо использовать Hammerspoon, как в исходной конфигурации. Прошивка не определяет язык автоматически и не хранит его во flash. Если язык был изменён мышью, другой клавиатурой или после reboot хоста, повторное направленное действие `→RU`/`→ENG` снова задаёт ожидаемую пару host language + ZMK layer.

## Временное поведение русского Caps Word

В `continue-list` встроенного `&caps_word` добавлены HID usages букв `Х`, `Ъ`, `Ж`, `Э`, `Б`, `Ю`, `Ё`. Поэтому эти буквы не завершают режим. Однако ZMK v0.3 автоматически добавляет Shift только к usages `A`–`Z`, поэтому обычный tap этих семи позиций остаётся строчным. Остальные русские буквы капитализируются; для всех 33 букв можно использовать Caps Lock.

Полная поддержка всех русских букв отложена в следующий OpenSpec change `implement-russian-caps-word`, который будет создан после аппаратной проверки этой миграции.
