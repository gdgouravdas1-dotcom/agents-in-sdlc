---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本工作坊中，你让 GitHub Copilot 执行了多项改善用户体验和添加功能的任务：为代码补充文档、实现设计团队可继续迭代的相关游戏功能，以及添加高对比度和浅色模式等无障碍功能。现在检查代码更改，必要时向 Copilot 提供反馈，然后再开放团队审查。

### 场景

使用生成式 AI 不会改变软件设计和 DevOps 的基本原则。始终应审查生成的代码，并遵循正常的 DevOps 流程。现在审查 Copilot 创建的文档、相关游戏和无障碍功能 PR。

## GitHub Copilot 云智能体的安全性

云智能体会异步、无人监督地执行任务，因此有一些安全限制：

- Copilot 只能读取存储库，并且只能写入它为代码工作使用的分支。
- 云智能体在 GitHub Actions 中运行，并在独立的临时环境中工作。
- GitHub Actions 工作流必须经人工批准后才能运行。
- 默认情况下，[对外部资源的访问受到限制][agent-firewall]，包括 MCP 服务器。

## 审查生成的文档

先使用 GitHub.com 的标准 PR 界面查看 Copilot 创建的第一个 PR：为代码添加文档。

> [!NOTE]
> 查看 PR 时可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认对外部资源的访问受到限制，包括外部 MCP 服务器。可以根据需要[自定义或禁用云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待它完成，这可能需要几分钟。

4. PR 准备好后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新增的 TSDoc 文档注释和其他文档。具体更改可能不同。留意 TSDoc 注释、TypeScript 约定和文件注释头；这些内容来自练习 2 中查看的自定义指令文件。
6. 审查完成后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准。
8. 如果有，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会显示在 PR 检查区域中。正常情况下，单个 Astro 应用的项目检查会通过，这可能需要几分钟。

## 请求 GitHub Copilot 修改

在 PR 评论或代码行内评论中提及 Copilot，可以要求它进行修改。Copilot 会读取评论并启动新会话。由于生成结果具有非确定性，无法提供唯一的提示示例；可要求它：

- 在每个代码文件顶部添加简短说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建介绍 Astro 应用结构的 README。

1. 添加一条请求更改的评论，并像提及其他用户一样提及 **@copilot**。可以使用以上建议，也可以提出其他文档改进要求。
2. 选择 **View Session**，查看 Copilot 如何启动新会话进行更新。
3. 选择 **Back to pull request** 可返回 PR。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新的提交。
5. 选择 **Files changed**，审查更新。

可以继续迭代，直到结果符合预期。完成后，可将 PR 从草稿状态设为可审查，然后合并。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

返回 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 审查生成的代码。
5. 审查完成后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准；如有，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

7. 工作流会显示在 PR 检查区域中。确认单个 Astro 应用的项目检查通过；这可能需要几分钟。
8. **可选：**在 Codespace 中切换到 Copilot 创建的分支，手动测试相关游戏功能。打开终端并运行以下命令，将 `<branch-name>` 替换为实际分支名，例如 **copilot/fix-8**：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已实现相关游戏功能。你可以继续通过 PR 评论提出修改，例如调整显示的相关游戏数量，或要求补充文件注释头和 TSDoc 注释（此议题是在更新自定义指令前分配的）。在 **Conversation** 选项卡中添加评论后，Copilot 会启动新会话处理请求。

## 审查无障碍功能

最后审查使用无障碍自定义智能体实现的功能。此 PR 应包含练习 3 中分配的高对比度模式，以及练习 4 中要求添加的浅色模式。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 会带有 **[WIP]** 标记。请等待它完成。

4. 选择 **Files changed** 选项卡并审查代码。
5. 查看实现，尤其要检查：
   - 切换显示模式的 UI 组件。
   - 使用 local storage 保存用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性，例如 ARIA 标签和键盘导航。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查完成后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准；如有，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 确认工作流已加入 PR 检查，并且单个 Astro 应用的项目检查通过；这可能需要几分钟。
9. **可选：**在 Codespace 中切换到 Copilot 创建的分支，手动测试无障碍功能。打开终端并运行以下命令，将 `<branch-name>` 替换为实际分支名：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   然后启动应用，在浏览器中测试高对比度和浅色模式切换开关，并确认设置在页面重新加载后仍然保留。

无障碍自定义智能体会引导 Copilot 遵循无障碍最佳实践。如果发现问题或改进机会，可像处理前面的 PR 一样在评论中提及 **@copilot** 请求更新。

## 可选练习 - 继续委派

云智能体适合处理真实待办事项，让你有空继续其他工作。可以在存储库中创建更多议题并分配给 Copilot，例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页添加分页。
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成云智能体环境的工作坊：

- **检查了存储库附带的自定义指令文件**，以便了解它们对云智能体输出的影响。
- **向 Copilot 云智能体分配了议题**，并观察它如何准备环境、规划并异步执行任务。
- **创建并使用了自定义智能体**处理无障碍任务，添加高对比度和浅色模式切换开关。
- **通过 Copilot agents 页面监控并引导无障碍会话**。
- **审查并迭代了云智能体创建的 PR**，使用 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

云智能体环境的学习已完成。还可以探索其他互补环境：

- 🖥️ **[VS Code 环境](../../vscode/)**：在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 环境](../../cli/)**：从终端交付两项经过审查的更改，使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云端委派功能。

也可以在自己的存储库中继续尝试：

- 将重构或测试覆盖议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈配置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
