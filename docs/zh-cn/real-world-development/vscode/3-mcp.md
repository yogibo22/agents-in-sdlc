---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用代理模式构建了筛选功能。创建拉取请求前，应先在浏览器中确认功能确实正常。现在连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器并测试功能，而不必亲自逐一操作应用，然后发布分支并创建 PR。

在本练习中，你将：

- 了解模型上下文协议 (MCP) 及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 请 Copilot 使用它在浏览器中手动测试筛选功能。
- 发布分支并为筛选功能创建拉取请求。

## 什么是模型上下文协议 (MCP)？

代理模式可以访问编辑器之外的资源，从而变得更强大。Copilot 使用模型上下文协议 (MCP) 与外部工具和服务通信。

![示意图展示代理模式如何与上下文、大语言模型和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)][mcp-blog-post]为人工智能 (AI) 智能体提供了与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时访问最新信息（使用资源），并代表你执行操作（使用工具）。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理智能体与外部工具（例如现有 API 或 npm 包等本地工具）之间的通信。每个 MCP 服务器都代表一组智能体可以访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP 服务器][github-mcp-server]**：提供一组用于管理 GitHub 存储库的应用程序编程接口 (API)，让 AI 智能体能够创建存储库、更新现有存储库，以及管理议题和拉取请求。
- **[Playwright MCP 服务器][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力，让 AI 智能体能够浏览网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可提供不同的工具和资源。GitHub 提供 [MCP 注册表][mcp-registry]，帮助开发者发现资源并为生态系统做出贡献。

> [!CAUTION]
> 出于安全考虑，应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请仔细审查其源代码、验证发布者并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可使用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 你会看到已经配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` 条目随项目模板提供，因此 Copilot 能在上一练习中读取议题待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装。VS Code 通过 HTTP 连接该服务器；Copilot 首次使用其工具时，你需要使用 GitHub 完成身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]会为 Copilot 提供可控的浏览器，正好可以用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列，使文件如下所示：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。
3. 如果 Copilot 后续报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

与远程 `github` 服务器不同，`playwright` 是一个**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志让 Playwright 在不显示窗口的情况下运行浏览器，这对于没有桌面环境的 Codespace 是必需的。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过此步骤，下一步中 Copilot 首次需要相关工具时，VS Code 会启动服务器并提示你信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可操作的真实浏览器。智能体可以打开网页、浏览页面、应用筛选条件并读取结果，然后向你总结观察到的情况，因此无需离开对话即可快速验证功能行为。

Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]而不是屏幕截图。也就是说，智能体会像辅助技术一样，根据结构化且带标签的元素（按钮、链接和列表项）进行判断，因此简单的功能检查也可以顺便验证基本无障碍情况。

连接服务器并启动应用后，请 Copilot 测试刚刚构建的筛选功能：

1. 确认应用正在 `http://localhost:4321` 运行；如果开发服务器已停止，在存储库根目录运行以下命令：

   ```bash
   npm run dev
   ```
2. 向 Copilot 发送以下提示：

   ```text
   Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

   1. Go to the games page and note how many games are listed.
   2. Apply a category filter and confirm the list updates to only show games in that category.
   3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
   4. Combine a category and a publisher filter and confirm the results respect both.

   Report what you observe at each step, and call out anything that does not behave as expected.
   ```
3. 如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。
4. 查看 Copilot 的报告，并按议题验收标准审查其观察结果。

Copilot 会通过 Playwright MCP 服务器启动浏览器并逐步执行检查，然后报告结果。根据议题中的验收标准审查其总结。如果发现问题，可以继续询问，或让它先修复代码再创建拉取请求。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板并审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板中显示 Copilot 更改的截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

创建拉取请求的方式有多种，包括使用 github.com 和 GitHub 命令行界面 (CLI)。既然你正在使用 GitHub Copilot，就让它来创建 PR。它可以找到相关议题并将 PR 与议题关联起来。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认智能体下拉列表中选中了 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 请 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会提示你登录 GitHub，请按提示授权。
5. 查看 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 生成的链接审查拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！在本练习中，你：

- 了解了模型上下文协议 (MCP) 及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到了工作区。
- 使用它在浏览器中手动测试筛选功能，然后才发布更改。
- 发布了分支并为筛选功能创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么人人都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [GitHub MCP 服务器][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
