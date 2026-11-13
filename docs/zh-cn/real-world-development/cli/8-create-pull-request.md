---
title: "第 8 课 - 创建并合并功能拉取请求 (PR)"
description: "一起审查筛选功能、指令、技能更新、质量保证 (QA) 配置文件和测试，然后创建拉取请求 (PR) 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证 (QA) 配置文件和测试都保存在同一分支上。现在一起审查这些更改并创建拉取请求 (PR)。星级评分 PR 是你手动合并的；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 实际项目中，通常会将功能、指令更新、技能更新和 QA 智能体拆分到几个独立的 PR 中。为简化研讨会流程，筛选功能和质量工作流一直使用同一对话和分支，并将在此 PR 中一起提交。

在本课中，你将：

- 了解 Agent Merge 及其如何自动处理合并流程。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能工作流中，你使用 Copilot 规划、实现并验证了功能。现在 Tailspin Toys 希望自动处理 PR 的剩余工作，同时让开发人员掌握合并授权权。

## 了解 Agent Merge

**Agent Merge** 会自动处理将拉取请求合并所需的剩余工作。启用后，Copilot 会解决 PR 当前的阻碍，例如修复持续集成 (CI) 检查失败、回应审查评论，以及在需要时变基；之后，如果存储库设置允许，还会启用 GitHub 自动合并。

之前都是由你选择 **Merge pull request**。Agent Merge 可以代为处理，但启用前仍须审查工作并确认 PR 已准备就绪。

## 使用 Agent Merge 管理 PR

代码已全部完成，现在一起审查更改、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建新的 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示了类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限限制。如果操作受阻，请查看报告的原因，并在存储库允许时手动完成已审查的合并。

## 总结和后续步骤

你已自动化开发流程中的多个部分，包括生成代码、测试和验证代码，以及处理拉取请求。在本课中，你：

- 了解了 Agent Merge 及其如何自动处理合并流程。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型、共享以及可选的云委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
