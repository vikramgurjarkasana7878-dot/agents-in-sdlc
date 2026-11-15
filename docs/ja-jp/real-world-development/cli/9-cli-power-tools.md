---
title: "レッスン 9 - GitHub Copilot CLI の便利な slash command"
description: "コンテキストの管理、モデルの選択、セッションの共有、cloud agent への任意の委任に使う slash command を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

優れた CLI ツールと同様に、GitHub Copilot CLI には slash command が多数用意されています。高度な機能、内部情報、追加の設定オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、さらに便利なコマンドをいくつか確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` で、使用できるモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認します。

## シナリオ

CLI の基本的なワークフローを終えました。ここでは、コンテキスト管理、モデルの切り替え、セッション共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルの最大 context window に達することがあります。Copilot CLI は必要に応じて会話を自動的に compact します。また、slash command でコンテキストを確認したり、自分で compact したりできます。

1. Codespace に戻り、まだ起動していなければリポジトリ ルートから Copilot CLI を起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在の token 使用量、システム instruction、tool、メッセージ、空き領域に分割されたコンテキストを確認します。
4. 会話を compact します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比較します。会話が短い場合は、大きな違いが見られないことがあります。

> [!NOTE]
> context window がいっぱいになると、Copilot CLI が自動的に compact します。実行タイミングを選びたい場合は `/compact` を使います。関係のない別タスクに切り替えて新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、モデルの一覧表示と選択ができます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選択します。<kbd>Esc</kbd> を押して一覧を閉じることもできます。

## セッションを共有する

チームで作業し、学びを共有すると、AI tool の活用方法を互いに向上できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンにある `/share` の選択肢を確認します。
2. セッションを共有する場合は、たとえばローカルの Markdown にエクスポートする `/share file` のように、目的に合った出力先を選びます。
3. エクスポートした内容を、誰かに送信したり公開したりする前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有するつもりのないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を並行化または委任する

ワークショップの基本内容は完了です。Copilot CLI には、大規模なタスクに使えるコマンドもあります。

- `/fleet` は独立したサブタスクを subagent に割り当て、並行して実行します。
- `/worktree` は別のタスク用に独立した Git worktree を作成します。
- `/delegate` は Copilot cloud agent にタスクを渡します。cloud agent は非同期に作業し、pull request を開くことがあります。

これらのコマンドは追加の worktree やリモート作業を作成することがあるため、任意の機能です。試す場合は、スコープを明確にした新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI の slash command を使うと、設定を変更したり、セッションを共有したり、内部で起きていることを確認したりできます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` で、使用できるモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認しました。

まだ多くの slash command があり、Copilot CLI で試せることもあります。次は、これまでに学んだこととその先の手順を[振り返りましょう][next-lesson]。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
