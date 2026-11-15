---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，将 issue 需求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动化检查，并使用 Playwright MCP 在浏览器中查看筛选体验。现在通过一个定义清晰 QA 流程的自定义智能体，将这些能力结合起来。

本课将介绍如何：

- 了解自定义智能体如何与说明、技能和 MCP 工具协作。
- 创建并检查可复用的质量保证配置。
- 选择 QA 智能体，并对照筛选 issue 审查其发现。

## 场景

创建拉取请求 (PR) 前，Tailspin Toys 希望以一致的方式审查需求、代码质量、自动化检查、测试覆盖和浏览器行为。自定义智能体可以协调这一 QA 流程，并提供可复用的报告。

## 什么是自定义智能体？

自定义智能体是在 Markdown 配置文件中定义的 Copilot 专业版本。配置文件描述智能体的用途、说明和可用工具。本工作坊将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用的各项自定义功能各有用途：存储库说明描述团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置则指导 Copilot 如何运用这些能力评估需求并报告发现。QA 配置不会取代这些能力，也不需要另开对话。

## 创建 QA 配置

打开功能 PR 前，先要求 Copilot 创建可复用的 QA 配置。该配置将定义 QA 要执行的检查以及必须遵守的边界。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 要求默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置

使用新智能体前，先检查其配置，确认 Copilot 理解了预期的 QA 流程和边界。这可避免配置不完整或权限过宽，导致只想验证功能时却修改了它。

1. 输入 `/diff`，打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 是必需字段；`name` 是可选字段，但添加后可以为智能体提供明确的显示名称。
3. 阅读配置说明，确认 QA 从需求出发、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持性证据，在更改实现代码前先询问，并且不会提交更改或打开 PR。
5. 如果生成的配置遗漏上述职责或边界，请先要求默认智能体修订，再继续。

## 针对 issue 运行 QA

Copilot CLI 会在对话启动时加载项目智能体。创建配置后，恢复同一段筛选对话并选择 QA，这样它就能使用上下文中已有的 issue 和规划决定。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会显示在可用智能体列表中。它仍然可用，上面的命令会启用它。

2. 使用以下提示词要求 QA 审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体会开始工作。
4. 完成后阅读它提供的报告。

## 总结和后续步骤

你已为工作流添加可复用的专业角色，并审查了它的工作。本课中，你：

- 了解了自定义智能体如何与说明、技能和 MCP 工具协作。
- 创建并检查了可复用的质量保证配置。
- 选择 QA 智能体，并对照筛选 issue 审查了它的发现。

现在，实现、技能更新、QA 配置、测试和验证报告都已准备好供审查。接下来将[汇总这些内容并创建功能 PR][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
