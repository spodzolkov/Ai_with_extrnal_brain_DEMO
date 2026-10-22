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

## 📑 3. Файл Контексту `CLAUDE.md`

Claude Code автоматично зчитує інструкції та контекст проєкту з файлу `CLAUDE.md` у корені репозиторію. У ньому описано:
- Розташування та призначення спеціалізованих скілів у `.agents/skills/`.
- Золоте правило незмінності суті вимог.
- Правила збереження опитувальних анкет у `clarifications/` (поза Git).
- Схему бази даних «1. Нормалізовані вимоги».

---

## 🚀 4. Запуск та Приклади Команд

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
