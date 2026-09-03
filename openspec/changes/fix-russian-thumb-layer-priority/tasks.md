## 1. Зафиксировать исходное состояние

- [x] 1.1 Проверить merge PR #5, обновить локальный `master` до `330fb1029ebb8c7e2d113bea1559c785f4b4f980` и создать отдельную ветку `20260903__fix_russian_thumb_layer_priority`; проверить, что работа не ведётся в `master`.
- [x] 1.2 Зафиксировать текущие шесть thumb bindings трёх базовых слоёв и содержимое общих overlay nodes до перестановки; после изменения подтвердить, что bindings не изменились.

## 2. Исправить приоритет слоёв

- [x] 2.1 Перенумеровать именованные константы в порядке `GRAPHITE`, `QWERTY`, `RUSSIAN`, `NUMBERS`, `NAVIGATION`, `FUNCTION`, `VIM`, `SYMBOLS`, `SMILES`, `RUSSIAN_SYMBOLS`, `RUSSIAN_SMILES`, `SYSTEM_BT`, Studio 12–14 и проверить уникальность индексов.
- [x] 2.2 Переставить layer nodes точно в порядке констант, не меняя их bindings; проверить соответствие node index каждой именованной константе и 42 bindings на каждом активном слое.
- [x] 2.3 Проверить структурные неравенства `SYSTEM_BT/NUMBERS/FUNCTION/NAVIGATION > RUSSIAN`, `VIM > NUMBERS/FUNCTION`, `SMILES > SYMBOLS`, `RUSSIAN_SMILES > RUSSIAN_SYMBOLS > RUSSIAN` и отсутствие необъяснимых числовых layer references.
- [x] 2.4 Сравнить thumb bindings `GRAPHITE`, `QWERTY`, `RUSSIAN` с baseline и подтвердить, что позиция 37 отличается только `SYMBOLS`/`RUSSIAN_SYMBOLS`, а содержимое `NAVIGATION`, combo и custom behaviors не изменилось.
- [x] 2.5 Увеличить только `&as.tapping-term-ms` с 250 до 300 мс; проверить сохранение `&as.quick-tap-ms = 0`, `&lt.quick-tap-ms = 250` и неизменность всех `AS(...)` bindings.

## 3. Обновить документацию

- [x] 3.1 Обновить `docs/layout.md` единой таблицей индексов и объяснением приоритета base/overlay; проверить схемы и thumb-переходы `RUSSIAN` на позициях 36–41.
- [x] 3.2 Обновить `docs/migration-matrix.md` актуальными target layer indices и отдельной записью аппаратно найденной регрессии; проверить отсутствие устаревших утверждений `RUSSIAN = 9` и `SYSTEM_BT = 8`.
- [x] 3.3 Расширить `docs/test-matrix.md` ручными тестами System, Russian Symbols, Numbers, Function и Navigation с `RUSSIAN` по USB и BLE; оставить аппаратные результаты незаполненными до проверки владельцем.
- [x] 3.4 Обновить layout, migration matrix и test matrix для Auto Shift 300 мс; добавить проверки false positive при быстром наборе и намеренного hold на Graphite/QWERTY/Russian по USB/BLE.

## 4. Проверить и передать

- [x] 4.1 Выполнить `git diff --check`, строгую `openspec validate fix-russian-thumb-layer-priority --strict` и статические проверки порядка/числа bindings; устранить все ошибки.
- [ ] 4.2 Собрать обе штатные конфигурации nice!view, включая Studio snippet на `corne_left`, и записать успешные GitHub Actions checks либо локальные результаты в test matrix.
- [ ] 4.3 Создать отдельный PR с причиной регрессии, ссылками на OpenSpec, результатами сборки и списком оставшихся аппаратных thumb-тестов; не архивировать change до подтверждения на клавиатуре.
