---
title: "练习 6 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中查看或配置 Playwright MCP，并在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不只是编写代码。还需要处理数据和外部服务，并允许 Copilot 使用其他自动化工具。MCP 服务器可以让 Copilot 使用 CLI 内置功能之外的工具和服务。

本课将：

- 了解模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它。
- 如果尚未配置，则添加 Playwright MCP 服务器。
- 让智能体操作浏览器，探索筛选功能。

## 场景

单元测试和端到端测试固然重要，但要验证 UI 更新，还需要实际与 UI 交互。希望 Copilot 能像用户一样使用正在开发的网站，进一步自动化更改流程，并增强对功能符合预期的信心。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post] 为 AI 智能体提供了与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时访问最新信息，并代表你执行操作。

AI 智能体通过 MCP 服务器访问工具和资源。MCP 服务器充当智能体与外部工具和服务之间的桥梁。每个 MCP 服务器都提供一组 AI 智能体可访问的工具和资源。

目前有多种常用 MCP 服务器：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

此外还有许多其他 MCP 服务器。GitHub 提供了 [MCP 注册表][mcp-registry]，方便大家查找和参与生态建设。

> [!CAUTION]
> 应像对待项目的其他依赖项一样对待 MCP 服务器。使用前，请审查其源代码、验证发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，就添加它，让 Copilot 可以像用户一样使用网站。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，直接继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

之前你已经手动确认功能符合预期。现在使用 Copilot CLI 中的 Playwright MCP 服务器完成同样的操作。

1. 使用以下提示，让 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确要求 Copilot 使用 MCP 服务器，它会自行选择合适的工具。不过，既然知道该用什么工具，明确指出也有好处！这样可以让结果更一致，还能节省一些 token。

2. 观察 Copilot 如何列出它在浏览器中执行的步骤，并验证功能。
3. 阅读报告，确认所有行为符合预期。

Copilot 会启动服务器，使用 Playwright 与网站交互，完成后停止服务器并向你报告结果。

## 总结和后续步骤

恭喜！你已从 Copilot CLI 中使用 Playwright MCP 服务器，在真实浏览器中探索功能。回顾一下，你：

- 了解了模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它。
- 如果尚未配置，则添加了 Playwright MCP 服务器。
- 让智能体操作浏览器，探索筛选功能。

接下来，[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具结合到专用角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
