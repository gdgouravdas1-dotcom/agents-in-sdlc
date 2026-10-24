---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证 (QA) 配置文件和测试都保存在同一分支上。现在一并审查这些内容并创建拉取请求。你已自行合并星级评分拉取请求 (PR)；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 智能体拆分到几个独立 PR 中。为简化工作坊流程，筛选和质量工作流一直保留在同一个对话和分支中，因此这些更改都纳入此 PR。

在本课中，你将：

- 了解 Agent Merge 及其如何自动执行合并流程。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

整个筛选工作流中，你使用 Copilot 规划、实现并验证功能。现在，Tailspin Toys 希望自动执行剩余的 PR 工作，同时由开发人员掌控合并授权。

## Agent Merge 简介

**Agent Merge** 会自动处理合并拉取请求前的剩余工作。启用后，Copilot 会处理阻塞 PR 的事项，包括修复持续集成 (CI) 检查失败、回复审查意见，以及在需要时变基。存储库允许时，它会启用 GitHub 自动合并。

此前你一直自行选择 **Merge pull request**。Agent Merge 可以接手这部分工作，但启用前仍应先审查并确认更改已准备就绪。

## 使用 Agent Merge 管理 PR

代码完成后，一并审查所有更改，创建 PR，再让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建新 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 将监视 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示了类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的批准、分支保护、合并队列、存储库设置或权限限制。如果流程受阻，请查看原因；在存储库允许的情况下，完成审查后手动合并。

## 总结和后续步骤

你已自动执行开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。本课中，你：

- 了解了 Agent Merge 及其如何自动执行合并流程。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，[探索 Copilot CLI 中用于管理上下文、模型、共享和可选云端委派的实用斜杠命令][next-lesson]。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
