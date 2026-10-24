---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的 AI 助手。可以在存储库的 `.github/agents` 文件夹中用 Markdown 文件定义智能体，为 Copilot 提供专门指令、最佳实践、编码模式和领域知识。团队可以将经验转化为可复用智能体，例如负责检查 [WCAG][wcag] 标准的无障碍智能体、遵循安全编码实践的安全智能体，或维护测试模式的测试智能体。

自定义智能体可以定义在项目的 `.github/agents` 文件夹中，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少有 `name` 和 `description` 字段，后面是定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体和智能体技能

自定义智能体和[智能体技能][agent-skills-concept]有些相似：两者主要都通过 Markdown 文件定义，并指导 AI 如何执行操作。可以将自定义智能体理解为**执行工作的角色**，将技能理解为**工具**。

自定义智能体有自己的上下文窗口，能够在工作中编排技能，甚至调用其他智能体。本练习中的无障碍智能体会根据无障碍准则审查并更新网站，也可以调用 PR 工作流技能或运行和管理测试的技能。

> [!NOTE]
> 自定义智能体没有唯一的正确写法。和其他 AI 工作流一样，应通过测试和迭代找到适合环境和场景的方案。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用众筹平台，不论其视觉能力或偏好如何。近期用户反馈显示，部分用户认为当前深色主题难以阅读，因为文字和背景颜色的对比度不足。为解决这个无障碍问题，设计团队希望添加可切换的高对比度模式。

无障碍功能非常重要，因此要尽快实现。接下来使用自定义智能体生成此功能。

在本练习中，你将：

- 检查现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用该智能体实现高对比度模式。

## 检查无障碍自定义智能体

项目已为无障碍工作预先创建自定义智能体。先查看其内容，了解它如何引导 Copilot。

返回 Codespace，打开终端，并从 `main` 创建新分支，将无障碍工作与练习 3 中的筛选功能 PR 分开：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看其中包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体的 frontmatter 必须包含 `name` 和 `description`。

3. 查看后续部分，其中包括无障碍网站代码生成的核心职责、最佳实践、HTML/CSS/JavaScript 代码示例，以及常见问题和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。选择智能体后，该会话会使用它的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的对话。不同练习的模式和模型可能不同；相关步骤会注明需要使用的选项。

1. 如果 Chat 视图中尚未选择 **Agent**，请从智能体下拉菜单中选择它。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 在 Chat 视图底部打开智能体下拉列表（默认显示 **default**）。
3. 从列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会探索代码库、提出更改并编辑项目文件。每项编辑都会在 Chat 中显示文件路径和差异。
6. **Files changed** 指示器会显示此会话中已修改文件的数量。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会修改 Astro 组件、CSS 和相关测试等现有文件。

下一项练习将介绍如何审查和引导正在进行的工作。

## 总结和后续步骤

本课探索了 GitHub Copilot 中的[自定义智能体][custom-agents]。自定义智能体是针对特定任务和领域定制的 AI 助手，可将团队经验和标准转化为可复用指导。

本课介绍了：

- 自定义智能体的定义方式。
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来，[监控并引导智能体工作][next-lesson]，审查正在进行的更改，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [Awesome Copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
