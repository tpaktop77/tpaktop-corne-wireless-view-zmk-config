## Purpose

Определяет воспроизводимую проверку обеих половин беспроводной Corne с nice!view, Studio RPC и документированными ручными аппаратными тестами.

## Requirements

### Requirement: Обе половины собираются с существующей аппаратной конфигурацией
CI SHALL успешно собирать `nice_nano_v2 + corne_left + nice_view_adapter + nice_view` и `nice_nano_v2 + corne_right + nice_view_adapter + nice_view`. Левая центральная половина SHALL сохранять snippet `studio-rpc-usb-uart`.

#### Scenario: Сборка левой половины
- **WHEN** GitHub Actions выполняет build matrix
- **THEN** сборка corne_left с nice!view и Studio snippet завершается успешно и создаёт firmware artifact

#### Scenario: Сборка правой половины
- **WHEN** GitHub Actions выполняет build matrix
- **THEN** сборка corne_right с nice!view завершается успешно и создаёт firmware artifact

### Requirement: Nice view и ZMK Studio остаются доступными
Миграция MUST NOT удалять nice!view configuration, Studio RPC или три reserved layers. Все активные слои SHALL иметь понятные display names для отображения и Studio.

#### Scenario: Проверка интеграций после сборки
- **WHEN** собранная левая половина подключена по USB
- **THEN** nice!view отображает состояние, а ZMK Studio может подключиться без использования Studio как источника истины для behaviors, macros или combo

### Requirement: Репозиторий проходит статическую и OpenSpec-проверку
Перед завершением изменения `git diff --check` SHALL проходить без ошибок, а OpenSpec strict validation/verify SHALL не содержать незакрытых требований реализации.

#### Scenario: Финальная статическая проверка
- **WHEN** реализация и документация завершены
- **THEN** whitespace-check и OpenSpec validation завершаются успешно

### Requirement: Непроверяемые в CI сценарии документированы
Test matrix SHALL перечислять ручные проверки Graphite, QWERTY, thumbs, Auto Shift, всех combo, overlap, slow-release, BT0–BT4, защищённого BT Clear, OS profiles/actions, USB, BLE, обеих половин, nice!view и Studio. Аппаратные пункты MUST оставаться отмеченными как ручные до подтверждения владельцем.

#### Scenario: Handoff владельцу
- **WHEN** pull request готов после успешного CI
- **THEN** его описание перечисляет ещё не выполненные ручные hardware tests со ссылкой на `docs/test-matrix.md`
