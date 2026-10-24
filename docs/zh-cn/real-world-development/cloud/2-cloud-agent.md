---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

许多组织都面临技术债务，例如未解决的安全问题、需要更新的旧代码，或长期留在待办列表中的功能请求。GitHub Copilot 云智能体可以自主更新代码和添加功能。任务完成后，它会创建草稿拉取请求 (PR)，供开发人员审查。这样可以把耗时任务交给智能体异步处理，让开发人员专注于更重要的工作。

本练习将介绍如何：

- 自定义代码生成环境。
- 确保操作以安全的方式执行。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初的网站开发承包商没有为代码添加文档；团队希望先为应用中所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改善游戏发现体验：在每款游戏的详情页显示同类别的其他游戏。当前只需一个可供验收测试的初步实现。此功能目前被其他优先级更高的议题阻塞，因此适合交给云智能体异步处理。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。你可以通过[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)启动任务。Copilot 会创建草稿 PR 来跟踪进度、准备环境并开始工作。你可以在会话运行期间或结束后查看详情；解决方案准备好审查时，Copilot 会在 PR 中提及你。

## 编写范围明确的指令

GitHub Copilot 不会代替思考。即使看似简单的操作也可能有复杂细节，因此应遵循[云智能体任务范围最佳实践][cloud-agent-best-practices]。像使用 AI 结对程序员一样分阶段工作、不断学习和调整；生成式 AI 不会改变软件开发的基本原则。

## 查看此存储库中的自定义指令

前面已介绍过自定义指令。如果还没有学习指令文件，现在可先只读查看项目中已有的文件，稍后审查云智能体 PR 时再观察它们的影响。

- 打开 `.github/copilot-instructions.md`，查看 **Code standards** 部分，特别是 TypeScript 约定和 TSDoc 文档注释要求。
- 打开 `.github/instructions/unit-tests.instructions.md`，查看将指令限定于 `**/*.test.ts` 文件的 `applyTo` frontmatter。

下一部分将 `Code lacks documentation` 议题分配给云智能体。审查 PR 时，请留意这些指令如何影响 TSDoc 注释、TypeScript 约定和文件注释头。

## 配置云智能体的开发环境

编写代码通常需要特定环境和设置脚本；将任务分配给 Copilot 时也是如此。云智能体使用 [GitHub Actions][github-actions]准备工作环境。可以在 `.github/workflows/copilot-setup-steps.yml` 中配置[专用设置工作流][setup-workflow]，提前安装所需工具和依赖项。项目已预先配置此工作流，其中包含 Node.js、项目依赖、端到端测试所需的浏览器，以及迁移并初始化数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许从存储库的 Actions 选项卡手动测试这些设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 只授予设置步骤所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖），需要 contents: read 权限。如果不在设置步骤中克隆，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端和应用设置：Node.js（整个应用现在基于 Astro 和 Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并初始化本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

这与其他 GitHub 工作流类似，但有几个重点：

- 工作流包含一个名为 `copilot-setup-steps` 的作业；Copilot 开始处理 PR 前会先运行它。
- `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流，无需等待 Copilot 启动任务。

## 为代码添加文档

过时或缺失的文档会降低效率，使代码库更难维护，也让新成员更难上手。Copilot 擅长生成文档，因此可以将此任务交给云智能体，稍后再审查结果。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡，再选择 **New issue**。
3. 选择 **Blank issue**。
4. 将 **Title** 设置为 `Code lacks documentation`。
5. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

6. 选择 **Create** 创建议题。
7. 在右侧选择 **Assign to Copilot** 打开分配对话框。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

8. 选择 **Assign**。

   ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

9. 选择 **Pull Requests** 选项卡并打开新创建的 PR。标题可能类似 `[WIP]: Code lacks documentation`。如果列表中还没有 PR，请稍等片刻并刷新页面。
10. 几分钟后，Copilot 应会在 PR 中创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项。它需要先运行前面介绍的工作流、准备环境、分析项目并制定解决方案。

11. 审查它计划完成的任务。
12. 在 PR 时间线中查看 Copilot 已开始处理议题的更新，然后选择 **View session**。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的指示器。

13. 浏览实时会话，查看 Copilot 如何探索代码、了解项目状态、思考计划并编写代码。

这可能需要几分钟。云智能体的优势之一是可以异步执行任务，让你处理其他工作。接下来，在 Copilot 处理该任务时，将另一个任务也分配给它。

## 在游戏详情页添加相关游戏

云智能体可以分担不同任务，让你同时处理其他工作。现在将设计团队提出的相关游戏功能分配给 Copilot。

1. 返回 github.com 上的存储库，选择 **Issues**，然后选择 **New issue**。
2. 选择 **Blank issue**。
3. 将 **Title** 设置为 `Show related games on the game details page`。
4. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

5. 选择 **Create** 创建议题。
6. 在右侧选择 **Assign to Copilot**，然后选择 **Assign**。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

稍后，议题的第一条评论旁会出现 👀，表示 Copilot 已开始处理。

![Copilot 使用 eyes 表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在后台处理第二个任务。云智能体的工作方式类似软件工程师，无需一直监控；可以等任务完成后再审查。接下来探索如何创建和使用自定义智能体。

## 总结和后续步骤

本课探索了 [GitHub Copilot 云智能体][copilot-agents]。你可以将议题分配给 Copilot，让它异步处理技术债务、新功能或框架迁移任务。

本课介绍了：

- 自定义代码生成环境。
- 确保操作以安全的方式执行。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

Copilot 正在后台工作，接下来创建并使用自定义智能体。云智能体也可以使用 [MCP 服务器][cloud-agent-mcp]，并读取前面模块介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
