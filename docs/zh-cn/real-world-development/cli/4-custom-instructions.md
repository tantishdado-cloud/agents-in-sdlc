---
title: "练习 4 - 使用自定义指令引导 Copilot"
description: "查看存储库指令，添加文档标准，并将其应用到筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应确保 Copilot 能访问相应指导。[指令文件][instruction-files]不仅描述想要什么代码，也说明代码应该如何组织。现在已经实现了筛选功能，接下来将查看 Copilot 使用的指令、添加一项文档标准，并将其应用到代码中。

本课将：

- 了解存储库指令和按路径应用的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 查看指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值（如适用，也包括可注入的 `db` 参数）。
- 可复用 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释也应保持更新。
- 应保留现有格式和 lint 指南。

使用指令文件可确保 Copilot 了解这些要求，并按相应实践完成任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以通过它引导 Copilot 提供更相关的建议和代码片段，也可以指定编码规范、首选库以及注释风格。指令既可以适用于整个存储库，也可以针对特定文件类型。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单一指令文件。内容应是适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：针对特定语言、文件类型或任务提供指导的指令文件。

> [!NOTE]
> 其他指令格式及其支持情况会因产品界面而异。依赖特定格式前，请先查看[自定义指令支持参考][custom-instructions-support]。

## 查看本项目中的自定义指令文件

项目模板已经包含一组指令文件。先查看现有内容，再进行更改，以便了解指令的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，注意其中简要介绍的项目和编码指南。这些指令适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并查看其中内容。注意这里包含 Astro 文件、Drizzle 数据层、测试等方面的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它指定指令适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他指令文件和项目现有文件的链接。这样可以将较大的指令集拆分成可复用的小文件，并让 Copilot 参考具体示例。

## 更新指令文件以符合团队指南

现有文件已经很有帮助，但还可以进一步完善。现在修改核心 `copilot-instructions.md` 文件，要求新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 章节（大约位于第 35 行）。
2. 在最后添加以下项目符号：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时载入存储库指令。保存更新后，退出并恢复筛选功能对话，让 Copilot 在不丢失功能上下文的情况下读取新指令。

1. 如果筛选功能对话仍在运行，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，选择筛选功能对话，并确认检出的分支为 `game-filters-cli`。
3. 使用以下提示让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查已更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何从指令文件获取上下文，并为功能添加了新的编码标准。接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持情况][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot - 指令文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
