---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案，使用 Autopilot 根据 issue 实现筛选功能，再通过 Interactive 模式审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了一项小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 支持团队已有的工作流程，帮助我们用合适的方法构建正确的功能。接下来的几课将遵循典型的智能体驱动开发流程：从 issue 开始实现功能，确保代码有效、行为符合预期，最终成功合并到项目中。

> [!NOTE]
> 继续完成该功能时，会沿用同一段对话和同一分支。实际工作中，不同类型的文件通常会使用不同分支或 PR；这里简化流程，以便专注于核心概念。

本课将介绍如何：

- 从 GitHub issue 启动新的 Copilot 对话。
- 在 Plan 模式中定义需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续完成该功能时，你还将更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并打开功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩大，访客需要按类别和发行商筛选游戏。待办 issue 已描述这项功能，但类别组合等细节需要在编码前达成共识。你将使用 Plan 模式确定这些决策，然后授权 Autopilot 在明确范围内实现功能。

## 背景

在开发流程中引入 AI 编码智能体不会改变基本原则，反而让这些原则更加重要。大多数开发人员遵循类似以下的流程：

1. 从已创建的 issue 开始，了解需要完成的工作。
2. 为需要构建的内容制定计划。
3. 构建并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 不同团队和组织的具体流程会有所不同，但大多是以上流程的变体。

遵循这种标准方法，可以确保 AI 生成的代码满足既定要求，并经过与手写代码相同的审查流程。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可循环切换模式：

- **Interactive**：你与智能体协同工作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先创建计划，且不能编辑项目文件。
- **Autopilot**：智能体自主工作，包括编写代码、运行测试并不断迭代，直到任务完成。

先在 Plan 模式中审查计划，再使用 Autopilot 实现。

## 从 issue 开始

开始筛选功能前，返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重启。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
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
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将 issue 添加到提示词，并返回 **Session** 选项卡。

现在提示词应以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的 issue 或拉取请求 (PR) 添加到上下文。

## 规划筛选功能

规划能让你在交给 Copilot 前，先确定实现功能或执行任务的方法。对于复杂工作，花些时间规划总是值得的。现在切换到 Plan 模式，请 Copilot 创建计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示词下方的模式指示器显示 **Plan**。
2. 在上一步添加的 issue 引用后输入以下提示词：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始探索项目并确定合适的实现方案。

3. Copilot 可能会询问筛选行为的细节。根据期望回答即可。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看计划。
6. 要求 Copilot 修订任何与你的决定不符的部分。

## 批准 Autopilot

计划撰写并审查完毕后，就可以开始实现。使用 Autopilot 让 Copilot 自动推进。

Autopilot 会让 Copilot 持续迭代，直到它认为任务完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或所安装版本中类似的选项。
2. 确认提示词下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何根据已确认的计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，应先解决设置问题，再认定检查已完成。

## 审查并验证实现

与其他代码一样，生成的代码也需要在合并前审查。下面审查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 对照 issue 和规划时达成的决定检查结果。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的输出，并要求 Copilot 解决失败项。

## 探索新功能

代码看起来没问题，但能否正常运行？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 要求 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 测试类别筛选、发行商筛选，以及计划中约定的组合方式。
4. 确认重置和无结果时的行为符合 issue 和已达成的决定。
5. 返回 Codespace，要求 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已使用不同对话模式构建并审查功能。本课中，你：

- 从 GitHub issue 启动了新的 Copilot 对话。
- 在 Plan 模式中定义了需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来将深入了解代码的生成方式，确保代码遵循已记录的实践，并[使用自定义说明][next-lesson]。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
