---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式人工智能 (AI) 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就要确保它能访问这些上下文。[指令文件][instruction-files]可以提供这些指导，让 Copilot 不仅了解要做*什么*，还了解应该*如何*完成。

在本练习中，你将：

- 了解如何通过存储库自定义指令和按路径限定范围的指令文件，为 Copilot 提供项目上下文、编码指南和文档规范。
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准。

> [!NOTE]
> 与 VS Code 和命令行界面 (CLI) 工作流不同，本练习不会使用前后对比提示。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示指令的影响。稍后审查云智能体生成的拉取请求时，可以看到这些指令的作用。

## 指令文件

### 场景

Tailspin Toys 团队有一套开发实践指南和要求：

- 数据层必须始终有单元测试。
- 用户界面 (UI) 应采用暗色模式并具有现代感。
- 代码中应使用 TSDoc 文档注释。
- 每个文件开头都应添加一段注释，说明文件的用途。

使用指令文件可以确保 Copilot 获取正确的信息，并按照这些实践要求完成任务。

### 自定义指令

自定义指令可为 Copilot 提供上下文和偏好设置，帮助它理解编码风格和需求，从而生成更相关的建议和代码片段。可以指定编码规范、库以及注释风格，也可以为整个存储库或特定文件类型创建指令。

指令文件分为两种：

- `.github/copilot-instructions.md`：每次针对该存储库向 Copilot 发出请求时，都会发送此指令文件。它应包含适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可为特定任务或文件类型创建这些指令文件，例如 TypeScript、Astro、UI 组件或新的单元测试。

> [!NOTE]
> 在集成开发环境 (IDE) 中，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

本研讨会不详细介绍如何创建指令文件，但示例项目展示了一种有代表性的做法。总体而言：

- 让 `copilot-instructions.md` 专注于项目级指导，例如项目说明、目录结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，确保指导清晰，并提供代码的正反示例。

创建指令文件没有唯一正确的方法，使用 AI 也是如此。通过实践可以找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的指令文件。浏览本项目的文件时，可以看到其中包含针对多种任务的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。每个使用界面的操作方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应工作流的课程会在相关处介绍具体方式。
>
> 如果需要模板或起点，可以浏览 [awesome-copilot][awesome-copilot] 存储库，其中包含指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览项目中的自定义指令文件

先查看此存储库自带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意其中对项目的简要说明，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。它们适用于你向 Copilot 提出的任何请求。
3. 打开 `.github/instructions` 文件夹并浏览其中内容。注意其中有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用来确定适用的文件。这里与 `**/*.test.ts` 匹配的 TypeScript 测试文件都会应用这些指令。
5. 查看此项目中创建单元测试的专用指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。注意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可复用的文件，并为 Copilot 提供可参考的示例。（这些路径相对于指令文件所在目录，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但还没有要求代码中包含文档注释。接下来会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来要做的是唯一一处编辑：添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。云智能体稍后处理本工作流分配的议题时会读取这条规则。

编辑前，先创建一个分支（云智能体会读取议题目标分支中的指令，但仍建议干净地提交这些更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来添加项目编码标准，以改善代码建议。

2. 重新打开 `.github/copilot-instructions.md`。
3. 找到 **Code formatting requirements** 部分（通常位于第 27 行附近）。查看其中记录的项目编码标准；目前没有代码文档规则，因此生成的辅助函数没有文档注释。
4. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建指令文件提供全局指导，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。刚添加的文档注释规则属于项目级标准，因此适合放在存储库级文件中。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令。下一练习将把议题分配给 Copilot，届时它会从 `main` 创建分支。因此，必须先将指令更改合并到 `main`，云智能体才能读取。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求，并合并该请求。下一练习中，云智能体处理分配的议题时就会读取这些指令。

> [!TIP]
> 如果你有权限直接推送到 `main`，可以跳过创建分支和拉取请求的步骤：完成编辑后，将提交推送到 `main`，再继续下一课。创建分支并通过拉取请求合并，是为了让研讨会流程更贴近实际项目中的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何读取本项目的指令文件，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准：

- 了解了存储库自定义指令和按路径限定范围的指令文件如何为 Copilot 提供项目上下文、编码指南和文档规范。
- 在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。

后续练习中，云智能体生成的拉取请求会体现这条标准。

接下来，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更优 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
