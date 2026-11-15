---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解一些背景信息，就应确保它能访问这些上下文。[指令文件][instruction-files]可提供此类指导，让 Copilot 不仅理解要做什么，也理解应该怎么做。

本练习将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及按路径应用的指令文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加一条存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，本学习路径不会使用前后对比提示。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示指令的效果。后续审查云智能体创建的拉取请求时，你会看到该指令文件的影响。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加注释块，说明文件的用途。

通过指令文件，可以确保 Copilot 了解这些要求，并按相应实践完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以通过它引导 Copilot 提供更相关的建议和代码片段，也可以指定编码规范、首选库以及注释风格。指令既可以适用于整个存储库，也可以针对特定文件类型。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单一指令文件。内容应是适用于大多数 Copilot 聊天或 CLI 请求的项目级信息，例如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的指令文件。可为特定语言（例如 TypeScript 或 Astro），或特定任务（例如创建 UI 组件或一组单元测试）提供指南。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot [还支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

本研讨会无法全面介绍如何创建指令文件，但示例项目展示了一种有代表性的做法。总体而言：

- 让 `copilot-instructions.md` 专注于项目级指导，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（例如单元测试、Astro 组件和数据层）或任务提供指导。
- 使用自然语言，保持说明清晰，并提供代码应如何编写（以及不应如何编写）的示例。

创建指令文件没有唯一正确的方法，使用 AI 也是如此。通过实践，你会逐渐找到适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的指令文件。浏览本项目中的文件时，你会看到其中包含针对多种任务的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作方式各异，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应产品的课程会在相关处说明。
>
> 若要查找模板或起点，请浏览 [awesome-copilot][awesome-copilot]，其中收录了指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

花些时间查看存储库自带的指令文件：包括一个核心 `copilot-instructions.md` 文件和一组用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意其中简要介绍的项目，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等章节。**Code standards** 中还包含 **GitHub Actions Workflows** 子章节。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并查看其中内容。注意这里包含 Astro 文件、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了相对于存储库根目录的 glob，用于确定指令适用的文件。这里，任何符合 `**/*.test.ts` 的 TypeScript 测试文件都会匹配。
5. 查看本项目专门用于创建单元测试的指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成可复用的小文件，并让 Copilot 参考具体示例。（这些路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 章节记录了项目编码标准，但目前尚未要求为代码添加文档。接下来会添加 TSDoc 文档注释和文件注释头的规则。

## 添加一项存储库标准

接下来只需在这里编辑一次：添加一条项目级规则，要求以 TSDoc 文档注释和文件级注释头的形式在代码中编写文档。稍后在本学习路径中向云智能体分配议题时，它会采用这条规则。

编辑之前，先创建一个分支（云智能体会从议题指向的分支读取指令，但仍建议干净地提交这些编辑）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，以改进 Copilot 的代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到靠近第 27 行的 **Code formatting requirements** 章节。注意其中记录了项目编码标准，但尚未规定代码文档要求，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所见，指令文件既可以放在存储库级别（`.github/copilot-instructions.md`）提供全局指导，也可以使用 `*.instructions.md` 针对特定语言、文件类型或任务提供指导。刚添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令。下一练习将把议题分配给 Copilot，届时它会从 `main` 创建分支。因此，必须先将指令更改合并到 `main`，云智能体才能使用。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。下一练习中，云智能体就会读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。设置分支是为了让研讨会更贴近真实项目的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何读取本项目的指令文件，并向 `.github/copilot-instructions.md` 添加了一项存储库级标准。接下来的课程中，可以在云智能体生成的拉取请求里看到这项标准的实际效果。

接下来将[把第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot - 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
