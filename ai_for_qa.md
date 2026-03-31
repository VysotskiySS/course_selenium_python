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
