## 1. Зафиксировать baseline и трассировку

- [x] 1.1 Перед реализацией повторно проверить default branches, HEAD SHA и открытые PR целевого и read-only QMK/Oryx репозиториев; записать фактические SHA в migration matrix и проверить, что рабочая ветка не является `master`.
- [x] 1.2 Дополнить `docs/migration-matrix.md` построчной трассировкой всех 33 букв, русских Symbols/Smiles, thumbs, языковых действий и каждого изменяемого combo; проверить наличие source layer, physical position, target position, behavior, status и validation.
- [x] 1.3 Явно внести в migration matrix исключённый верхний ряд Voyager, `%`, `₽`, `+`, Unicode, QWERTY language cycling, overlap tuning и custom Russian Caps Word; проверить, что ни один исключённый пункт не обозначен как реализуемый в этом change.

## 2. Подготовить общие определения

- [x] 2.1 Добавить именованные константы `RUSSIAN = 9`, `RUSSIAN_SYMBOLS = 10`, `RUSSIAN_SMILES = 11` и сдвинуть три `STUDIO_EXTRA` на 12–14; проверить отсутствие необъяснимых числовых ссылок и уникальность всех индексов.
- [x] 2.2 Добавить прокомментированные направленные macros входа/выхода, которые последовательно отправляют `Ctrl+Shift+2`/`Ctrl+Shift+1` и выполняют `&to RUSSIAN`/`&to GRAPHITE`; проверить macro bindings и отсутствие нового сохранённого language state.
- [x] 2.3 Расширить встроенный Caps Word `continue-list` стандартными Underscore/Backspace/Delete и ZMK usages `LBKT`, `RBKT`, `SEMI`, `SQT`, `COMMA`, `DOT`, `GRAVE`; проверить generated devicetree, соответствие QMK `KC_LBRC/KC_RBRC` и отсутствие custom `ru_caps_word` behavior в текущем change.
- [x] 2.4 Добавить два смыслово именованных и прокомментированных Russian Smiles macro для исходных `ST_MACRO_35`/`ST_MACRO_36`; проверить последовательности `Shift+6, Shift+0` и `Shift+6, Shift+9`.

## 3. Реализовать русские слои

- [x] 3.1 Добавить `RUSSIAN` с точным позиционным отображением всех 33 букв, прозрачным fallback в позициях 0/12 и направленным выходом в позиции 24; сверить все 42 bindings с layout spec и QMK migration matrix.
- [x] 3.2 Обернуть только 33 русские буквенные позиции существующим `AS(...)`; проверить `tapping-term-ms = 250`, tap/hold bindings и отсутствие Auto Shift на thumbs, symbols, smiles и language actions.
- [x] 3.3 Добавить русскую thumb-схему `SYSTEM_BT`, `BSPC/RUSSIAN_SYMBOLS`, `DEL/NUMBERS`, `TAB/FUNCTION`, `SPACE/NAVIGATION`, `none`; проверить сохранение отдельного `&lt.quick-tap-ms = 250`.
- [x] 3.4 Добавить `RUSSIAN_SYMBOLS` только из трёх нижних рядов источника и настроить удержание правого внутреннего thumb на `RUSSIAN_SMILES`; проверить прозрачную цепочку `RUSSIAN_SYMBOLS → RUSSIAN → GRAPHITE` и отсутствие переноса верхнего ряда.
- [x] 3.5 Добавить `RUSSIAN_SMILES` с макросами на позициях 19 и 20 и понятные `display-name` для трёх русских слоёв; проверить число bindings каждого слоя и Studio metadata.

## 4. Интегрировать позиционные combo

- [x] 4.1 Добавить `RUSSIAN` в scopes совместимых Enter, Tab, Caps Word, Caps Lock, восьми modifier и тринадцати OS-aware combo; проверить неизменность `key-positions`, bindings, timeout 50 мс и централизованного `&os_action`.
- [x] 4.2 Сохранить English Smiles combo на `GRAPHITE QWERTY` и добавить отдельное Russian Smiles combo 5/17 только на `RUSSIAN` с `slow-release`; проверить отсутствие пересекающихся layer scopes.
- [x] 4.3 Добавить два языковых combo на позициях 7/8/9 с отдельными scopes `GRAPHITE` и `RUSSIAN`; проверить, что каждое вызывает соответствующий направленный macro и не включено на QWERTY/служебных слоях.
- [x] 4.4 Проверить, что Vim combo не расширено на `RUSSIAN`, arithmetic combo остаются только на `NUMBERS`, все восемь modifier combo сохраняют `slow-release`, а существующие overlap-пары и их timeout не изменены.

## 5. Документировать и проверить

- [x] 5.1 Обновить `docs/layout.md` схемами `RUSSIAN`, `RUSSIAN_SYMBOLS`, `RUSSIAN_SMILES`, нумерацией позиций, thumb-переходами, language combo/позициями 24 и fallback-цепочкой; проверить соответствие 42 физическим позициям.
- [x] 5.2 Обновить `docs/os-profile.md`: описать независимость OS/BT/language, обязательные host shortcuts `Ctrl+Shift+1/2`, Russian–PC и отсутствие detection/persistence; проверить отдельные инструкции для Windows/macOS/Linux.
- [x] 5.3 Расширить `docs/test-matrix.md` тестами трёх слоёв, 33 букв, Auto Shift, прозрачности, thumbs, smiles, совместимых combo и языковой синхронизации на Windows/macOS/Linux по USB/BLE и на обеих половинах; оставить физические результаты незаполненными до выполнения владельцем.
- [x] 5.4 Добавить отдельные тесты временного Caps Word: режим продолжается через семь punctuation-backed букв, эти семь не заявляются автоматически заглавными, остальные буквы капитализируются, Caps Lock работает для всех 33; сверить формулировки с `russian-caps-word-compatibility` spec.
- [x] 5.5 Выполнить `git diff --check` и строгую OpenSpec validation/verify; устранить все ошибки и проверить, что diff не содержит изменений QMK-репозитория или overlap-ветки.
- [x] 5.6 Собрать `nice_nano_v2 + corne_left + nice_view_adapter + nice_view` со Studio snippet и соответствующую `corne_right`; записать команды, SHA и успешные результаты обеих сборок в test matrix или приложить успешные GitHub Actions checks.

## 6. Передать на аппаратную проверку и следующий этап

- [x] 6.1 Создать отдельный PR из русской ветки с summary, ссылками на OpenSpec, source SHA, результатами сборок и списком незавершённых аппаратных Windows/macOS/Linux USB/BLE тестов; проверить, что change не архивирован до согласованного завершения тестирования.
- [x] 6.2 Зафиксировать в документации следующий OpenSpec change `implement-russian-caps-word`, запускаемый только после аппаратного подтверждения русских слоёв; проверить, что текущий PR содержит только `continue-list` workaround и не содержит реализации custom Caps Word.
