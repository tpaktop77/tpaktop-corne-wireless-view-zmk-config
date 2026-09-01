## 1. Planning и исходные данные

- [x] 1.1 Проверить default branches, remote HEAD SHA и открытые PR обоих репозиториев; зафиксировать `master@8e885d4`, `main@5d674ed` и отсутствие открытых PR в proposal/design
- [x] 1.2 Создать рабочую ветку `20260830__complete_graphite_zmk_migration` до файловых изменений и проверить, что она ответвлена от актуального target master
- [x] 1.3 Инициализировать repo-local OpenSpec и создать proposal, design и шесть обязательных delta specs; проверить их через `openspec status`

## 2. Документационный baseline до кода

- [x] 2.1 Создать `docs/migration-matrix.md` с source/target трассировкой Graphite, QWERTY, thumbs, System, каждого combo/action, Auto Shift и exclusions; проверить наличие всех обязательных колонок
- [x] 2.2 Создать `docs/layout.md` со схемами всех активных слоёв, position numbering, System controls, thumb entry и Graphite/QWERTY switching; сверить 42 позиции каждого слоя
- [x] 2.3 Создать `docs/os-profile.md` с default Windows, API, таблицей действий, volatile/independent constraints и выбором Voice consumer usage; сверить 13 actions
- [x] 2.4 Создать `docs/test-matrix.md` с разделёнными static/CI/manual USB/BLE тестами всех acceptance areas; проверить наличие обеих половин, nice!view и Studio

## 3. Repo-local OS profile module

- [x] 3.1 Добавить `zephyr/module.yml`, `CMakeLists.txt` и `Kconfig` по ZMK v0.3 module contract; проверить обнаружение файлов и central-role source condition
- [x] 3.2 Добавить dt-binding header с тремя profiles и 13 actions и devicetree bindings/dtsi для `&os_set` и `&os_action`; проверить отсутствие magic profile/action numbers в keymap API
- [x] 3.3 Реализовать volatile OS state с default Windows и `&os_set`; проверить статически отсутствие settings/flash/layer/BT dependencies
- [x] 3.4 Реализовать централизованную `[profile][action]` HID-таблицу и `&os_action` через штатный `&kp`; сверить все 39 table cells со spec
- [x] 3.5 Реализовать симметричный press/release с сохранением выбранного keycode на время удержания; проверить кодовый путь смены profile между press/release и consumer release

## 4. Слои и раскладки

- [x] 4.1 Добавить именованные константы `GRAPHITE`–`STUDIO_EXTRA_3` и удалить необъяснимые layer numbers; проверить поиском все layer behaviors/combo scopes
- [x] 4.2 Заменить layer 0 точным Graphite body, `&none` вместо F22/F23/F24 и `&to QWERTY`; сверить 36 body positions с layout spec
- [x] 4.3 Добавить минимальный QWERTY с буквами, `&none`, одинаковыми thumbs и `&to GRAPHITE`; проверить отсутствие запрещённых body keycodes
- [x] 4.4 Перенумеровать Numbers, Navigation, Function, Vim, Symbols и Smiles без функциональной переработки Navigation; сверить все межслойные ссылки и display names
- [x] 4.5 Добавить System layer с BT0–BT4, OS selectors и BT Clear на требуемых позициях; сверить position map
- [x] 4.6 Реализовать BT Clear macro-wrapper и tap-preferred hold 1500 мс; проверить, что short-tap branch равен `&none`, а `&bt BT_CLR` доступен только hold branch

## 5. Auto Shift и positional combo

- [x] 5.1 Добавить отдельный Auto Shift hold-tap с `tapping-term-ms = 250`, `quick-tap-ms = 0` и tap-preferred flavor; проверить независимость от `&lt.quick-tap-ms`
- [x] 5.2 Применить `AS()` ко всем и только английским буквам Graphite/QWERTY; проверить количество букв и отсутствие AS на non-letter bindings
- [x] 5.3 Преобразовать буквенные combo PR #2 на `layers = <GRAPHITE QWERTY>`, арифметические — на `<NUMBERS>`; проверить отсутствие duplicate key-position definitions на одинаковом scope
- [x] 5.4 Переименовать 13 Windows combo и заменить bindings на `&os_action`; проверить соответствие action mapping и отсутствие `windows_combo`/hardcoded OS chords
- [x] 5.5 Добавить `slow-release` восьми modifier combo и Smiles combo, не добавляя его обычным command combo; проверить точный список из девяти nodes
- [x] 5.6 Сохранить overlapping Copy/Cut и arithmetic combo с timeout 50 мс; проверить присутствие коротких и длинных nodes

## 6. Документация после реализации и статическая проверка

- [x] 6.1 Обновить migration/layout/OS/test docs по фактическим именам файлов, positions и behaviors; проверить, что Navigation отмечен `migrated, OS adaptation deferred`
- [x] 6.2 Проверить exclusions поиском: нет русских слоёв, QMK layer 2 macros, mouse/trackpad, OS layers, persistence и BT↔OS coupling
- [x] 6.3 Выполнить `git diff --check`, структурный аудит 42 bindings на активный слой и проверку всех именованных ссылок; устранить ошибки
- [x] 6.4 Выполнить OpenSpec strict validation/verify и отметить все выполненные implementation tasks; проверить отсутствие validation errors

## 7. CI, pull request и handoff

- [x] 7.1 Закоммитить и отправить рабочую ветку, создать PR с summary, OpenSpec links и описанием использования смерженного combo PR #2; проверить URL PR
- [x] 7.2 Дождаться успешной GitHub Actions сборки corne_left с nice!view/Studio и corne_right с nice!view; приложить ссылки и статусы в PR/docs
- [x] 7.3 При CI failure исправить implementation в той же ветке и повторять проверки до двух успешных builds; проверить финальный commit SHA против PR head
- [x] 7.4 Перечислить в PR все оставшиеся ручные тесты владельца из `docs/test-matrix.md`, не помечая их выполненными; проверить USB, BLE, macOS Voice, slow-release, display и Studio rows
- [x] 7.5 Оставить OpenSpec change активным и не архивировать до аппаратного подтверждения владельцем; проверить наличие change в `openspec list`

## 8. Follow-up: позиция OS selectors

- [x] 8.1 Сдвинуть Windows/macOS/Linux на System positions 19–21 и синхронизировать layout, OS profile, migration и test docs; проверить, что position 18 равна `&none`, а combo не изменились
- [x] 8.2 Назначить `&kp ESC` на Graphite position 0 и синхронизировать layout, migration и test docs; проверить, что QWERTY и positional combo не изменились
- [ ] 8.3 Выполнить `git diff --check`, OpenSpec strict validation и сборку обеих половин; затем закоммитить и отправить follow-up в существующий PR #3
