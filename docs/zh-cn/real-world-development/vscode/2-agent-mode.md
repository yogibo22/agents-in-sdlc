---
title: "练习 2 - 使用 Copilot 代理模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发人员的工作流程往往包括找到所有相关文件、进行修改、运行测试、调试、找出遗漏的文件并再次修改，循环往复。

Copilot 代理模式可以帮助处理这类工作。

Copilot 代理模式可以在 IDE 中更自主地工作。它的行为类似开发人员：先探索现有项目结构，执行所需更新，运行测试等任务，并自动修复发现的问题。现在了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和云智能体的名称相似，但适用于不同的工作方式。代理模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解如何：

- 使用 GitHub Copilot 代理模式跨用户界面 (UI) 和数据层实现新功能。
- 使用 Copilot 代理模式探索项目、识别相关文件并协调更改。
- 在将代码合并到代码库之前审查 Copilot 代理模式生成的更改和测试。

## 场景

游戏列表不断增加，因此需要让用户按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 代理模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行更改前，先浏览 Tailspin Toys 网站，了解它当前的功能。

该网站是一个带有开发者主题的桌游众筹平台，用户可以浏览游戏列表和详情。网站是一个 Astro 应用，会在构建时将页面生成为静态超文本标记语言 (HTML)。页面通过 Drizzle 对象关系映射 (ORM) 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端应用程序编程接口 (API) 或客户端用户界面 (UI) 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则使用少量范围明确的 Astro `<script>` 和标准文档对象模型 (DOM) API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

5. 如果弹出对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。
6. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），同时选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost 链接时，系统会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace（当前运行网站服务器）的私有隧道。

### 浏览网站

网站运行后，可以浏览其功能。主要功能包括：

- **主页**：显示桌游列表、标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页面，显示标题、说明、发行商和类别等信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前：
>
> 1. 返回 Codespace。
> 2. 打开 Copilot Chat 面板。
> 3. 选择 **New Chat**，开始全新对话。模式和模型的选择因练习而异；相关步骤会在需要时说明。

网站初始版本已能正常运行，但还需要添加更多功能。先查看待办事项。通过模板设置项目时，系统已自动创建议题待办事项；现在请 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。Agent 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并不偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此将其列在说明中，希望生成的代码建议相对一致，从而带来良好体验。不过，大语言模型 (LLM) 具有概率性，因此建议可能与研讨会中的内容不同。这完全正常。

> [!NOTE]
> 由于大语言模型 (LLM) 具有概率性，Copilot 可能会使用不同的模型上下文协议 (MCP) 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问议题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

注意 Copilot 还根据它对用户价值的判断为议题排定了优先级。

## 查看指令文件

启动智能体生成代码前，先查看用于向 Copilot 提供上下文的指令文件。本练习将使用 [UI 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中包含向网站添加功能时的指导。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看添加功能时的总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各文件类型专用指令的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在目录。这样可以复用指导、将复杂内容拆分为较小部分，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用程序进行三项更改：

- 在数据层 (`src/lib/`) 添加或改进剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

此外，在将更改合并到代码库前，还需要运行并通过测试。Copilot 代理模式可以执行这些任务，现在开始添加功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图智能体下拉列表中仍选中了 **Agent**。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，请 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找相关文件，包括数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂度，这可能需要几分钟。过程中编辑器可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会提供 **Keep** 或 **Undo** 选项，因此不必在工作进行时立即选择。

7. 按 Copilot 的提示选择 **Continue** 以运行测试。

   ![Copilot Chat 面板中询问用户是否允许运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会有暂停，甚至出现测试失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务且未检测到错误。

   ![Copilot 代理模式完整聊天会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行着网站的浏览器并探索新功能。
11. 确认功能正常且已审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加功能。你了解了如何：

- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能。
- 使用 Copilot 代理模式探索项目、识别相关文件并协调更改。
- 在将代码合并到代码库之前审查 Copilot 代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]并创建拉取请求。

### 额外探索练习 - 实现分页

随着游戏列表不断增加，可能需要启用分页。使用本练习中学到的技能，请 Copilot 更新网站以实现分页。可以考虑：

- 遵循现有最佳实践，包括使用已有指令文件。
- 确定分页方式，例如是否允许用户选择每页项目数，还是使用固定数量。
- 编写提示时提供 Copilot 所需的指导，确保实现符合预期。
- 通过迭代向 GitHub Copilot 请求更改并补充上下文。这是正常的协作流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
