---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 应了解某些背景信息，就需要确保这些上下文可供它使用。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[指令文件][instruction-files]指导代码生成。

在本练习中，你将：

- 探索项目专属上下文、编码准则和文档标准如何通过存储库自定义指令及路径范围指令文件传递给 Copilot。
- 在当前指令的基础上生成筛选功能的第一块数据切片（publishers helper）。
- 向 `.github/copilot-instructions.md` 添加一项新的全库标准。
- 再次运行相同提示，观察生成的代码如何遵循新标准。
- 将指令更新和筛选功能代码提交到 `main`，供下一项练习使用。

> [!CAUTION]
> 生成的代码可能不会完全遵循某些标准。Copilot 具有非确定性。本练习旨在观察更新指令后行为变化的趋势，而不是要求输出逐字符一致。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践准则和要求，其中包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式，并具有现代感。
- 代码应使用 TSDoc 文档注释。
- 每个文件顶部都应添加说明文件用途的注释块。

使用指令文件可让 Copilot 获取这些要求，并据此完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，使它更好地理解编码风格和要求。你可以指定编码约定、库，以及希望包含的注释类型。可以为整个存储库创建指令，也可以针对特定文件类型提供任务级上下文。

指令文件分为两类：

- `.github/copilot-instructions.md`：针对该存储库的每次请求都会发送给 Copilot 的单个指令文件。它应包含项目级信息，例如技术栈、项目概述、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建，用于提供 TypeScript 或 Astro 等语言的指导，也可用于 UI 组件或单元测试等任务。

> [!NOTE]
> 在 IDE 中工作时，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一次编辑建议。Copilot Chat、Copilot CLI 和 Copilot cloud agent 生成代码时都会使用存储库级指令以及带 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件格式，例如 `AGENTS.md` 和 `CLAUDE.md`。请查看[自定义指令支持参考][custom-instructions-support]。

### 管理指令文件的最佳实践

示例项目展示了一种有代表性的做法：

- 将 `copilot-instructions.md` 中的内容聚焦于项目级指导，例如项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为单元测试、Astro 组件、数据层等文件类型或特定任务提供详细指导。
- 使用自然语言清晰说明要求，并提供代码应该如何编写或不应如何编写的示例。

指令文件没有唯一的正确写法。通过尝试，可以找到适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一组完善的指令文件。项目中包含面向 [UI 更新][ui-instructions]和 [Astro][astro-instructions]等任务的示例。Copilot 也可以帮助生成指令文件，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。还可以从 [awesome-copilot][awesome-copilot] 获取模板和起点。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

先阅读存储库附带的核心 `copilot-instructions.md` 和各类 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中查看。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 指导，这些内容适用于与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览，其中包含 Astro、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置相对于存储库根目录的 glob，决定哪些文件适用。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件会使用这些指令。
5. 查看其中为本项目编写单元测试的专门要求。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意它链接到了其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件。将大型指令集拆分为较小、可复用的文件，并向 Copilot 提供示例，有助于它生成代码。（这些路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 为更改创建分支

先按最佳实践创建分支，再开始修改代码。

1. 返回上一项练习中的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

先在现有指令生效时发送提示，然后更新指令并再次发送相同提示，以观察差异。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的对话。不同练习的模式和模型可能不同；相关步骤会注明需要使用的选项。

1. 关闭之前练习打开的编辑器标签页，让 Copilot 只读取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 的位置。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，使 Copilot 可以修改文件。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更改，通常会修改 helper 文件和测试。
6. 注意建议的 helper 是一个类型化函数，第一个参数是 `db` 客户端，返回类型化的 publishers 数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，即使没有明确要求也可能添加文档注释。如果发生这种情况也没关系；重点仍是观察新增指令对输出一致性的影响。

## 添加新的全库标准

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在补充存储库编码标准，改善代码建议质量。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。该部分记录了项目编码标准，但尚未包含代码文档规则，这就是 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令文件可以放在存储库级文件 `.github/copilot-instructions.md` 中提供全局指导，也可以放在 `*.instructions.md` 文件中，为特定语言、文件类型或任务提供指导。刚添加的文档注释规则属于项目级标准，适合放在存储库级文件中。

## 再次运行提示并观察变化

更新指令后，请 Copilot CLI 按照新规则更新刚刚生成的 publishers 文件。

1. 在 Copilot Chat 中选择 **New Chat**，清除上下文。
2. 重新打开 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 再次发送**相同提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意建议文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意建议的函数现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新标准，并将其应用于后续练习会继续使用的代码。

## 提交并推送分支

指令文件与其他存储库文件一样，应通过版本控制管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 确认指令和 helper 更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新及 Copilot 为 helper 基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求并关联待办事项中的指令更新议题。Copilot 可以通过已连接的 GitHub 工具查找议题、创建 PR 并合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已配置 GitHub 工具，无需额外设置；Copilot 首次使用工具时，VS Code 可能会提示登录 GitHub。按照提示授权即可。后续练习会介绍此连接如何通过模型上下文协议 (MCP) 工作。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中已选择 **Agent**。
3. 请 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明 PR 会关闭该议题，也包含 publishers helper 基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. Copilot 请求允许代表你执行 GitHub 操作时，先审查命令，再根据需要选择 **Allow**。
6. PR 创建后，请 Copilot 合并 PR 并将本地分支切回 `main`，然后拉取最新代码：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求执行 GitHub 操作和 shell 命令的权限时，审查命令并按需选择 **Allow**。

现在，你已在 GitHub Copilot 的帮助下创建并合并了 PR。

## 总结和后续步骤

你已了解 Copilot 如何从项目指令文件获取上下文，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，观察 Copilot 在现有指令下的输出。
- 向 `.github/copilot-instructions.md` 添加新的全库标准。
- 再次发送相同提示，观察生成的代码如何遵循新标准。
- 将指令更新和 helper 基础提交到 `main`，供下一项练习使用。

接下来，Copilot 将在[智能体模式][next-lesson]中使用这些指令，为整个代码库添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
