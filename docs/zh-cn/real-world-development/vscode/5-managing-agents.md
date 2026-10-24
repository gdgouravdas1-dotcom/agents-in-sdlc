---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用 GitHub Copilot 智能体模式时，它会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作会实时在本机进行，VS Code 中的 Copilot Chat 会显示每次工具调用和文件编辑。你可以直接查看差异、接受或拒绝单项更改，并通过后续提示引导对话，全程无需离开编辑器。

在本练习中，你将：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话中途引导智能体添加浅色模式切换开关。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。开发过程中，设计团队提出了额外需求：为觉得默认主题太暗的用户提供浅色模式切换开关。无需重新开始对话，可以继续引导当前会话完成这项工作。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括读取的文件、运行的命令和建议的编辑。每项文件编辑都会在 Chat 中显示差异以及 **Keep** 和 **Undo** 操作。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 智能体工作时，在 Chat 中滚动查看每项工具调用，包括文件读取、文件编辑和请求运行的终端命令。
3. 智能体建议编辑文件时，差异会直接显示在对话中：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体再次尝试。
4. 也可以打开 Chat 顶部的 **Files changed** 指示器，查看该会话修改的所有文件。

> [!NOTE]
> 如果工具调用需要权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式，并查看它对更改的总结。

## 引导智能体添加浅色模式

高对比度模式完成后，在同一对话中添加浅色模式切换开关。继续使用同一个会话，智能体会保留此前工作涉及的组件、样式和本地存储模式等上下文。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会检查已有更改并提出浅色模式的更新。
3. 像处理高对比度模式一样，逐项审查新编辑，并使用 **Keep** 或 **Undo**。
4. 如果智能体的方向不符合预期（例如不希望它重构切换开关 UI），可以使用 Chat 输入框旁的 **Stop** 按钮停止工作，再发送澄清提示。

> [!TIP]
> 在进行中的会话里继续引导通常比重新开始更快。可以通过后续提示调整实现，例如简化 CSS、使用 CSS 变量、将浅色模式集成到现有设置面板，或为两个开关添加键盘快捷键。

## 审查工作集

提交更改前，快速审查智能体修改的所有文件。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看所有已更改文件。应包含 Astro 组件、样式和相关测试。
3. 打开部分文件并检查差异，确认包含自定义智能体建议的无障碍实践，例如 ARIA 属性、键盘导航、语义化 HTML，以及通过 local storage 保留设置。
4. 确认更改后，在 Source Control 面板中暂存并提交。下一项练习会发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 智能体模式监控并引导了正在进行的会话：逐项审查文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留上下文。

本练习介绍了：

- 实时监控智能体工作。
- 接受或拒绝单项文件编辑。
- 使用后续提示在会话中途引导智能体。

接下来，在[下一项练习][next-lesson]中发布无障碍更改为拉取请求，并审查本实验中本地自定义智能体的工作。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
