## Purpose

Определяет единую английскую структуру слоёв Corne: загрузочный Graphite, минимальный QWERTY, одинаковые thumb-клавиши и сохранённые вспомогательные слои.

## Requirements

### Requirement: Graphite является единственным загрузочным базовым слоем
Система SHALL определять `GRAPHITE` как слой 0 и после каждой загрузки SHALL иметь только Graphite в качестве выбранной буквенной раскладки. Все ссылки на активные и reserved слои MUST использовать именованные константы, а не необъяснимые числовые индексы.

#### Scenario: Загрузка клавиатуры
- **WHEN** любая половина запускается после reset или подачи питания
- **THEN** активной буквенной раскладкой является `GRAPHITE` с индексом 0

#### Scenario: Именованные ссылки
- **WHEN** keymap ссылается на Numbers, Navigation, Function, Vim, Symbols, Smiles, System или reserved layer
- **THEN** ссылка использует соответствующую именованную константу

### Requirement: Graphite повторяет три нижних ряда Voyager
Graphite SHALL содержать body-позиции `ESC B L D W Z | NUMLOCK F O U J none`, `none N R T S G | Y H A E I none`, `none Q X M C V | K P none none none QWERTY`. Верхний ряд Voyager SHALL быть исключён, position `0` SHALL отправлять Escape, а бывшие `F22/F23/F24` SHALL быть `&none`.

#### Scenario: Проверка Graphite body
- **WHEN** пользователь просматривает или тестирует слой Graphite
- **THEN** все 36 body-позиций соответствуют указанной схеме и Num Lock остаётся в исходной физической позиции

#### Scenario: Переход в QWERTY
- **WHEN** пользователь нажимает правую нижнюю body-клавишу Graphite
- **THEN** система напрямую выбирает слой `QWERTY`

### Requirement: QWERTY является минимальной альтернативной раскладкой
QWERTY SHALL содержать только английские буквы в схеме `none Q W E R T | Y U I O P none`, `none A S D F G | H J K L none none`, `none Z X C V B | N M none none none GRAPHITE`, одинаковые с Graphite thumb bindings и прямую кнопку возврата. Body QWERTY MUST NOT содержать Tab, Escape, Ctrl, Shift, пунктуацию или специальные клавиши.

#### Scenario: Проверка QWERTY body
- **WHEN** выбран слой QWERTY
- **THEN** его body содержит только заданные буквы, `&none` и кнопку `&to GRAPHITE`

#### Scenario: Возврат в Graphite
- **WHEN** пользователь нажимает правую нижнюю body-клавишу QWERTY
- **THEN** система напрямую возвращается в `GRAPHITE`

### Requirement: Thumb bindings совпадают на Graphite и QWERTY
Оба буквенных слоя SHALL определять шесть thumb-позиций как: momentary System; tap Backspace/hold Symbols; tap Delete/hold Numbers; tap Tab/hold Function; tap Space/hold Navigation; `&none`. Существующая настройка `&lt.quick-tap-ms = 250` SHALL сохраняться независимо от Auto Shift.

#### Scenario: Сравнение thumb-клавиш
- **WHEN** пользователь переключается между Graphite и QWERTY
- **THEN** каждая из шести thumb-позиций сохраняет одно и то же поведение

### Requirement: Вспомогательные слои сохраняют назначение
Numbers, Navigation, Function, Vim, Symbols и Smiles SHALL оставаться функционально эквивалентны смерженной конфигурации с обновлёнными именованными ссылками. Navigation MUST NOT получать OS-aware адаптацию в рамках этого изменения. Активные слои SHALL иметь display names `Graphite`, `QWERTY`, `Numbers`, `Navigation`, `Function`, `Vim`, `Symbols`, `Smiles`, `System`; три reserved слоя SHALL сохраняться для ZMK Studio.

#### Scenario: Переходы между вспомогательными слоями
- **WHEN** пользователь использует существующие переходы Numbers→Vim, Function→Vim или Symbols→Smiles
- **THEN** открывается тот же функциональный слой после перенумерации

#### Scenario: Navigation после миграции
- **WHEN** пользователь открывает Navigation
- **THEN** его команды работают как до изменения, за исключением технически переименованных ссылок на слои
