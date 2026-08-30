## Context

См. мотивацию в `proposal.md`. Целевая база — `master` на `8e885d4`, куда уже смержен PR #2 с 32 позиционными combo. Открытых PR в обоих репозиториях нет. Актуальный QMK reference — remote `main` на `5d674ed`; существующая локальная QMK-копия отстаёт, поэтому анализ выполнен по отдельному read-only snapshot в `/tmp`, исходный репозиторий не изменяется.

Текущее состояние расходится с заданием:

- слой 0 — полный QWERTY с Tab/Ctrl/Shift/пунктуацией, а не Graphite;
- слои 1/2 заняты общими Lower/Raise, Numbers начинается с 3;
- thumb-клавиши всё ещё ссылаются на Lower/Raise и числовые индексы;
- combo из PR #2 ограничены текущим Base, а системные действия зафиксированы Windows keycodes;
- modifier/Smiles combo не имеют `slow-release`;
- нет независимого OS profile, System layer и Auto Shift;
- три reserved Studio layers уже находятся на индексах 9–11 и должны остаться там.

`config/west.yml` закрепляет ZMK `v0.3`. Официальный `build-user-config.yml@v0.3` автоматически добавляет корень config-репозитория как `ZMK_EXTRA_MODULES`, если существует `zephyr/module.yml`; значит behavior module можно хранить в этом же репозитории. ZMK v0.3 `&kp` обрабатывает press/release через encoded HID usage, включая modifiers и consumer page, а `zmk_behavior_invoke_binding()` позволяет безопасно делегировать ему динамически выбранный action.

## Goals / Non-Goals

**Goals:**

- Завершить английскую Graphite-миграцию без дублирования layout по ОС.
- Сохранить behavior state и OS-specific HID mapping в одном repo-local ZMK module.
- Сохранить физические позиции combo независимо от содержимого body keymap.
- Сделать структуру слоёв и все ссылки читаемыми через именованные константы.
- Оставить аппаратную конфигурацию, nice!view и Studio совместимыми с текущим CI.

**Non-Goals:**

- Persistence или автоматическое определение OS profile, связь OS с Bluetooth endpoint.
- Русские layout/symbol/smiles, QMK language switching и старый QMK layer 2.
- Mouse, trackpad, navigator/automouse и переработка Navigation под ОС.
- Перенос исключённых macros/shortcuts, RGB, bootloader, EEPROM/profile clear.
- Автоматизация физических USB/BLE/macOS проверок, недоступных CI.

## Decisions

### 1. Фиксированная именованная структура слоёв

Используется порядок:

```text
0 GRAPHITE
1 QWERTY
2 NUMBERS
3 NAVIGATION
4 FUNCTION
5 VIM
6 SYMBOLS
7 SMILES
8 SYSTEM_BT
9 STUDIO_EXTRA_1
10 STUDIO_EXTRA_2
11 STUDIO_EXTRA_3
```

Graphite остаётся default layer 0. `&to QWERTY` поднимает альтернативный слой над Graphite, `&to GRAPHITE` отключает его и возвращает default. Это обеспечивает предсказуемый reset в Graphite без persistent settings. Альтернатива с двумя default layers отвергнута: она сложнее и допускает сохранение нежелательного base state.

Numbers/Navigation/Function/Vim/Symbols/Smiles переносятся без функциональной переработки; меняются только индексы и ссылки. Lower/Raise удаляются. Reserved nodes остаются последними для Studio.

### 2. Одинаковые thumb bindings в двух буквенных слоях

Шесть thumb-позиций задаются явно и одинаково:

```text
&mo SYSTEM_BT
&lt SYMBOLS BSPC
&lt NUMBERS DEL
&lt FUNCTION TAB
&lt NAVIGATION SPACE
&none
```

Глобальная настройка `&lt.quick-tap-ms = 250` сохраняется. Дублирование bindings на Graphite/QWERTY выбрано вместо `&trans`, чтобы QWERTY был самодостаточным и Studio/layout documentation показывали фактическое назначение.

### 3. System/Bluetooth layer вместо Lower/Raise

System использует body positions 0–4 для BT0–BT4; position 24 — защищённый BT Clear; positions 18–20 — Windows/macOS/Linux. Остальные body-позиции `&none`, thumbs прозрачны только для корректного release уже нажатого momentary entry.

BT Clear оборачивается в zero-parameter macro, вызывающий `&bt BT_CLR`. Отдельный tap-preferred hold-tap с `tapping-term-ms = 1500` вызывает macro только как hold action, а короткий tap вызывает `&none`. Tap-preferred выбран специально: нажатие другой клавиши до 1500 мс не должно преждевременно очистить профиль. Прямая передача `&bt` в hold-tap отвергнута, потому что `&bt` имеет два binding cells.

### 4. Repo-local ZMK module с двумя публичными behaviors

Корень репозитория становится Zephyr module:

```text
CMakeLists.txt
Kconfig
zephyr/module.yml
dts/behaviors/os_profile.dtsi
dts/bindings/behaviors/tpak,behavior-os-set.yaml
dts/bindings/behaviors/tpak,behavior-os-action.yaml
include/dt-bindings/tpak/os_profile.h
include/tpak/os_profile.h
src/os_profile.c
src/behaviors/behavior_os_set.c
src/behaviors/behavior_os_action.c
```

Исходники behavior собираются только на central role, как рекомендует ZMK для keymap behaviors split-клавиатур. Devicetree instance `os_action` содержит phandle на штатный `&kp`; implementation извлекает binding и вызывает его через `zmk_behavior_invoke_binding()` вместо прямого изменения HID reports. Это сохраняет штатную обработку keyboard/consumer pages и modifiers.

`&os_set` принимает один profile parameter и меняет только статическое RAM-состояние, инициализированное Windows. `&os_action` принимает один action parameter и выбирает encoded keycode из таблицы `[profile][action]`. Таблица содержит все 13 действий из spec в одном C-файле.

Для каждого action хранится encoded keycode, выбранный на press, до release. Поэтому даже если профиль изменится между событиями, release делегируется `&kp` с тем же кодом и не оставляет modifier/consumer usage зажатым. Отдельные OS layers, macros на каждое действие и прямое управление endpoint HID отвергнуты как дублирующие или менее безопасные.

macOS Voice использует официальный ZMK keycode `C_VOICE_COMMAND` (HID Consumer usage 0xCF), соответствующий QMK `host_consumer_send(0xCF)`. Совместимость конкретного macOS host отмечается как manual test, потому что официальная таблица ZMK не гарантирует её для всех macOS версий.

### 5. Auto Shift как стандартный параметризованный hold-tap

В keymap определяется отдельный behavior `as`:

```dts
#define AS(keycode) &as LS(keycode) keycode

as: auto_shift {
    compatible = "zmk,behavior-hold-tap";
    #binding-cells = <2>;
    tapping-term-ms = <250>;
    quick-tap-ms = <0>;
    flavor = "tap-preferred";
    bindings = <&kp>, <&kp>;
};
```

Это официальный ZMK pattern, адаптированный к требуемым 250 мс. `AS()` применяется только к буквенным body bindings Graphite/QWERTY. Tap-preferred предотвращает превращение быстрой последовательности букв в Shift только из-за interrupt; удержание по timeout остаётся hold. Использование глобального QMK Auto Shift или изменение `&lt` отвергнуто: в ZMK v0.3 нужное поведение выражается hold-tap и должно быть независимо от thumbs.

### 6. Combo PR #2 преобразуется на месте

PR #2 уже смержен, поэтому новая ветка основана на нём; cherry-pick и дублирование не нужны. Существующие combo nodes переименовываются и редактируются:

- буквенные: `layers = <GRAPHITE QWERTY>`;
- арифметические: `layers = <NUMBERS>`;
- Windows-suffixed системные combo: нейтральные имена и `&os_action`;
- modifier и Smiles: `slow-release`;
- timeout: 50 мс;
- positions 32–34 сохраняются, хотя Graphite bindings становятся `&none`.

ZMK поддерживает fully/partially overlapping combo, поэтому Copy/Cut и arithmetic pairs сохраняются. Обычные command combo не получают `slow-release`.

### 7. Документация является частью реализации

`docs/migration-matrix.md` трассирует каждый перенос к QMK `5d674ed` и ZMK position/action. `docs/layout.md` фиксирует position numbering и все слои. `docs/os-profile.md` фиксирует API и таблицу. `docs/test-matrix.md` разделяет CI/static проверки и ручные hardware tests; непроверенные физически пункты не объявляются пройденными.

### 8. Проверка выполняется в два этапа

До push выполняются `git diff --check`, OpenSpec strict validation и доступные структурные проверки. После push GitHub Actions собирает обе половины, включая Studio snippet слева. При ошибке CI исправление выполняется в той же ветке, tasks и validation повторяются. OpenSpec change не архивируется в рамках этой задачи: hardware tests остаются владельцу, а пользователь прямо запретил закрывать change до полной проверки.

## Risks / Trade-offs

- [Auto Shift hold-tap добавляет до 250 мс задержки одиночной буквы] → используется официальный tap-preferred pattern; реальная скорость и misfire проверяются по USB/BLE до возможного отдельного tuning change.
- [Большое число пересекающихся combo взаимодействует с Auto Shift] → позиции и единый timeout сохраняются из уже собранного PR #2; test matrix отдельно проверяет короткие/длинные chords и быстрый набор.
- [Consumer Voice может различаться по версии macOS] → используется официальный `C_VOICE_COMMAND`, выбор задокументирован, macOS Voice оставлен обязательным manual test.
- [Смена profile во время удержания action] → release использует сохранённый encoded keycode press, а не текущий profile.
- [BT Clear может сработать от interrupt] → tap-preferred hold-tap не разрешает hold до 1500 мс только из-за другой клавиши.
- [Перенумерация слоёв ломает скрытую числовую ссылку] → все ссылки ищутся статически, заменяются константами и проверяются сборкой обеих половин.
- [Repo-local module может не подключиться CI] → дизайн следует ветке `v0.3` workflow: наличие `zephyr/module.yml` активирует `ZMK_EXTRA_MODULES`; обе матрицы CI обязательны.
- [CI не подтверждает физическую release-семантику, BLE timing и display runtime] → эти проверки остаются явно незакрытыми manual rows, перечисляются в PR.

## Migration Plan

1. Зафиксировать OpenSpec artifacts и documentation contract до кода.
2. Добавить module scaffold и behaviors, затем подключить dt-binding API в keymap.
3. Перестроить именованные слои, Graphite/QWERTY, thumbs и System.
4. Преобразовать combo PR #2 без дублирования и добавить Auto Shift.
5. Заполнить четыре документа, выполнить static/OpenSpec validation.
6. Закоммитить, push, создать PR и дождаться обеих GitHub Actions builds.
7. При CI failure исправить в ветке; при успехе передать владельцу manual hardware matrix.

Rollback выполняется revert коммита/PR: исходная точка `8e885d4` остаётся полностью собираемой, QMK reference не затрагивается.

## Open Questions

- Подтвердит ли конкретная версия macOS вызов Siri через `C_VOICE_COMMAND` — проверяется владельцем без изменения API или task breakdown.
- Потребуют ли физические USB/BLE тесты отдельного изменения timeout 50 мс или Auto Shift 250 мс — текущие значения фиксированы этим change, дальнейший tuning выносится отдельно.
