---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几节练习中，你要求 Copilot cloud agent 执行了三项改进用户体验和添加功能的任务。Cloud agent 虽然可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心，可以查看所有创建了未关闭 PR 的智能体任务，了解已执行的操作，还可以引导正在进行的会话。

本课将介绍如何：

- 浏览智能体页面，监控 cloud agent 任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

分配高对比度模式任务后，团队意识到也可以同时添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，加入这项功能很合适。你将引导智能体，让它在高对比度模式之外再添加浅色模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到你分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的各项步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 当前的工作状态和处理方式而有所不同。

4. 同时留意右侧的拉取请求 (PR) 面板。可以通过它查看 PR 和已更改文件，进一步监控任务。

## 引导 cloud agent

现在已经看到正在运行的任务，接下来要求 Copilot 在实现高对比度模式时也添加浅色模式切换开关。

1. 选择与添加高对比度模式有关的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![Mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始执行任务。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示词：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的 cloud agent 任务，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 留意 Copilot 如何确认收到提示词并将其加入工作流程。

## 等待 Copilot 完成

与之前一样，Copilot 会继续处理更新后的任务。收到请求后，它会先完成当前步骤，再将新请求加入工作流程。

这也需要几分钟，正好可以暂停一下，回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，它是监控和引导 GitHub Copilot cloud agent 任务的中心。你可以跟踪所有进行中和已完成的任务、审查执行情况，还可以调整任务范围或补充说明，以引导正在进行的任务。

本课介绍了以下概念：

- 使用 mission control 和 agents 页面监控 cloud agent 任务。
- 引导正在进行的会话，要求添加其他功能。

Copilot 正在完成无障碍功能。接下来将[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
