# 💻 Розгортання у VS Code та Похідних IDE (Cursor, Windsurf, Cline / Roo Code)

Цей посібник описує налаштування проєкту для роботи в середовищі **Visual Studio Code** та популярних AI-орієнтованих IDE: **Cursor**, **Windsurf**, а також розширень **Cline** та **Roo Code**.

---

## ⚙️ 1. Налаштування Notion MCP

У репозиторії вже підготовлено готовий конфігураційний файл [`.vscode/mcp.json`](file:///.vscode/mcp.json):

```json
{
  "servers": {
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

### Налаштування залежно від вашої IDE:

### 🔹 А. Cursor IDE
1. Відкрийте **Cursor Settings** (`Ctrl + Shift + J` або `Cmd + Shift + J`).
2. Перейдіть у розділ **Features** -> **MCP**.
3. Натисніть **+ Add New MCP Server**:
   - **Name**: `notion`
   - **Type**: `command` (stdio)
   - **Command**: `npx -y mcp-remote https://mcp.notion.com/mcp`
4. Натисніть кнопку підключення. При першому запуску відкриється сторінка браузера для авторизації у вашому Notion воркспейсі.

### 🔹 Б. Windsurf IDE (Codeium)
1. Відкрийте файл налаштувань MCP:
   - Windows: `%USERPROFILE%\.codeium\windsurf\mcp_config.json`
   - macOS/Linux: `~/.codeium/windsurf/mcp_config.json`
2. Додайте секцію:
   ```json
   {
     "mcpServers": {
       "notion": {
         "command": "npx",
         "args": ["-y", "mcp-remote", "https://mcp.notion.com/mcp"]
       }
     }
   }
   ```
3. Перезапустіть Windsurf Cascade.

### 🔹 В. Розширення Cline / Roo Code у VS Code
1. Відкрийте панель розширення Cline / Roo Code.
2. Натисніть іконку шестерні (Settings) -> вкладка **MCP Servers**.
3. Додайте сервер `notion`:
   - Command: `npx`
   - Args: `["-y", "mcp-remote", "https://mcp.notion.com/mcp"]`
4. Натисніть **Save**.

---

## 🧭 2. Правила для Агента (`.cursorrules` / `.windsurfrules`)

У корені проєкту створено файл `.cursorrules`, який автоматично інструктує AI-асистента в Cursor та Windsurf:
- Завжди читати інструкції відповідного скіла в `.agents/skills/<skill_name>/SKILL.md` перед виконанням задачі.
- Зберігати опитувальні анкети у `clarifications/` (папка виключена з Git через `.gitignore`).
- Дотримуватися Золотого правила незмінності суті вимог.
- Спілкуватися та генерувати документацію українською мовою.

---

## 🚀 3. Приклади Використання в Чаті IDE

У чаті агента (Cursor Composer `Ctrl + I`, Cascade `Ctrl + L` або Cline):

1. **Перший запуск (Створення структури Notion):**
   > *@notion-project-init Ініціалізуй новий проєкт у Notion: 'Онлайн Банкінг'*

2. **Нормалізація вимог:**
   > *@requirements-normalizer Візьми файл docs/spec.md, розбий його на атомарні тестовані вимоги REQ-* та збережи в базі Notion.*

3. **Аналіз на якість:**
   > *@requirements-analyzer Знайди в Notion неузгоджені вимоги, перевір їх на повноту та однозначність. Якісні вимоги підтвердь, а дефекти занеси в таблицю Розділу 2.*

4. **Формування анкети:**
   > *@requirements-resolver Згенеруй опитувальну анкету для проблемних вимог у папці clarifications/.*

5. **Застосування рішень:**
   > *@requirements-decision-applier Я заповнив анкету clarifications/clarification_20261022.md. Онови стан вимог у Notion на 'узгоджено' та постав позначки 'Вирішено'.*
