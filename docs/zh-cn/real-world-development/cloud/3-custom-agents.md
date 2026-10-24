---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的 AI 助手。可以在存储库的 `.github/agents` 文件夹中用 Markdown 文件定义智能体，为 Copilot 提供专门指令、最佳实践、编码模式和领域知识。团队可以将经验转化为可复用智能体，例如负责检查 [WCAG][wcag] 标准的无障碍智能体、遵循安全编码实践的安全智能体，或维护测试模式的测试智能体。

自定义智能体也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少有 `name` 和 `description` 字段，后面是定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体和智能体技能

自定义智能体和[智能体技能][agent-skills-concept]都通过 Markdown 提供指导。可以将自定义智能体理解为**执行工作的角色**，将技能理解为**工具**。

自定义智能体有自己的上下文窗口，可以在工作中编排技能，甚至调用其他智能体。本练习中的无障碍智能体会根据无障碍准则审查并更新网站，也可以调用 PR 工作流或运行和管理测试的技能。

> [!NOTE]
> 自定义智能体没有唯一的正确写法。应通过测试和迭代找到适合环境和场景的方案。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

自定义智能体练习将介绍：

- 如何定义自定义智能体。
- 如何将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用众筹平台，不论其视觉能力或偏好如何。近期反馈显示，部分用户认为当前深色主题难以阅读，因为文字和背景颜色对比度不足。设计团队希望添加可切换的高对比度模式。

无障碍功能十分重要，因此使用自定义智能体尽快生成此功能。

## 检查无障碍自定义智能体

项目已为无障碍工作创建自定义智能体。先查看它如何引导 Copilot。

1. 打开 Codespace 并查看 `.github/agents/accessibility.md`。
2. 注意 YAML frontmatter 中的 `name` 和 `description` 字段。

> [!CAUTION]
> 自定义智能体的 frontmatter 必须包含 `name` 和 `description`。

3. 浏览后续部分，查看无障碍网站代码生成的核心职责、最佳实践、HTML/CSS/JavaScript 示例，以及常见问题和错误。

## 创建并分配议题

GitHub 上的 agents 页面是管理智能体任务的中心位置。你可以在那里分配任务、监控进度、重定向任务并提供额外指导。现在将高对比度模式任务分配给 Copilot。

1. 打开存储库并选择 **Issues**。
2. 选择 **New issue**，再选择 **Blank issue**。
3. 将 **Title** 设置为 `Add high contrast mode to website`。
4. 将 **Description** 设置为：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

5. 选择 **Create** 创建议题。
6. 在右侧选择 **Assign to Copilot**，打开分配对话框。
7. 从自定义智能体列表中选择 **Accessibility agent**。

   ![云智能体分配屏幕截图，突出显示自定义智能体和 Accessibility](../../../_images/ex5-select-custom-agent.png)

8. 选择 **Assign**。Copilot 会在后台开始处理任务。

## 总结和后续步骤

本课探索了 GitHub Copilot 中的[自定义智能体][custom-agents]。自定义智能体是针对特定任务和领域定制的 AI 助手，可将团队经验和标准转化为可复用指导。

本课介绍了：

- 如何定义自定义智能体。
- 如何将任务分配给自定义智能体。

Copilot 正在实现高对比度模式，接下来在 GitHub 的 agents 页面中[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做准备][org-custom-agents]
- [为企业使用自定义智能体做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
