---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并运行测试。开发人员往往需要找到所有相关文件、实施更改、运行测试、调试并补充遗漏内容。Copilot 智能体模式可以帮助处理这些步骤。

智能体模式可在 IDE 中更自主地工作：先探索项目结构，再修改相关文件、运行测试，并自动修复发现的问题。下面使用智能体模式为网站添加功能。

> [!NOTE]
> 智能体模式和云智能体虽然名称相似，但适用于不同的工作方式。智能体模式在 IDE 中运行，可快速反馈并与开发人员交互；云智能体则异步工作，像团队成员一样通过议题和拉取请求协作。

在本练习中，你将了解 Copilot 智能体模式如何：

- 探索项目并找到相关文件，协调多个文件的更改。
- 在 UI 和数据层实现功能。
- 在代码合并前生成并审查更改和测试。

## 场景

随着游戏列表变长，你希望用户可以按类别和发行商筛选。上一项练习已创建 publishers helper，现在将使用 Copilot 智能体模式完成其余数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

先浏览 Tailspin Toys，了解网站当前的功能。它是一个开发者主题的桌游众筹平台，允许用户浏览游戏列表和详情。网站是单个 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有独立的后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`；交互功能通过使用标准 DOM API 的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 命令来启动 Astro 开发服务器。

1. 返回 Codespace。上一项练习结束后应位于 `main` 分支。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）在 Codespace 中打开新终端。
3. 创建筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似下面的地址：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现提示，询问是否在浏览器中打开 `http://localhost:4321`，请选择 **x** 关闭对话框。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），同时选择终端中的 Astro 地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择 localhost 链接时，系统会自动将其转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 中网站的专用隧道。

### 浏览网站

网站运行后，可以探索主要功能：

- **主页**：显示桌游的标题、图像和说明。
- **游戏详情页**：选择游戏后显示其标题、说明、发行商和类别等详情。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新的对话。不同练习使用的模式和模型可能不同；相关步骤会注明需要使用的选项。

项目初始版本已可运行，但还可以添加更多功能。先查看从模板创建项目时自动生成的待办议题。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。该智能体可以自主规划和修改多个文件、运行终端命令并调用工具。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并未偏好某个模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此在这里列出，希望生成的代码建议相对一致。不过，LLM 具有概率性，实际建议可能与示例不同，这是正常现象。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，查看存储库中的议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看 Copilot 生成的议题列表和优先级。

## 查看指令文件

生成代码前，先查看将为 Copilot 提供工作上下文的指令文件。[用户界面 (UI)][ui-instructions]文件说明了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，包括应用架构、组件设计原则、可测试性和无障碍，以及针对 Astro、Tailwind CSS 和 Drizzle 的指令链接。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。这些路径相对于指令文件所在位置，便于重用指导、拆分复杂内容，并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要三项更改：

- 在 `src/lib/` 数据层添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

所有测试都必须通过，才能合并到代码库。Copilot 智能体模式可以执行这些任务。

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，再选择 `ui` 指令文件。

   ![显示如何选择 UI 指令文件的屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单仍选择 **Agent**。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认当前模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，请 Copilot 根据待办议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 查找相关数据层 helper、UI 和测试文件并进行修改。

   ![显示 Copilot 正在探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> 探索项目、修改文件和运行测试可能需要几分钟。过程中，编辑器可能会显示 **Keep** 和 **Undo**。Copilot 完成后，每项更改都会显示这两个选项，无需在它工作时提前选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![显示 Copilot Chat 窗格中确认运行测试的对话框](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停或出现测试失败。Copilot 会在生成代码和运行测试之间迭代，直到任务完成且不再发现错误。
9. 检查生成的代码是否存在问题。

   ![显示 Copilot 智能体模式完整 Chat 会话的屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

> [!CAUTION]
> 始终审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行的网站，探索新功能。
11. 确认功能正常且代码审查无误后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 添加功能，包括：

- 在 UI 和数据层实现新功能。
- 探索项目、找到相关文件并协调更改。
- 在合并前审查生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]，并创建功能 PR。

### 额外练习 - 实现分页

游戏列表增长后可能需要分页。使用本练习学到的技能，请 Copilot 为网站添加分页。考虑是否允许用户选择每页大小、还是使用固定值；沿用现有指令和最佳实践，并在需要时迭代提示和提供上下文。

## 资源

- [Copilot ask、edit 和 agent 模式：功能及使用时机][choose-mode]
- [VS Code 智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
