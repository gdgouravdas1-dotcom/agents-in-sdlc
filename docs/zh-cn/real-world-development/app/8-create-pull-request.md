---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

筛选功能实现、指令更新、技能更新、质量保证 (QA) 配置文件和测试都保存在同一分支上。现在一并审查这些内容并创建拉取请求。你已自行合并星级评分拉取请求 (PR)；这次将让 **Agent Merge** 管理流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 智能体拆分为几个 PR。为简化工作坊流程，整个筛选和质量工作流都保留在同一个会话和分支中，并将所有更改纳入此 PR。

在本课中，你将：

- 了解 Agent Merge 及其如何自动执行合并流程。
- 检查完整的功能 PR 和验证证据。
- 审查后再授权 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选工作流中，你使用 Copilot 规划、实现并验证功能。现在，Tailspin Toys 希望自动执行剩余的 PR 工作，同时让开发人员掌控合并授权。

## Agent Merge 简介

**Agent Merge** 会自动处理拉取请求合并前的剩余工作。在 GitHub Copilot app 中启用后，会话会读取 PR 并处理阻塞事项，包括修复持续集成 (CI) 检查失败、回应审查意见，以及在需要时进行变基。GitHub 允许后，它会合并 PR。此功能在后台运行，应用重启后仍会继续，并会在 PR 合并后自动关闭。

此前你一直自行选择 **Merge pull request**。Agent Merge 可以接手这项工作，但仍需明确授权它编辑代码和合并。授予合并权限前，请审查允许的操作和已完成的工作。

## 使用 Agent Merge 管理 PR

所有代码都已创建并审查完成，现在让 Agent Merge 管理 PR 流程。

1. 使用智能体选择器选择 **Default agent**。
2. 选择 **Create PR** 旁的下拉菜单。
3. 选择 **Agent Merge**。按钮会更改为 **Agent Merge**。
4. 选择 **Agent Merge** 按钮，启动该流程。仅选择下拉菜单中的选项并不会启动流程。

Agent Merge 会：

- 创建带有标题和说明的拉取请求。
- 如果会话从议题开始，则在 PR 说明正文中引用相关议题。
- 对目标分支执行变基，或处理潜在的合并冲突。
- 监视 CI 流程，确保所有检查通过。
- 监视其他开发人员或 Copilot 代码审查在 PR 中提交的反馈，并更新代码以解决意见。
- 可选择在所有步骤完成后自动合并 PR。

接下来启用自动合并。

5. 授予合并权限前，审查 PR 差异和验证证据。
6. 选择 **Agent Merge** 旁的下拉菜单。
7. 确认 **Merge pull request** 旁出现勾选标记。
8. 明确要求智能体合并：

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 跟进处理进度并解决任何阻塞项。继续前确认 PR 状态为 **Merged**；仅仅启用合并权限或看到智能体正在运行，并不能证明 PR 已完成合并。

> [!IMPORTANT]
> Agent Merge 不会绕过存储库保护或权限限制。继续前先解决这些阻塞项。

## 总结和后续步骤

你已自动执行开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。本课中，你：

- 了解了 Agent Merge 及其如何自动执行合并流程。
- 检查了完整的功能 PR 和验证证据。
- 仅在审查后授权 Agent Merge，并确认 PR 已合并。

接下来，[使用现有画布并创建分类画布][next-lesson]，探索如何与智能体一起检查、规划和可视化工作。

## 资源

- [使用 GitHub Copilot app 管理议题和拉取请求][managing-issues-prs]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
