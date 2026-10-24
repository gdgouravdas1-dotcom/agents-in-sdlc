---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义设置和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到现有的筛选议题。
- 添加了星级评分，在转发的浏览器中审查结果，并手动合并第一个拉取请求 (PR)。
- 从筛选议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中实现，并在 Interactive 模式中审查。
- 使用自定义指令引导智能体，自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加了 Playwright 模型上下文协议 (MCP) 服务器，并用它在真实浏览器中探索筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，评估需求、覆盖范围、技能检查结果和浏览器证据。
- 审查了完整的筛选功能更改，并授权 Agent Merge 处理筛选 PR。
- 探索了用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 交付内容

本工作坊包含两个 PR 里程碑，每个里程碑都基于更新后的 `main` 单独创建分支：

1. **星级评分：**在游戏卡片中显示现有的 `starRating` 和明确的未评分状态。
2. **筛选功能和质量工作流：**实现筛选功能、更新指令并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并添加相关测试。

从规划筛选功能到创建 PR，你始终使用同一对话和分支。为简化工作坊流程，这些更改都放在一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 将这些结果与需求和测试覆盖审查结合起来，为 PR 做准备。

新增测试应弥补真正的覆盖缺口；如果 QA 运行后确认无需新增测试，也可能是正确结果。授权合并前，先审查代码和证据；更改后要重新收集受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，你更新了指令、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。可以在不同对话中复用这些自定义设置，并根据团队需求进行调整。指令用于设定标准，技能用于描述可重复任务，自定义智能体用于定义专业角色，MCP 服务器则用于连接外部工具。审查实际更改和工具结果，不要只看智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在开始构建前梳理方案，使用 **Interactive** 处理需要密切参与的更改，使用 **Autopilot** 处理范围明确的任务。常规编辑可选择速度更快的模型，复杂工作则可选择能力更强的模型。

上下文与基础设施同样重要。清楚说明想构建**什么**、**为什么**构建以及**如何**构建，会显著影响结果。

## 更多探索

核心工作流已完成。还可以进一步了解以下 CLI 功能：

- 使用 `/review` 请代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并听取不同视角。
- 使用 `/fleet` 并行处理彼此独立的子任务。
- 使用 `/worktree` 隔离单独的任务。
- 使用 `/delegate` 将任务交给 Copilot 云智能体。

## 后续步骤

提升任何工具使用能力的最好方式，就是持续使用它。可以将其用于生产代码、个人项目，或那个想了多年却一直没开始实现的小应用。与团队分享经验，也向团队学习，并继续查阅文档。

如果想从终端继续扩展 Tailspin Toys，可以学习可选的 [Foundry Backer Concierge 系列课程][foundry]。也可以探索其他环境的工作坊：[VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot 云智能体工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
