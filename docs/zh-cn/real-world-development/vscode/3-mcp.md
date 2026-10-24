---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚使用智能体模式构建了筛选功能。创建拉取请求前，应先在浏览器中确认功能正常。与其手动浏览应用，不如连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器进行测试，然后发布分支并创建 PR。

在本练习中，你将：

- 了解模型上下文协议 (MCP) 以及 MCP 服务器如何为 Copilot 添加工具。
- 将 Playwright MCP 服务器添加到工作区。
- 请 Copilot 使用它在浏览器中手动测试筛选功能。
- 发布分支并为筛选功能创建拉取请求。

## 什么是模型上下文协议 (MCP)？

智能体模式可以连接编辑器之外的工具，这会大幅扩展它的能力。模型上下文协议 (MCP) 是 Copilot 使用的标准方式，可让智能体与外部工具和服务通信。

![示意图：智能体模式的内部结构，以及它如何与上下文、LLM 和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为 AI 智能体提供了与外部工具和服务通信的方式。借助 MCP，智能体可以实时获取最新信息（使用资源）并代表你执行操作（使用工具）。

MCP 服务器在 AI 智能体与外部工具和服务之间建立连接，并负责管理双方的通信。外部工具可以是现有 API，也可以是 NPM 包等本地工具。每个 MCP 服务器都提供一组不同的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供管理 GitHub 存储库的 API，可创建和更新存储库、管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能，可浏览网页、填写表单和选择按钮。

还有许多其他 MCP 服务器。GitHub 托管了 [MCP registry](https://github.com/mcp)，以便发现服务器并为生态系统作出贡献。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前，请仔细审查源代码、验证发布者并考虑安全影响。只使用可信服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区中的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 查看已预先配置的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

此 `github` 项目来自项目模板，因此 Copilot 在上一项练习中可以读取 GitHub 议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装；VS Code 通过 HTTP 连接，Copilot 首次使用其工具时需通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

再添加一个服务器。[Playwright MCP 服务器][playwright-mcp-server]可为 Copilot 提供可控的浏览器，以便测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 项，与 `github` 并列：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是本地服务器：VS Code 会运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志让 Playwright 在无可见窗口的模式下运行，这对于没有桌面环境的 Codespace 是必需的。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 进行端到端测试，通常已安装所需浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动它，并请你确认是否信任。

1. 在 `.vscode/mcp.json` 中，`playwright` 项上方会出现 **Start** 操作。选择它立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选步骤。若跳过，VS Code 会在下一步 Copilot 首次使用这些工具时启动服务器，并提示你确认是否信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可驱动的真实浏览器。智能体可以打开页面、应用筛选条件并读取结果，然后汇报观察到的行为，让你无需离开对话即可验证功能。

Playwright MCP 服务器依赖页面的[无障碍树][playwright-mcp-server]而非截图工作。智能体会像辅助技术一样，根据带标签的结构化元素（按钮、链接、列表项）进行推理。因此，快速功能检查也能作为简单的无障碍检查。

服务器已连接且应用正在运行后，请 Copilot 测试刚才构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 启动浏览器并逐步执行检查，然后报告结果。对照议题的验收标准审查报告；如果有异常，请追问或让 Copilot 修复代码，再创建 PR。

> [!NOTE]
> 测试需要应用运行在 `http://localhost:4321`。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 时可能需要下载浏览器；如果缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，发布 `filtering-vscode` 分支，以便团队审查。

1. 在 Codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示更改列表的 Source Control 面板屏幕截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish** 将分支推送到存储库。

### 创建拉取请求

可以通过 github.com 或 GitHub 命令行接口 (CLI) 创建 PR，也可以让 Copilot 查找相关议题并创建与该议题关联的 PR。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认智能体下拉菜单中选择了 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 请 Copilot 为当前分支创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据提示选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，可能需要通过 GitHub 身份验证；按照提示授权即可。
5. 查看 Copilot 如何查找议题并创建 PR。
6. 选择 Copilot 提供的链接审查 PR，但**暂时不要合并**。

## 总结和后续步骤

在本练习中，你：

- 了解了模型上下文协议 (MCP) 以及 MCP 服务器如何为 Copilot 添加工具。
- 将 Playwright MCP 服务器添加到工作区。
- 使用它在发布前通过浏览器手动测试筛选功能。
- 发布了分支并为筛选功能创建拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
