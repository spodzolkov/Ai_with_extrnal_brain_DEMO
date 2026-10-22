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

## 🖥️ 3. Налаштування через Графічний Інтерфейс (GUI) Десктопного Клієнта ChatGPT

Офіційний десктопний додаток **ChatGPT для Windows та macOS** підтримує інтеграцію з локальними файлами та середовищем розробки через власний графічний інтерфейс.

### Крок 1. Налаштування «Work with Apps» у ChatGPT Desktop
1. Завантажте та встановіть додаток [ChatGPT Desktop](https://openai.com/chatgpt/desktop/).
2. Відкрийте графічні налаштування:
   - Натисніть на аватар свого профілю (у лівому або правому нижньому куті) -> оберіть **Settings**.
3. Перейдіть до розділу **Work with Apps**:
   - Увімкніть доступ для **Visual Studio Code** та **Terminal**.
   - Це дозволяє викликати вікно швидкого чату комбінацією `Alt + Space` (Windows) або `Option + Space` (macOS) безпосередньо поруч із відкритим проєктом: ChatGPT автоматично зчитуватиме виділені фрагменти ТЗ або коду.

### Крок 2. Створення кастомного помічника (Custom GPT) через GUI
1. У лівому бічному меню клієнта натисніть **Explore GPTs** -> кнопка **+ Create** (Створити GPT).
2. У верхній частині перейдіть із вкладки *Create* на вкладку **Configure**:
   - **Name**: `Knowledge Base & Requirements Manager`
   - **Description**: `AI-асистент для нормалізації, аналізу та ведення бази знань у Notion`
   - **Instructions**: вставте повний текст із файлу [`.github/copilot-instructions.md`](file:///.github/copilot-instructions.md).
3. **Завантаження знань (Knowledge Base в GUI):**
   - У блоці **Knowledge** натисніть графічну кнопку **Upload files**.
   - Виділіть та завантажте файли інструкцій з папки [`.agents/skills/`](file:///.agents/skills/):
     * `requirements-normalizer/SKILL.md`
     * `requirements-analyzer/SKILL.md`
     * `requirements-resolver/SKILL.md`
     * `requirements-decision-applier/SKILL.md`
     * `openapi-spec-generator/SKILL.md`
4. **Підключення Notion API (Actions):**
   - У блоці **Actions** натисніть кнопку **Create new action**.
   - Вкажіть OpenAPI схему для Notion API або імпортуйте ендпоінти для додавання сторінок у базу даних.
   - У полі авторизації оберіть `API Key` -> тип `Bearer` -> введіть ваш токен `ntn_...`.
5. Натисніть **Create / Update** у правому верхньому куті. Тепер ваш Custom GPT доступний у десктопному додатку!

---

## 💻 4. Налаштування Графічного Інтерфейсу в GitHub Copilot Desktop (VS Code GUI)

Для користувачів, які працюють через візуальний інтерфейс розширення GitHub Copilot у VS Code:

1. **Відкриття графічної панелі Copilot Chat:**
   - На лівій панелі активностей (Activity Bar) натисніть на іконку **Copilot Chat** (іконка чату з логотипом GitHub).
2. **Прикріплення контексту через інтерфейс (Attach Context):**
   - Біля поля введення повідомлення натисніть на графічну кнопку зі скріпкою/плюсом (`+ Attach Context`).
   - Оберіть пункт **Files...** та вкажіть інструкцію потрібного скіла (наприклад: `.agents/skills/requirements-normalizer/SKILL.md`).
3. **Налаштування параметрів агента в GUI:**
   - Відкрийте меню `File` -> `Preferences` -> `Settings` (або `Ctrl + ,`).
   - У рядку пошуку налаштувань введіть: `github.copilot.chat`.
   - Увімкніть параметр **Github > Copilot > Chat: Agent Mode** (активує здатність Copilot самостійно планувати кроки та викликати MCP інструменти).

---

## 🚀 5. Приклади Промптів для GitHub Copilot / Codex

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
