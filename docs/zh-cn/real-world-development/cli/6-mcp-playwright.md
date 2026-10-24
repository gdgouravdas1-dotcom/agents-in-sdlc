---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "检查或配置 Copilot CLI 中的 Playwright MCP，并在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不仅是写代码。我们还需要处理数据和外部服务，并让 Copilot 使用更多自动化工具。MCP 服务器可以扩展 Copilot CLI 内置的能力，让它使用更多工具和服务。

在本课中，你将：

- 了解模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加该服务器。
- 请智能体驱动浏览器，探索筛选功能。

## 场景

单元测试和端到端测试很重要，但要验证 UI 更新，还需要实际与界面交互。你希望 Copilot 像用户一样使用正在开发的网站，进一步自动化更改流程，并提高对功能符合预期的信心。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post]为 AI 智能体提供了与外部工具和服务通信的方式。借助 MCP，智能体可以实时获取最新信息，并代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器在 AI 智能体与外部工具和服务之间建立连接；每个服务器代表一组不同的可用工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

此外还有许多其他 MCP 服务器。GitHub 托管了 [MCP registry][mcp-registry]，以便发现服务器并为生态系统作出贡献。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前，请审查源代码、验证发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 中尚未提供 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果已在 Copilot CLI 中配置 Playwright MCP，请跳过这些设置步骤并继续下一部分。

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

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 使用 Playwright 让 Copilot 探索功能

此前你已手动确认功能行为符合预期。现在使用 Copilot CLI 中的 Playwright MCP 服务器重复验证。

1. 使用以下提示告诉 Copilot 通过 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确要求 Copilot 使用 MCP 服务器，它会根据情况自行选择。不过，指出你希望它使用的工具，有助于让结果更一致。

2. 观察 Copilot 在浏览器中执行检查时列出的步骤。
3. 阅读报告，确认各项行为符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器，然后提供报告。

## 总结和后续步骤

你已使用 Playwright MCP 服务器，通过 Copilot CLI 在真实浏览器中探索功能。具体而言，你：

- 了解了模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它。
- 如果 Playwright MCP 服务器尚未配置，则添加了它。
- 请智能体驱动浏览器，探索筛选功能。

接下来，[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专业角色中。

## 资源

- [MCP 是什么？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [为 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
