## OpenCode

Агент с бесплатными моделями

Установка

`curl -fsSL https://opencode.ai/install | bash`

MCP Chrome dev tools
- Создать `opencode.json` в ~/.opencode для глобального применения или в корне проекта для локального
```
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "chrome-devtools": {
      "type": "local",
      "command": ["npx", "-y", "chrome-devtools-mcp@latest"]
    }
  }
}
```
MCP Postgresql  
Добавьте в конфиг в блок mcp, заменив user/pass/host/port/database

```
    "postgres": {
      "type": "local",
      "command": ["npx", "-y", "@modelcontextprotocol/server-postgres", "postgresql://user:pass@host:port/database?sslmode=disable"]
    }
```
или

`opencode mcp add postgres-connection --label "PostgreSQL" --command "npx" --args "-y","@modelcontextprotocol/server-postgres","postgresql://user:pass@host:port/database?sslmode=disable"` 

Superpowers for OpenCode

Tell OpenCode:

`Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.opencode/INSTALL.md`

## Codex

Агент для платной модели OpenAI

https://github.com/openai/codex?ysclid=mne64891v9745151934

Установка

`npm install -g @openai/codex`

MCP Chrome dev tools

`codex mcp add chrome-devtools -- npx chrome-devtools-mcp@latest`

Superpowers for Codex

Tell Codex:

`Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.codex/INSTALL.md`

## Skills for agents

- https://Github.com/obra/superpowers
- https://Skills.sh

## MCP
- https://github.com/ChromeDevTools/chrome-devtools-mcp?ysclid=mmyq06omx2432332233

## Other commands

`/plugin update superpowers`

## Как пользоваться Opencode для написания автотестов

1. Используйте Plane mode (TAB) что бы поставить задачу, внести коррективы и только после этого включайте Build mode.
2. Создайте вручную заготовку нового теста либо попросите создать новый по подобию существующего.
3. Старайтесь сужать контекст через обозначение нужных файлов и описание шагов нужного сценария.
4. Создайте наборы правил для наиболее частых повторяющихся задач, что бы не писать каждый раз одно и то-же. В запросе укажите файл правил через @.
5. Лучше начать новую сессию если в рамках сессии поставленная задача решена и новая задача имеет мало пересечений с выполненной. Большой контекст может путать.
6. Можно добиться рабочего кода и только после этого попросить выполнить ревью для приведения к общему стилю проекта.
7. Можно просить изучить DOM для поиска или проверки xpath.
8. Можно просить что бы проверил тест, иногда путается как запускать, лучше так-же описать правилами. Например "Все нужное для запуска есть в conftest".
9. Если из размышлений видно что уходит в не правильную сторону: остановить (двойное нажатие Esc) написать уточнение и запустить. 
