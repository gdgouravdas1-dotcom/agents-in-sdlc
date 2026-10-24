---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "探索存储库指令，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应向 Copilot 提供相应指导。[指令文件][instruction-files]不仅说明需要什么代码，也说明代码应如何组织。筛选功能已经构建完成，现在将探索 Copilot 使用的指令、添加文档标准，并将其应用于代码。

在本课中，你将：

- 探索存储库指令和路径范围指令文件如何传递给智能体。
- 更新指令文件，以确保遵循编码标准。
- 查看指令文件对代码的影响。

## 场景

和优秀的开发团队一样，Tailspin Toys 也有一套开发实践准则和要求，其中包括：

- 注释应说明意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 记录用途、参数和返回值；如果有可注入的 `db` 参数，也应记录。
- 可复用的 Astro 组件应记录其 `Props` 契约，并在相关代码变化时同步更新注释。
- 保留现有格式和 lint 指导。

通过指令文件，可以确保 Copilot 获得正确的信息，按这些实践完成任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好，使其更好地理解编码风格与要求。你可以指定首选编码约定、库，甚至希望代码中包含的注释类型。可以为整个存储库创建指令，也可以针对特定文件类型提供任务级上下文。

指令文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。此文件应包含项目级信息，即与大多数聊天或 CLI 请求相关的上下文。
- `.github/instructions/*.instructions.md`：可针对特定语言、文件类型或任务提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 其他指令格式及支持情况因操作环境而异。依赖某种格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 探索项目中的自定义指令文件

初始项目已包含一组指令文件。先查看现有内容，了解指令如何影响后续更改。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览该文件，留意项目简介和编码指导。这些指令适用于与 Copilot 在此存储库中的所有交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意，文件涵盖 Astro、Drizzle 数据层、测试等内容。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它通过 glob 指定这些指令适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他指令文件和项目现有文件的链接。这样可以将大型指令集拆分成较小、可复用的文件，并为 Copilot 提供可参考的示例。

## 更新指令以符合团队准则

现有指令文件是良好的起点，但仍需补充 TypeScript 文档注释要求。更新核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，位置大约在第 35 行。
2. 在该部分最后添加以下内容：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指令

Copilot CLI 会在对话开始时读取存储库指令。保存更新后，退出并恢复筛选功能对话，以便 Copilot 在保留功能上下文的同时读取最新指令。

1. 如果筛选功能对话仍处于打开状态，输入 `/exit` 将其关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选功能对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，查看所有已更改的 TypeScript 文件。留意新添加的 TSDoc 注释，并确认注释准确说明了代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何读取指令文件中的上下文，并将新标准应用于功能。具体而言，你：

- 探索了存储库指令和路径范围指令文件如何传递给智能体。
- 更新了指令文件，以确保遵循编码标准。
- 查看了指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持参考][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
