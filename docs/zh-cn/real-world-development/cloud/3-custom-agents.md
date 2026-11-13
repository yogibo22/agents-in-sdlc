---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中的特定任务或领域创建的专用人工智能 (AI) 助手。可以在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，为 Copilot 提供具体指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识编写成可复用的智能体，例如遵循 [Web 内容无障碍指南 (WCAG)][wcag] 规范的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体可通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局目录 `~/.copilot/agents` 中定义。每个文件都包含 YAML（YAML Ain't Markup Language）frontmatter，至少需要 `name` 和 `description` 字段，随后是用于定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体与智能体技能

自定义智能体和[智能体技能][agent-skills-concept]在概念上有些重叠。两者主要通过 Markdown 文件定义，用于指导 AI 执行任务。简单来说，**自定义智能体负责工作，技能则是工具**。

自定义智能体有自己的上下文窗口，能够在工作时协调技能甚至其他智能体。本实验中的无障碍自定义智能体会根据无障碍指南审查并更新网站；在执行过程中，它可以调用拉取请求工作流技能或用于运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。与 AI 的其他用法一样，需要通过测试和迭代，找到适合环境和场景的方案。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

自定义智能体将帮助你了解：

- 如何定义自定义智能体。
- 如何将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，当前暗色主题的文字与背景对比度不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

由于无障碍至关重要，你希望尽快实现此功能，因此将使用自定义智能体生成相关功能。

## 查看无障碍自定义智能体

项目已为无障碍工作创建了自定义智能体。先查看其内容，了解它如何指导 Copilot。

返回 Codespace，并查看无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 继续浏览后续部分，查看其中列出的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - 超文本标记语言 (HTML)、层叠样式表 (CSS) 和 JavaScript 代码示例。
   - 常见问题和错误。

## 创建并分配议题

“Mission control”是管理环境中所有智能体的中心位置。可以在这里将任务分配给 Copilot 云智能体、监控任务，甚至重定向会话并提供更多指导。现在将创建高对比度模式的任务并分配给 Copilot。

1. 打开自己的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

    ![Copilot 云智能体分配界面，其中突出显示了自定义智能体和 Accessibility](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，它们是针对特定任务和领域定制的 AI 助手。借助自定义智能体，可以将团队专业知识和标准编写为可复用智能体，从而指导 Copilot 更有效地完成特定类型的工作。

本课介绍了以下概念：

- 如何定义自定义智能体。
- 如何将任务分配给自定义智能体。

Copilot 正在实现高对比度模式。接下来转到“Mission control”，[监控并指导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为在组织中使用自定义智能体做好准备][org-custom-agents]
- [为在企业中使用自定义智能体做好准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
