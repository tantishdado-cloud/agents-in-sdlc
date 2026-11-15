---
title: "练习 2 - 添加星级评分：快速上手"
description: "使用 Copilot CLI 修改游戏卡片，在转发浏览器中检查更改，并将其作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试对话后，现在要对项目进行第一次更改。先从小处着手：游戏数据中已有星级评分，但主页上的游戏卡片尚未显示评分。你将让智能体显示已有评分、审查更改，并将其作为第一个拉取请求合并。

本练习将：

- 在功能分支上启动专注于单一任务的 Copilot 对话。
- 让智能体对项目进行小幅修改。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，游戏详情页已经显示此评分。不过，主页上的游戏卡片只显示标题、类别、发行商和说明。现在让智能体在卡片上显示已有评分，完成一个小型、独立的修改，为首次使用 Copilot CLI 进行开发热身。

## 对话的组成

**对话**是使用 Copilot CLI 完成任务的工作区。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，而不是创建专用工作树。保存的对话可以让你稍后返回同一讨论，而文件和分支仍是磁盘上的普通 Git 状态。

在对话中，可以看到三类内容：你发送的提示和智能体的回答、智能体探索和编辑文件时的工具活动，以及通过 `/diff` 检查的更改。

## 开始对话并请求更改

开始新对话，实施此功能。

1. 返回 Codespace。
2. 如果之前的终端未打开，按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，使用以下命令启动：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保开始新会话。
5. 使用以下提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目，找到用于显示游戏详情的文件并生成所需代码。现在已经使用 Copilot CLI 添加了新功能。

## 审查差异

所有 AI 生成的更改，即使很小，也应在合并前审查。现在在 Copilot CLI 中检查更改。

1. 输入 `/diff` 并检查所有已更改文件。
2. 确认游戏卡片在有评分时显示数值评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖上述两种状态。
4. 查看 Copilot 运行的检查结果，并要求它修复任何失败。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异界面。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的输出具有非确定性，因此生成的代码可能有所不同。请审查功能行为，而不是要求实现必须与某个示例完全相同。

## 检查更改

不能只阅读代码就假定功能正常。让 Copilot 启动网站，然后在 Codespaces 转发的浏览器中检查更新后的 UI。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了五分制评分。
4. 返回 Copilot，让它停止刚才启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

现在已经完成新功能，可以创建拉取请求 (PR)，将代码合并到项目中。

1. 让默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 创建 PR 时会推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，打开 PR。
4. 审查更改的文件和检查结果。
5. 准备就绪后，选择 **Merge pull request** 并确认合并。
6. 返回 Codespace，使用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜！你已使用 GitHub Copilot CLI 提交了第一个更改。具体来说，你：

- 在功能分支上启动了专注于单一任务的 Copilot 对话。
- 让智能体修改游戏卡片。
- 使用 `/diff` 审查了更改。
- 运行应用，并在转发浏览器中确认星级评分。
- 创建并合并了第一个拉取请求。

接下来，[从筛选议题开始，使用 Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
