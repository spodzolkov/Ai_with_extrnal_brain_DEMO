# 🧠 Розгортання та Використання в OpenAI Codex та GitHub Copilot

Цей посібник описує налаштування та використання проєкту в середовищах на базі **OpenAI Codex**, **GitHub Copilot Chat** (VS Code, JetBrains, GitHub.com) та **ChatGPT / OpenAI Assistants**.

---

## 📋 1. Як OpenAI Codex та GitHub Copilot взаємодіють із проєктом

GitHub Copilot та моделі сімейства Codex автоматично зчитують контекстні правила репозиторію з файлу:
- [`.github/copilot-instructions.md`](file:///.github/copilot-instructions.md)

Цей файл гарантує, що Copilot / Codex знає:
1. Призначення та розташування всіх скілів у папці `.agents/skills/`.
2. Правила нормалізації, аудиту, формування анкет та застосування рішень.
3. Вимогу вести всі артефакти та діалоги українською мовою.
4. Правило ізоляції опитувальних анкет у папці `clarifications/` (поза Git).

---

## 🔌 2. Підключення до Notion для OpenAI / Copilot

### 🔹 Варіант А: GitHub Copilot Workspace / VS Code Copilot Chat
1. Переконайтеся, що у VS Code налаштовано Notion MCP сервер через [`.vscode/mcp.json`](file:///.vscode/mcp.json):
   ```json
   {
     "servers": {
       "notion": {
         "command": "npx",
         "args": ["-y", "mcp-remote", "https://mcp.notion.com/mcp"]
       }
     }
   }
   ```
2. У Copilot Chat увімкніть режим агента (`@workspace` або `#mcp`).

### 🔹 Варіант Б: OpenAI API / Custom GPTs / ChatGPT Actions
Якщо ви використовуєте кастомний GPT або Assistant API з OpenAI:
1. Створіть внутрішню інтеграцію в Notion: [Notion My Integrations](https://www.notion.so/my-integrations) та отримайте `Internal Integration Secret` (токен `ntn_...`).
2. Надайте цій інтеграції доступ до кореневої сторінки «База знань проєкту» (три крапки `...` -> *Connect to* -> оберіть вашу інтеграцію).
3. Додайте інструкції з `.github/copilot-instructions.md` у поле **Instructions** вашого GPT.

---

## 🚀 3. Приклади Промптів для GitHub Copilot / Codex

У чаті Copilot або під час кодогенерації:

### 1. Ініціалізація бази знань:
> *@workspace Прочитай інструкцію `.agents/skills/notion-project-init/SKILL.md`. Сформуй запити для створення структури бази знань у Notion для проєкту 'Fintech Payment Gateway'.*

### 2. Нормалізація сирих вимог:
> *@workspace Використай правила з `.agents/skills/requirements-normalizer/SKILL.md`. Нормалізуй текст нижче на атомарні вимоги REQ-*, збережи суворе дотримання суті без власних додумувань:*
> ```
> [Вставте текст ТЗ або посилання на файл]
> ```

### 3. Аудит на дефекти та тестованість:
> *@workspace За правилами `.agents/skills/requirements-analyzer/SKILL.md` перевір нормалізовані вимоги. Знайди логічні дефекти та сформулюй варіанти рішень з відкритими запитаннями.*

### 4. Генерація опитувальної анкети:
> *@workspace За інструкцією `.agents/skills/requirements-resolver/SKILL.md` сформуй файл анкети у папці clarifications/ для проблемних вимог з чіткими полями відповідей.*

### 5. Застосування відповідей:
> *@workspace Прочитай `.agents/skills/requirements-decision-applier/SKILL.md`. Опрацюй відповіді людини з clarifications/clarification_20261022.md та покажи оновлені записи.*
