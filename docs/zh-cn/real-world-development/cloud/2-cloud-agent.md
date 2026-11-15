---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot cloud agent 可以自主完成更新代码、添加功能等任务。完成后，它会创建草稿 PR，供开发人员审查。这样可以将繁琐任务交给智能体，加快开发进度，让开发人员专注于更重要的工作。

本课将探索 Copilot cloud agent 的以下方面：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初开发网站的承包商没有留下文档，因此团队希望先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望每款游戏的详情页显示相关游戏（即同类别的其他游戏），方便访客继续浏览。当前不需要完整打磨实现，只要能用于 UX 验收测试即可。这项工作目前受阻，但优先级低于其他 issue。

这些都是容易被搁置的任务，适合交给 Copilot cloud agent。它可以异步处理，开发人员则可以继续其他工作，之后再审查结果并确认符合预期。

## 认识 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发人员一样在后台执行任务。你可以通过多种方式启动它，例如[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理。你可以在会话进行中或完成后查看详情。解决方案准备好供审查时，Copilot 会在拉取请求中提及你。

## 明确界定说明范围的重要性

GitHub Copilot 并非魔法，不能替代思考。看似简单的操作也可能涉及复杂问题，因此[为 Copilot cloud agent 界定任务范围时要谨慎][cloud-agent-best-practices]。把它当作 AI 结对编程伙伴：分阶段工作、学习、尝试并不断调整。生成式 AI 不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其如何引导 Copilot。如果你已完成自定义说明练习，就已经见过 `.github/copilot-instructions.md` 的作用；如果还没看过，现在可以快速浏览。分配任务前，先只读查看存储库中已有的说明文件，以便稍后观察其影响。

在 GitHub Web UI 中或已运行的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释的要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将这些说明限定为 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` issue 分配给 cloud agent 时，留意它创建的 PR 是否遵循这些说明中的 TSDoc 注释、TypeScript 约定和文件注释头。之后审查 PR 时还会再次检查。

## 为 Copilot cloud agent 设置开发环境

编写代码通常需要特定环境和设置脚本，以确保环境准备就绪。将任务分配给 Copilot 时也一样，它会像软件工程师一样完成工作。

Cloud agent 使用 [GitHub Actions][github-actions] 执行任务。你可以通过 `.github/workflows/copilot-setup-steps.yml` 中配置的[专用设置工作流][setup-workflow]自定义环境，在它开始工作前安装所需开发工具和依赖项。实验已预先配置此工作流，以保证流程顺利并便于学习。它会为单体 Astro 应用安装 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在存储库的 Actions 选项卡中测试这些设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 只授予这些步骤所需的最低权限。Copilot 会获得自己的操作令牌。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果不在设置步骤中克隆，Copilot 会在步骤结束后自动完成。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置：Node.js（应用现为 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，供构建和测试使用。
      - name: Set up the database
        run: npm run db:setup
```

这看起来与其他 GitHub 工作流类似，但有几点值得注意：

- 工作流包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器可让你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于测试工作流是否成功，而不必等待 Copilot 启动它。

## 添加文档

大多数项目都存在文档过时或缺失的问题。这类技术债务常被搁置，会拖慢工作效率，并增加维护代码库或引导新开发人员的难度。Copilot 擅长创建文档，因此很适合把这项工作分配给 Copilot cloud agent。它会在后台生成文档，之后的练习会审查其成果。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

    ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的 PR，其标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会在 PR 中创建待办列表。

> [!NOTE]
> Copilot 创建的待办列表可能需要几分钟才会显示。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定最佳处理方案。

13. 审查任务列表及 Copilot 准备执行的工作。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理 issue 的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到状态更新。

16. 浏览实时会话，观察 Copilot 如何探索代码、了解现状、暂停思考并确定计划，然后编写代码。

这可能需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让开发人员专注于其他工作。接下来会同时再分配一项任务，然后转而为应用编写功能代码。

## 在游戏详情页添加相关游戏

如前所述，GitHub Copilot cloud agent 的一大优势是可以拆分工作：你处理一项任务，它处理另一项。为设计团队添加相关游戏功能可能不需要很长时间，但也可以利用这段时间做其他工作。现在将这项任务分配给 Copilot cloud agent。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择空白模板 **Blank issue**。
5. 将 **Title** 设为 `Show related games on the game details page`。
6. 将 **Description** 设为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

很快，你会在 issue 的第一条评论中看到一组 👀，表示 Copilot 已开始工作。

![Copilot 使用眼睛表情符号表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。它的工作方式类似软件工程师，你不必一直监控，完成后再审查即可。现在可以转而创建和使用自定义智能体。

## 总结和后续步骤

本课探索了 [GitHub Copilot cloud agent][copilot-agents]。你可以将 issue 分配给 cloud agent，让它异步工作。Copilot 可以处理技术债务、创建新功能，或协助将代码迁移到其他框架。

本课介绍了以下概念：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

Copilot 正在后台工作，现在可以转而创建并使用自定义智能体。[Copilot cloud agent 也支持 MCP 服务器][cloud-agent-mcp]，并可使用之前课程中介绍的自定义说明。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
