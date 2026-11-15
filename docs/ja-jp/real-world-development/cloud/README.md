---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub でタスクを割り当てると、cloud agent はバックグラウンドでリポジトリを調べ、変更を加え、pull request を作成します。その間、別の作業に取り組めます。

この演習では、cloud agent が従う custom instruction を追加し、GitHub issue を割り当てて作業を依頼します。custom agent を使って作業の進め方を調整し、エージェント ダッシュボードでセッションを監視して案内します。最後に、作成された pull request をレビューし、結果を反復して改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. Custom instructions][ex1] | コンテキスト | cloud agent が従う custom instruction を追加します |
| [2. Cloud agent][ex2] | 非同期エージェント | issue を Copilot cloud agent に割り当てます |
| [3. Custom agents][ex3] | 専門エージェント | custom agent を確認して使います |
| [4. エージェントの管理][ex4] | 監視 | エージェントのセッションを監視して案内します |
| [5. 反復して改善する][ex5] | レビュー | PR をレビューし、Copilot の作業を反復して改善し、次のステップを選びます |

## 前提条件

ワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを利用できる GitHub アカウント
- [ ] アカウントまたは組織で有効になっている Copilot cloud agent

> [!TIP]
> 有料プランを利用していない場合でも、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべての harness を完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> cloud agent は **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free は対象外です。Copilot Business または Enterprise の場合、管理者が組織でこの機能を有効にする必要があります。

> [!NOTE]
> この harness では MCP を扱いません。Copilot で MCP server を使う方法については、[CLI harness](../cli/) または [VS Code harness](../vscode/) を確認してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
