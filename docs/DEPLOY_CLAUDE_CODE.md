# 🤖 Розгортання та Використання в Claude Code (Anthropic CLI)

Цей посібник описує налаштування та запуск проєкту за допомогою офіційного CLI-інструменту **Claude Code** (`@anthropic-ai/claude-code`).

---

## 📋 1. Встановлення Claude Code

Переконайтеся, що у вас встановлено Node.js (>= 18), та встановіть Claude Code глобально:

```bash
npm install -g @anthropic-ai/claude-code
```

Перевірте авторизацію в системі Anthropic:
```bash
claude login
```

---

## 🔌 2. Підключення Notion MCP до Claude Code

Щоб надати Claude Code доступ до ваших сторінок та баз даних Notion через MCP, додайте сервер `notion`:

```bash
claude mcp add notion -- npx -y mcp-remote https://mcp.notion.com/mcp
```

### Альтернативний варіант через файл конфігурації:
Створіть або оновіть файл `.claude/mcp.json` (або `~/.claude/mcp.json`):
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

При першому виклику інструментів Notion CLI виведе URL для авторизації в браузері. Після надання доступу до вашого воркспейсу токен закешується.

---

## 🖥️ 3. Налаштування через Графічний Інтерфейс (GUI) Claude for Desktop

Окрім роботи через консольний CLI, ви можете використовувати офіційний десктопний додаток **Claude for Desktop** (для Windows та macOS).

### Крок 1. Відкриття налаштувань розробника в GUI
1. Завантажте та встановіть [Claude for Desktop](https://claude.ai/download).
2. Запустіть додаток та відкрийте налаштування через графічне меню:
   - **Windows:** меню `File` -> `Settings` (або комбінація `Ctrl + ,`).
   - **macOS:** меню `Claude` -> `Settings...` (або комбінація `Cmd + ,`).
3. У лівому меню вікна налаштувань оберіть пункт **Developer** (Розробник).

### Крок 2. Редагування конфігурації через кнопку в GUI
1. У вкладці Developer натисніть кнопку **Edit Config** (Редагувати конфігурацію).
2. Додаток автоматично відкриє системний файл конфігурації `claude_desktop_config.json` у вашому текстовому редакторі за замовчуванням:
   - **Шлях у Windows:** `%APPDATA%\Claude\claude_desktop_config.json`
   - **Шлях у macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
3. Додайте сервер `notion` у блок `mcpServers`:
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
4. Збережіть файл (`Ctrl + S` / `Cmd + S`).

### Крок 3. Перезапуск та перевірка інструментів у GUI
1. Повністю закрийте додаток Claude for Desktop (через трей/панель завдань -> *Quit*) та відкрийте його знову.
2. Відкрийте новий діалог. У правому нижньому куті поля вводу з'явиться **іконка молотка (Hammer 🔨)** — це індикатор підключених MCP серверів.
3. Натисніть на цю іконку: у випадаючому списку з'являться інструменти Notion (`notion-fetch`, `notion-create-pages`, `notion-update-page`, `notion-create-database` тощо).

### Крок 4. Робота з проєктом через інтерфейс «Projects»
1. У лівій панелі додатку натисніть **Projects** -> **Create Project**.
2. Вкажіть назву: *База знань проєкту*.
3. У блоці **Project Knowledge** натисніть кнопку **Add Content** -> **Upload Files**:
   - Завантажте файл [`CLAUDE.md`](file:///c:/Work/My_repos/Qa_auto_agen_demo/CLAUDE.md) з кореня проєкту.
   - Завантажте інструкції скілів з папки [`.agents/skills/`](file:///c:/Work/My_repos/Qa_auto_agen_demo/.agents/skills/).
4. Тепер Claude у десктопному додатку володіє всім контекстом правил і може керувати базою знань у Notion через MCP!

---

## 📑 4. Файл Контексту `CLAUDE.md`

Claude Code автоматично зчитує інструкції та контекст проєкту з файлу `CLAUDE.md` у корені репозиторію. У ньому описано:
- Розташування та призначення спеціалізованих скілів у `.agents/skills/`.
- Золоте правило незмінності суті вимог.
- Правила збереження опитувальних анкет у `clarifications/` (поза Git).
- Схему бази даних «1. Нормалізовані вимоги».

---

## 🚀 5. Запуск через Claude Code CLI та Приклади Команд

Перейдіть у директорію проєкту та запустіть інтерактивну сесію:

```bash
cd /path/to/Qa_auto_agen_demo
claude
```

### Приклади запитів у Claude Code:

1. **Ініціалізація бази знань:**
   > *«Прочитай інструкцію .agents/skills/notion-project-init/SKILL.md та ініціалізуй базу знань у Notion для проєкту 'FinTech Core'»*

2. **Нормалізація вимог із файлу:**
   > *«Прочитай файл специфікації docs/SRS.md, застосуй правила з .agents/skills/requirements-normalizer/SKILL.md, нормалізуй вимоги на атомарні твердження та завантаж їх у базу Notion»*

3. **Аудит та виявлення проблем:**
   > *«Використай скіл .agents/skills/requirements-analyzer/SKILL.md: знайди непідтверджені вимоги в Notion, перевір їх на повноту та однозначність. Якісні вимоги підтвердь, а дефекти занеси в таблицю Розділу 2»*

4. **Генерація опитувального листа:**
   > *«Застосуй .agents/skills/requirements-resolver/SKILL.md: сформуй анкету узгодження проблемних вимог у папці clarifications/»*

5. **Застосування відповідей:**
   > *«Опрацюй заповнену анкету clarifications/clarification_20261022.md за інструкцією .agents/skills/requirements-decision-applier/SKILL.md та онови статуси в Notion»*
