---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流程、两个拉取请求、可复用自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你通过 GitHub Copilot CLI 完成了 Tailspin Toys 的连续工作流。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预置的筛选 issue。
- 添加了星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求 (PR)。
- 从筛选 issue 开始，在 Plan 模式中确定方案、在 Autopilot 模式中实现，并在 Interactive 模式中审查。
- 使用自定义说明引导智能体，随后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加了 Playwright 模型上下文协议 (MCP) 服务器，并用它在真实浏览器中探索筛选功能。
- 创建并选择了质量保证 (QA) 自定义智能体，以评估需求、覆盖情况、技能结果和浏览器证据。
- 审查了完整的筛选功能更改，并授权 Agent Merge 管理筛选 PR。
- 探索了用于上下文、模型、共享和可选 cloud agent 委托的斜杠命令。

## 完成的内容

本工作坊包含两个 PR 里程碑，每个里程碑都从更新后的 `main` 分支创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating`，并明确显示未评分状态。
2. **筛选功能和质量工作流**：实现筛选功能、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置，并加入相关测试。

从规划筛选功能到创建 PR，你一直使用同一段对话和同一分支。为了简化工作坊，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、自己在浏览器中的检查，以及 Copilot 通过 MCP 进行的浏览器探索。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。创建 PR 前，QA 将这些结果与需求及测试覆盖情况的审查结合起来。

新增测试应解决实际缺口；QA 运行不需要新增测试也可能完全正确。授权合并前，应审查代码和证据；代码更改后，应更新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，你更新了说明、自定义了技能、创建了 QA 配置并配置了 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。说明设定标准，技能描述可重复执行的任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。应审查实际更改和工具结果，而不只是智能体的摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前确定方案，使用 **Interactive** 参与范围明确的更改，仅对范围清晰的任务使用 **Autopilot**。日常编辑可选择更快的模型；复杂工作则选择能力更强的模型并提高推理强度。

上下文和基础设施同样重要。清楚说明要构建*什么*、*为什么*构建以及*如何*构建，会显著影响输出。

## 进一步探索

你已掌握核心工作流程。还可以了解一些 Copilot CLI 功能：

- `/review`：要求代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获得另一种观点。
- `/fleet`：并行协调独立子任务。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务委托给 Copilot cloud agent。

## 后续步骤

提升工具使用能力的最佳方式是持续实践。可以将它用于生产代码、个人项目，或那个构思多年却一直没有动手的小应用。与团队分享经验，也向团队学习，并继续查阅文档。

若想通过终端继续扩展 Tailspin Toys，可选修 [Foundry Backer Concierge 系列][foundry]。若想比较其他工作环境，可探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
