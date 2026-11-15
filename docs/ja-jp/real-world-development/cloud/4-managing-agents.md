---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの 2 つの演習では、ユーザー体験の改善と機能追加を目的とする 3 つの別々のタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクの管理には、github.com の[エージェント ページ][agents-page]など、さまざまな tool を使えます。この mission control では、open pull request (PR) があるすべてのエージェント タスクを確認できます。実行された操作を調べたり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次のことを行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイ コントラスト モードの作成をエージェントに依頼した後、チームはライト モードも追加したいと考えました。サイトのスタイル更新と切り替え機能の実装がすでに始まっているため、あわせて実装するのが適切です。ハイ コントラストに加えてライト モードも実装されるよう、エージェントの作業を案内します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左ペインの両方にあるタスク一覧を確認します。割り当てたタスクとして、次の内容などが表示されます。
    - コードベースのドキュメント更新。
    - ゲーム変更用 API の生成。
    - Web サイトへのハイ コントラスト モード追加。
3. 実行中のタスクを 1 つ選択し、Copilot が行った操作を確認します。コードの checkout、作業環境の作成、MCP server のセットアップ、タスク完了に向けた各種操作などが含まれます。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示される pull request (PR) ペインも確認してください。PR と変更されたファイルを確認でき、作業をさらに監視できます。

## cloud agent の作業を案内する

実行中のタスクを確認したので、ハイ コントラスト モードの作業にライト モードの切り替え機能も含めるよう依頼します。

1. ハイ コントラスト モードを追加するセッションを選択します。タイトルは Copilot の付け方や作業状況によって異なります。

    ![mission control の Accessibility セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始したことがわかるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次の prompt を入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot が作業中の agent task と、Steer active session while copilot is working ダイアログが表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押して prompt を送信します。
5. Copilot が prompt を受け取り、作業に追加したことを確認します。

## Copilot に作業を続けてもらう

Copilot は更新されたタスクに取りかかります。メッセージを送信した時点で作業中だった手順が終わると、新しい依頼が作業の流れに取り込まれます。

この作業にも数分かかります。これまで学んだ内容を振り返りながら待ちましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視して案内する中心的な場所である Copilot agents ページを確認しました。mission control では、進行中または完了したタスクを追跡し、実行中の作業をレビューして、進行中のタスクの範囲や指示を変更できます。

このレッスンでは、次のことを行いました。

- mission control とエージェント ページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、作成された pull request を[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [custom agent について][custom-agents]

---

| [← 前のレッスン: Custom agents][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
