---
title: "演習 5 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom agents][previous-lesson] |
|:--|

GitHub Copilot を agent mode で使うと、コードベースの調査、変更の提案、ファイルの編集、コマンドの実行などを自律的に行います。VS Code の Copilot Chat では、その作業がローカル環境でリアルタイムに進むため、tool 呼び出しやファイルの編集を順番に確認できます。差分をインラインでレビューし、変更ごとに採用または破棄できます。また、エディターを離れずに追加 prompt を送信して、作業を調整したり拡張したりできます。

この演習では、次のことを行います。

- アクセシビリティ agent の作業をリアルタイムで監視します。
- 提案されたファイル編集を確認して採用します。
- セッションの途中で agent に指示し、ハイ コントラストに加えてライト モードの切り替えを追加します。

## シナリオ

[演習 4][previous-lesson] で使ったアクセシビリティ custom agent は、ハイ コントラスト モードを実装しています。作業中、デザイン チームから、既定のテーマが暗すぎると感じるユーザー向けにライト モードも用意したいという追加要望がありました。最初から新しい会話を始める代わりに、既存のセッションに追加の作業を依頼します。

## Copilot の進捗を監視する

Copilot Chat ビューには、agent が調査するファイル、実行するコマンド、提案する編集が表示されます。ファイル編集は差分と **Keep** / **Undo** の操作とともにインラインで表示されます。

1. 演習 4 で使った Copilot Chat ビューに戻ります。
2. agent の作業中は chat をスクロールし、tool の操作を確認します。ファイルの読み取り、編集、実行を求められたターミナル コマンドなどが表示されます。
3. agent がファイル編集を提案すると、差分がインラインに表示されます。
    - **Keep** を選択すると変更を採用します。
    - **Undo** を選択すると変更を元に戻し、agent に再度取り組んでもらいます。
4. chat 上部の **Files changed** を開くと、agent がこのセッションで変更したファイルの一覧を確認できます。

> [!NOTE]
> ターミナル コマンドの実行などで許可が必要な場合、agent は **Allow** を表示して待機します。実行してもよいコマンドだけを許可してください。

5. agent がハイ コントラスト モードの実装を終えるまで待ちます。最後に、変更内容の概要が表示されます。

## agent にライト モードを追加するよう依頼する

ハイ コントラスト モードができたら、同じ会話でライト モードの切り替え機能を追加します。同じ chat session を続けることで、agent は前の作業で変更した component、スタイルの方法、設定の保存方法などのコンテキストを保ちます。

1. Copilot Chat の入力欄に次の追加 prompt を送信します。

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. agent はすでに行った変更を確認し、ライト モードの追加を提案します。
3. 変更ごとに差分を確認し、ハイ コントラストのときと同じように **Keep** または **Undo** を選択します。
4. agent が意図しない方向に進んだ場合（たとえば、気に入らない方法で toggle UI をリファクタリングする場合）は、chat 入力欄の横にある **Stop** ボタンで停止してから、追加の説明を送ります。

> [!TIP]
> 実行中のセッションに指示を追加すると、最初からやり直すより短時間で調整できます。追加 prompt で、変更（「CSS を簡素化して、selector の重複ではなく CSS 変数を使う」）、軌道修正（「3 つ目の toggle は追加せず、既存の設定パネルにライト モードを組み込む」）、範囲の拡張（「両方の toggle にキーボード ショートカットも追加する」）を依頼できます。

## 作業内容をレビューする

変更を commit する前に、agent が編集した内容を確認します。

1. VS Code の **Source Control** ビューを開きます。
2. 変更されたファイルの一覧を確認します。Astro component、スタイル、関連テストなどが含まれます。
3. 変更されたファイルをいくつか開き、差分を確認します。custom agent のアクセシビリティ方針に従い、ARIA 属性、キーボード操作、セマンティック HTML、local storage による設定の保持が実装されていることを確認します。
4. 問題がなければ、Source Control パネルで変更を stage して commit します。次のレッスンで公開します。

## まとめと次のステップ

Copilot Chat の agent mode で進行中のセッションを監視し、各ファイルの編集を確認して採用または破棄し、コンテキストを維持したまま追加 prompt で作業を拡張しました。

このレッスンでは、次のことを確認しました。

- agent の作業をリアルタイムで監視する。
- 個々のファイル編集を採用または破棄する。
- 実行中のセッションに追加 prompt を送り、作業を案内する。

[次のレッスン][next-lesson]では、アクセシビリティの作業を pull request として公開し、このラボ全体でローカルの custom agent が作成した変更をレビューします。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [agent mode を使う][agent-mode]

---

| [← 前のレッスン: Custom agents][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
