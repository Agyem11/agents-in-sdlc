---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中查看或配置 Playwright MCP，并通过浏览器探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不只是写代码。我们还需要处理数据和外部服务，并允许 Copilot 使用更多自动化功能。这时就需要 MCP 服务器。MCP 服务器可以让 Copilot 使用 CLI 内置功能之外的工具和服务。

本课将介绍如何：

- 了解 Model Context Protocol (MCP) 以及 Copilot CLI 如何使用它。
- 在尚未配置时添加 Playwright MCP 服务器。
- 让智能体驱动浏览器并探索筛选功能。

## 场景

单元测试和端到端测试固然重要，但验证 UI 更新还需要实际与 UI 交互。你希望 Copilot 像用户一样使用正在开发的网站，以便自动执行更多检查，并更有信心地确认更新是否符合预期。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 智能体与外部工具和服务通信提供了一种方式。借助 MCP，AI 智能体可以实时访问最新信息，并代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁。每个 MCP 服务器都提供一组可供智能体访问的工具和资源。

以下是一些常见的 MCP 服务器：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 仓库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，帮助用户发现并参与生态系统。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前先审查源代码、验证发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请将其添加到 Copilot，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 已经配置 Playwright MCP，请跳过这些设置步骤，直接进入下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

之前你手动确认了功能是否符合预期。现在使用 Copilot 中的 Playwright MCP 服务器完成同样的检查。

1. 使用以下提示，让 Copilot 调用 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确要求 Copilot 使用 MCP 服务器，它往往会自行判断。但既然你知道要使用哪个工具，明确指出也有助于获得更一致的结果，并节省一些令牌。

2. 观察 Copilot 如何列出浏览器中的各项操作步骤并检查功能。
3. 阅读报告，确认所有功能都符合预期。

Copilot 会启动服务器，通过 Playwright 与网站交互，完成后停止服务器，并向你报告结果。

## 总结和后续步骤

恭喜！你使用 Playwright MCP 服务器，让 Copilot CLI 在真实浏览器中检查功能。本课中，你：

- 了解了 Model Context Protocol (MCP) 以及 Copilot CLI 如何使用它。
- 在尚未配置时添加了 Playwright MCP 服务器。
- 让智能体驱动浏览器并探索筛选功能。

接下来创建一个[用于整合技能和浏览器工具的 QA 自定义智能体][next-lesson]。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [为 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
