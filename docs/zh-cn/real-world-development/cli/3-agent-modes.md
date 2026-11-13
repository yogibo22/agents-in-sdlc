---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先在项目中添加了一个小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 能融入组织现有的工作流，帮助团队以正确的方式构建正确的功能。本课将开始一组课程，带你体验典型的智能体开发流程：从议题生成新功能，确保代码有效、功能符合预期，最后成功合并到项目中。

> [!NOTE]
> 后续功能工作将继续使用同一对话和分支。实际工作中，通常会针对不同文件类型使用不同分支或拉取请求 (PR)；本研讨会采用简化流程，以便聚焦核心概念。

在本课中，你将：

- 从 GitHub 议题启动新的 Copilot 对话。
- 在 Plan 模式下确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

后续课程还会更新存储库指令、自定义现有 `quality-checks` 技能、添加模型上下文协议 (MCP) 验证、创建质量保证 (QA) 智能体，并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩展，访客需要按类别和发行商筛选游戏。待办事项中的议题描述了该功能，但组合类别等细节需要在编码前确定。你将先用 Plan 模式做出这些决定，然后授权 Autopilot 执行范围明确的实现。

## 背景

在开发流程中引入人工智能 (AI) 编码智能体并不会改变基本原则，事实上这些原则变得更加重要。大多数开发人员遵循类似以下的流程：

1. 从描述需求的已登记议题开始。
2. 制定构建方案。
3. 构建并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码通过审查且持续集成 (CI) 流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述步骤类似。

遵循标准流程可以确保 AI 生成的代码满足需求，并经过与手写代码相同的审查过程。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先创建计划，且不能编辑项目文件。
- **Autopilot**：智能体自主工作，编写代码、运行测试并反复迭代，直至任务完成。

先使用 Plan 模式审查计划，然后使用 Autopilot 实现计划。

## 从议题开始

开始筛选功能前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd>，打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，并返回 **Session** 选项卡。

现在提示开头应显示 `#7`（或类似编号）。`#` 可将 GitHub 上的议题或拉取请求 (PR) 添加到上下文中。

## 规划筛选功能

规划可以在将实现任务交给 Copilot 前，先确定功能或任务的实施方式。对于复杂任务，花些时间规划总是有益的。现在切换到计划模式并请 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，再确定最佳方案并制定计划。

3. Copilot 可能会询问筛选功能的工作方式。根据自己的偏好回答，没有错误答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动查看计划。
6. 如果计划中的任何部分不符合你的决定，请要求 Copilot 修改。

## 批准 Autopilot

计划已编写并审查完毕，现在开始实现。使用 Autopilot 让 Copilot 执行计划。

Autopilot 会让 Copilot 反复处理问题，直到它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前安装版本中含义相同的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后，实现可能会立即开始，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再判断检查是否完成。

## 审查并验证实现

代码生成后，和其他代码一样，在合并前需要审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 根据议题和规划时做出的决定审查结果。
4. 完成审查后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 修复任何失败项。

## 探索新功能

代码看起来不错，但能否正常运行？与之前一样，启动应用并通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中确定的组合筛选。
4. 确认重置和无结果时的行为符合议题和你的决定。
5. 返回 Codespace，并请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已使用不同的对话模式构建并审查功能。在本课中，你：

- 从 GitHub 议题启动了新的 Copilot 对话。
- 在 Plan 模式下确定了需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来深入了解代码生成方式，确保代码遵循已记录的实践：[使用自定义指令][next-lesson]。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
