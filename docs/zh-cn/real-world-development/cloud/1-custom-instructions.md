---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 应了解某些背景信息，就需要确保这些上下文可供它使用。[指令文件][instruction-files]可提供指导，帮助 Copilot 理解不仅要做什么，也要如何完成。

在本练习中，你将：

- 探索项目专属上下文、编码准则和文档标准如何通过存储库自定义指令及路径范围指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的全库标准。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，本环境不会通过前后对照的提示来演示效果：Copilot 云智能体会异步处理 GitHub 议题，因此难以实时比较。后续审查云智能体创建的 PR 时，可以看到这些指令的影响。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践准则和要求，其中包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式，并具有现代感。
- 代码应使用 TSDoc 文档注释。
- 每个文件顶部都应添加说明文件用途的注释块。

通过指令文件，可以确保 Copilot 获取这些要求，并据此完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，使其更好地理解编码风格和要求。你可以指定首选编码约定、库，以及希望包含的注释类型。可以为整个存储库创建指令，也可以针对特定文件类型提供任务级上下文。

指令文件分为两类：

- `.github/copilot-instructions.md`：每次针对存储库的请求都会发送给 Copilot 的单个指令文件。它应包含项目级信息，例如技术栈、项目概述、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建，用于提供 TypeScript 或 Astro 等语言的准则，也可用于 UI 组件或单元测试等任务。

> [!NOTE]
> 在 IDE 中工作时，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一次编辑建议。Copilot Chat、Copilot CLI 和 Copilot 云智能体生成代码时都会使用存储库级指令以及带 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件，例如 `AGENTS.md` 和 `CLAUDE.md`。请查看[自定义指令支持参考][custom-instructions-support]。

### 管理指令文件的最佳实践

项目示例展示了一种有代表性的做法：

- 将 `copilot-instructions.md` 中的内容聚焦于项目级指导，例如项目简介、结构和全局编码标准。
- 使用 `*.instructions.md` 为单元测试、Astro 组件、数据层等文件类型或特定任务提供详细指导。
- 使用自然语言清晰说明要求，并提供代码应该如何编写或不应如何编写的示例。

指令文件没有唯一的正确写法。通过尝试，可以找到适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有完善的指令文件。项目中包含面向 [UI 更新][ui-instructions]和 [Astro][astro-instructions]等任务的指令。Copilot 也可以帮助生成指令文件，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。还可以在 [awesome-copilot][awesome-copilot] 中寻找模板和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

先阅读存储库附带的核心 `copilot-instructions.md` 和各类 `*.instructions.md` 文件。可以在 GitHub Web UI 或 Codespace 中查看。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 指导，这些内容适用于与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览，其中包含 Astro、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它设置了相对于存储库根目录的 glob，决定哪些文件适用。例如匹配 `**/*.test.ts` 的 TypeScript 测试文件会使用这些指令。
5. 查看其中为本项目创建单元测试的专门指导。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意它链接到了其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件。将较大的指令集拆分成较小、可复用的文件，并为 Copilot 提供示例。（这些路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 添加新的全库标准

这是本练习中唯一需要编辑的部分：添加项目级规则，要求使用 TSDoc 文档注释和文件级注释头。后续练习中云智能体处理议题时会读取这项标准。

先创建分支。下一项练习中，云智能体会读取议题目标分支上的指令，因此提交到分支后还要将更改合并到 `main`。

1. 在 Codespace 终端中创建并切换到分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. 重新打开 `.github/copilot-instructions.md`。
3. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。该部分记录了项目编码标准，但尚未包含代码文档规则。
4. 在现有标准下方添加以下 Markdown，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令文件可以放在存储库级文件 `.github/copilot-instructions.md` 中提供全局指导，也可以放在 `*.instructions.md` 文件中，为特定语言、文件类型或任务提供指导。刚添加的文档注释规则属于项目级标准，适合放在存储库级文件中。

## 提交、推送并合并指令更新

下一项练习中，云智能体会从议题目标分支创建工作分支，因此必须先将指令更新合并到 `main`，云智能体才能读取它。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。下一项练习中云智能体处理议题时，会读取这些指令。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main` 分支，也可以跳过分支步骤并直接提交到 `main`。创建分支是为了模拟实际项目中的工作方式。

## 总结和后续步骤

你已探索 Copilot 如何从项目指令文件获取上下文，并向 `.github/copilot-instructions.md` 添加新的全库标准。后续练习中，你会看到该标准如何影响云智能体创建的 PR。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
