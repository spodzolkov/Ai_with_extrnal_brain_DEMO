# 🚀 Розгортання та Використання в Google Antigravity IDE

Цей посібник описує, як запустити проєкт у середовищі **Google Antigravity IDE** (Antigravity 2.0) та використовувати вбудовану систему агентських скілів разом із Notion MCP.

---

## 📋 1. Передумови (Prerequisites)

1. **Встановлений Node.js**: версія `>= 18.x` (необхідний для запуску `npx` та `mcp-remote`).
2. **Акаунт Notion**: із правом створення та редагування сторінок у вашому воркспейсі.
3. **Antigravity IDE**: версія з підтримкою MCP серверів та системи кастомізацій (`.agents/skills/`).

---

## ⚙️ 2. Конфігурація Notion MCP

Antigravity IDE автоматично зчитує конфігурацію MCP з файлу [`.agents/mcp_config.json`](file:///.agents/mcp_config.json):

```json
{
  "mcpServers": {
    "notion": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mcp.notion.com/mcp"
      ]
    }
  }
}
```

### 🔑 Первинна Авторизація (OAuth):
При першому запуску MCP-сервера у вікні консолі або терміналу Antigravity з'явиться посилання для авторизації в браузері:
1. Відкрийте запропоноване посилання.
2. Оберіть ваш воркспейс Notion та сторінки, до яких надається доступ.
3. Підтвердіть доступ. Авторизаційний токен автоматично збережеться в безпечному кеші `mcp-remote` і діятиме для всіх наступних сесій без необхідності вводити ключі вручну.

---

## 🧠 3. Як Antigravity Працює зі Скілами

Усі скіли проєкту розміщено в директорії [`.agents/skills/`](file:///.agents/skills/):
- [`notion-project-init`](file:///.agents/skills/notion-project-init/SKILL.md) — первинне розгортання структури бази знань у Notion.
- [`knowledge-base-manager`](file:///.agents/skills/knowledge-base-manager/SKILL.md) — головний диспетчер маршрутизації матеріалів.
- [`requirements-normalizer`](file:///.agents/skills/requirements-normalizer/SKILL.md) — декомпозиція сирих вимог в атомарні.
- [`requirements-analyzer`](file:///.agents/skills/requirements-analyzer/SKILL.md) — перевірка якості та виявлення проблем у вимогах.
- [`requirements-resolver`](file:///.agents/skills/requirements-resolver/SKILL.md) — генерація локальних анкет для стейкхолдерів у папці `clarifications/` (поза Git).
- [`requirements-decision-applier`](file:///.agents/skills/requirements-decision-applier/SKILL.md) — валідація відповідей та оновлення статусів у Notion.
- [`openapi-spec-generator`](file:///.agents/skills/openapi-spec-generator/SKILL.md) — генерація специфікацій OpenAPI 3.0+ та матриці API.

> [!NOTE]
> Скіли також автоматично синхронізовані в глобальну конфігурацію Antigravity: `~/.gemini/config/skills/`, що дозволяє агенту використовувати їх навіть у нових вікнах.

---

## 🏁 4. Початок Роботи: Сценарії Використання

### Сценарій А: Перший старт (Чистий воркспейс)
Якщо бази знань у Notion ще немає, надішліть у чат Antigravity:
```text
Ініціалізуй новий проєкт у Notion: База знань мого проєкту
```
*Агент викличе скіл `notion-project-init`, створить кореневу сторінку, базу даних «1. Нормалізовані вимоги» зі схемою SQL DDL та 4 підпорядковані розділи.*

---

### Сценарій Б: Нормалізація сирих вимог
Передайте файл, посилання або текст:
```text
Ось файл специфікації docs/SRS.md (або тікет Jira PROJ-101 / текст ТЗ). 
Нормалізуй вимоги та збережи їх у Notion.
```
*Агент застосує `requirements-normalizer`, розіб'є текст на атомарні пункти `REQ-*` без додумувань та збереже їх у стані `потребує аналізу`.*

---

### Сценарій В: Аналіз вимог на дефекти
```text
Проаналізуй збережені нормалізовані вимоги на повноту та тестованість.
```
*Агент запустить `requirements-analyzer`: якісні вимоги узгодить (`Ким підтверджено: AI Agent`), а проблемні винесе в таблицю Розділу 2 з відкритими питаннями.*

---

### Сценарій Г: Формування анкети для стейкхолдерів
```text
Згенеруй анкету для уточнення проблемних вимог.
```
*Агент активує `requirements-resolver` та створить локальний файл `clarifications/clarification_YYYYMMDD.md` з чіткими полями відповідей поза контролем Git.*

---

### Сценарій Ґ: Застосування затверджених рішень
```text
Я заповнив анкету clarifications/clarification_20261022.md. Застосуй рішення до Notion.
```
*Агент викличе `requirements-decision-applier`, переведе вимоги в стан `узгоджено`, оновить текст та поставить позначку `✅ Вирішено` в Розділі 2.*
