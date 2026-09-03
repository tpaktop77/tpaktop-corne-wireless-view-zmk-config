## Why

После слияния миграции русских слоёв аппаратная проверка показала две регрессии настройки: удержание общего правого внутреннего thumb на `RUSSIAN` не открывает `NAVIGATION`, а Auto Shift с тайм-аутом 250 мс слишком часто даёт ложные заглавные буквы. Первая проблема вызвана тем, что базовый `RUSSIAN` имеет индекс 9 и перекрывает общие momentary-слои с меньшими индексами; вторая требует консервативной аппаратной настройки порога без изменения семантики Auto Shift.

## What Changes

- Перестроить именованный порядок слоёв так, чтобы все базовые слои находились ниже открываемых thumb-кластером overlay-слоёв: `GRAPHITE = 0`, `QWERTY = 1`, `RUSSIAN = 2`, затем общие и русские overlay-слои, `SYSTEM_BT = 11` и reserved-слои Studio 12–14.
- Сохранить без изменений шесть физических thumb bindings на `GRAPHITE`, `QWERTY` и `RUSSIAN`; изменить только приоритет целевых слоёв, чтобы одинаковый кластер работал одинаково.
- Сохранить вложенный порядок русских слоёв: `RUSSIAN_SYMBOLS` выше `RUSSIAN`, а `RUSSIAN_SMILES` выше `RUSSIAN_SYMBOLS`.
- Увеличить единый `tapping-term-ms` Auto Shift с 250 до 300 мс для английских и русских букв, чтобы снизить число false positive; не менять набор клавиш с Auto Shift и не менять thumb `&lt.quick-tap-ms = 250`.
- Не менять содержимое `NAVIGATION`, combo, language macros, OS profile, Bluetooth behaviors, nice!view или ZMK Studio, кроме необходимых именованных индексов и порядка layer nodes.
- Добавить структурные и ручные регрессионные проверки всех пяти используемых thumb-hold переходов с русского базового слоя.

## Capabilities

### New Capabilities

- `base-layer-thumb-consistency`: одинаковая физическая и наблюдаемая семантика thumb-кластера на `GRAPHITE`, `QWERTY` и `RUSSIAN`, включая требования к приоритету целевых overlay-слоёв.
- `auto-shift-tuning`: единый аппаратно настроенный порог Auto Shift 300 мс для букв трёх базовых раскладок при сохранении независимой настройки thumb layer-tap.

### Modified Capabilities

Синхронизированных main capability specs в `openspec/specs/` пока нет; смерженный, но не архивированный change `migrate-russian-layers` остаётся историей исходной миграции и не переписывается.

## Impact

- `config/corne.keymap`: перенумерация именованных слоёв, перестановка соответствующих layer nodes без изменения bindings и увеличение `&as.tapping-term-ms` до 300 мс.
- `docs/layout.md`, `docs/migration-matrix.md`, `docs/test-matrix.md`: исправленный порядок слоёв и аппаратные проверки thumb-переходов с `RUSSIAN`.
- OpenSpec change `migrate-russian-layers` остаётся неизменным как уже смерженный план; эта регрессия и её исправление документируются отдельным change.
- Прошивка обеих половин, nice!view и Studio должны продолжить собираться; фактическая доступность overlay-слоёв подтверждается владельцем на клавиатуре.
