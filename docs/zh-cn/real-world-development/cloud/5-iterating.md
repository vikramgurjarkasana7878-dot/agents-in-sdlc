---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 合作完成了多项改善用户体验和添加功能的任务：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包括高对比度和浅色模式切换在内的无障碍功能。现在检查代码更改，必要时向 Copilot 提供反馈，进一步改进结果。

### 场景

如前所述，生成式 AI 不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。因此，在开启团队审查前，先检查 GitHub Copilot 为文档、相关游戏功能和无障碍功能提出的更改。

## 安全与 GitHub Copilot cloud agent

Copilot cloud agent 异步且自主地执行任务，因此设有一些安全限制：

- Copilot 对存储库只有读取权限，对自己用于编写代码的分支才有写入权限。
- Cloud agent 在 GitHub Actions 中运行，并使用单独的临时环境进行工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- [默认情况下，外部资源访问受限](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot cloud agent 创建的第一个 PR，即为代码添加文档的 PR。这里使用 GitHub.com 上的标准 PR 界面。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器调用。如有需要，可以[自定义或禁用 Copilot cloud agent 的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 或更完整的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题中会包含 **[WIP]**。此时请等 Copilot 完成，可能需要几分钟。

4. PR 准备好后，选择 **Files changed** 选项卡，审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新增的 TSDoc 文档注释和其他文档。具体更改可能会有所不同。

   检查 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自你在练习 2 开始时查看的自定义说明文件。

6. 审查完成后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有，选择 **Approve and run workflows**。

    ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入 PR 检查队列。正常情况下，单体 Astro 应用的项目检查都会通过。这可能需要几分钟。

## 向 GitHub Copilot 请求更改

与 Copilot 协作处理拉取请求并非单向过程。你可以像提及团队成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取评论，并启动新会话来处理请求。由于结果具有非确定性，这里无法提供唯一的提示词。可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 添加评论，请求 Copilot 更改生成的文档；像提及其他用户一样提及 **@copilot**。可以使用上面的任一建议，也可以提出其他文档改进请求。
2. 选择 **View Session**，观察 Copilot 执行更改。它会启动新会话来更新内容。
3. 选择 **Back to pull request** 返回 PR。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新的提交。
5. 选择 **Files changed** 选项卡，审查更改。

可以继续迭代，直到满意为止。之后可以将 PR 从草稿改为可供审查状态，并合并到主分支。

![将 PR 转为可供审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

回到 Copilot 为解决游戏详情页显示相关游戏 issue 创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或更完整的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查完成后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流正在等待批准。
7. 如果有，选择 **Approve and run workflows**。

    ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 工作流应会进入 PR 检查队列。正常情况下，单体 Astro 应用的项目检查都会通过。这可能需要几分钟。
9. **可选：** 可以在 Codespace 中切换到该分支，手动测试相关游戏功能。打开 Codespace 终端，运行以下命令，并将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 **copilot/fix-8**：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能。与之前一样，你可以继续迭代并要求 cloud agent 更新。例如，要求 Copilot 调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（注意：分配此任务时，你尚未更新自定义说明）。和之前一样，可以在 **Conversation** 选项卡添加评论，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在 mission control 请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或更完整的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会包含 **[WIP]**。请等待 Copilot 完成，可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 检查实现，重点关注：
   - 用于切换模式的 UI 组件。
   - 使用本地存储持久保存用户偏好的方式。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 审查完成后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有，选择 **Approve and run workflows**。

    ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入 PR 检查队列。正常情况下，单体 Astro 应用的项目检查都会通过。这可能需要几分钟。
10. **可选：** 可以在 Codespace 中切换到该分支，手动测试无障碍功能。打开 Codespace 终端，运行以下命令，并将 `<branch-name>` 替换为 Copilot 创建的分支名：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，确认功能符合预期，并且在页面重新加载后仍保留设置。

注意，无障碍自定义智能体帮助 Copilot 根据无障碍最佳实践实现这些功能。如果发现无障碍问题或改进点，可以像处理前面 PR 一样，在评论中提及 **@copilot** 请求更新。

## 可选练习 - 继续委托任务

当你能把真实待办任务交给 cloud agent 并转而处理其他工作时，它最能发挥作用。可以在自己的存储库中创建更多 issue 并分配给 Copilot，练习这种工作方式。可尝试：

- 在游戏详情页创建支持者意向表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 学习路径。通过这些课程，你：

- **检查了存储库自带的自定义说明文件**，以便之后观察它们对 cloud agent 输出的影响。
- **将 issue 分配给 Copilot cloud agent**，并观察它设置环境、制定计划和异步执行任务。
- **创建并使用自定义智能体**处理无障碍工作，添加高对比度和浅色模式切换。
- **使用 Copilot agents 页面作为控制中心**，监控并引导无障碍会话。
- **审查并迭代改进 cloud agent 创建的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成 Cloud agent 学习路径。其他学习路径可以补充此处的实践，探索不同的开发环境：

- 🖥️ **[VS Code 学习路径](../../vscode/)** — 在 IDE 中直接体验 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)** — 在终端中完成两个经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 桥接。

你也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖 issue 分配给 cloud agent。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
