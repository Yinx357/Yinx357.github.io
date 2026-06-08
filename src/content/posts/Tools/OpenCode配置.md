---
title: OpenCode配置
published: 2026-05-25
image: api
category: 教程
tags: [AI, Tool, OpenCode, 开发工具]
---
# OpenCode配置

## 自定义 API BaseURL
OpenCode 的配置文件通常位于 `~/.config/opencode/opencode.json`（全局）或项目根目录下的 `opencode.json`（项目级）。

你需要使用 `provider` 字段来定义一个新的提供商，或者覆盖现有的提供商配置。
### 方法一：覆盖现有提供商（推荐）
如果你只是想把 OpenCode 默认的 `openai` 提供商指向你的中转地址：

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "openai": {
      "options": {
        "baseURL": "https://api.your-custom-domain.com/v1",
        "apiKey": "sk-your-oneapi-key"
      }
    }
  }
}
```

### 方法二：添加自定义提供商
如果你想保留官方 OpenAI 配置，同时添加一个名为 "MyAPI" 的自定义渠道：

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "myapi": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "My Custom API",
      "options": {
        "baseURL": "https://api.your-custom-domain.com/v1",
        "apiKey": "sk-your-oneapi-key"
      },
      "models": {
        "gpt-4o": {
          "name": "GPT-4o (OneAPI)"
        },
        "claude-3-5-sonnet-20240620": {
          "name": "Claude 3.5 Sonnet"
        }
      }
    }
  }
}
```

配置完成后，运行 `opencode`，你就可以在模型列表中选择 `My Custom API` 下的模型了。

## OpenCode 中文支持指南

### 方法一：全局配置文件
你可以在全局配置文件 `~/.config/opencode/opencode.json` 中添加通用指令：

```json
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": [
    "请始终使用简体中文进行回复，除非我明确要求使用其他语言。",
    "涉及编程术语时，请保留英文原文并（在必要时）提供中文解释。"
  ]
}
```

### 方法二：项目级配置 (AGENTS.md)
如果你希望在特定项目中让 AI 遵循特定的语言规范（例如：提交信息必须用中文，或者必须用英文），可以在项目根目录创建 `AGENTS.md` 文件：

```markdown
<!-- AGENTS.md -->
# 项目语言规范

- **日常交流**：使用简体中文。
- **代码注释**：使用英文。
- **Git Commit**：使用中文，格式遵循 "feat: 新增功能"。
```

OpenCode 会自动读取并遵循这些规则。

## 在 Oh My OpenCode 中保留原生 Plan 和 Builder Agent
安装 `oh-my-opencode` 后，默认可能会替换原生的 `plan` 和 `builder` Agent。如果你希望保留这两个原生 Agent（例如用于执行轻量级任务以节省 Token），可以通过修改配置文件来实现。

### 1. 修改配置文件
在 `~/.config/opencode/oh-my-opencode.json` 文件的第一层级中添加以下配置：

```json
{
  "sisyphus_agent": {
    "default_builder_enabled": true,
    "replace_plan": false
  }
}
```

> 详细配置项可参考 [JSON Schema↗](https://raw.githubusercontent.com/code-yeongyu/oh-my-opencode/master/assets/oh-my-opencode.schema.json)

### 2. 重启并验证
修改完成后重启 OpenCode。使用 `/agents` 命令检查，你应该能看到 `plan` 和 `opencode-builder` 两个 Agent 已恢复。

### 为什么需要这样做？
- **节省 Token**：原生 Agent 通常更轻量，适合处理简单的任务。
- **快速执行**：对于不需要复杂推理的任务，原生 Agent 响应更快。
- **灵活切换**：保留原生 Agent 后，你可以根据任务的复杂度在 Sisyphus Agent 和原生 Agent 之间灵活切换。

## 配置代理
``` powershell
# 仅当前PowerShell窗口
$env:https_proxy="http://127.0.0.1:7890"
$env:http_proxy="http://127.0.0.1:7890"

# 配置为环境变量
[Environment]::SetEnvironmentVariable("http_proxy", "http://127.0.0.1:7890", "User") 
[Environment]::SetEnvironmentVariable("https_proxy", "http://127.0.0.1:7890", "User")
```
- `"User"` 表示设置为当前用户的环境变量。
- 如果要设置为系统级变量，可以使用 `"Machine"`，但这通常需要管理员权限。