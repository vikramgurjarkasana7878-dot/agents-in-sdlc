---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つの pull request、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys で一連の開発ワークフローを進めました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、用意されていたフィルタリング issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初の pull request (PR) を手動でマージしました。
- フィルタリング issue から始めて、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードで結果をレビューしました。
- custom instruction でエージェントを導き、既存の `quality-checks` skill をカスタマイズして、unit test、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) server を追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) custom agent を作成して選択し、要件、カバレッジ、skill の結果、ブラウザーでの検証結果を評価しました。
- フィルタリングの変更全体をレビューし、Agent Merge にフィルタリングの PR を任せました。
- slash command を使って、コンテキスト、モデル、共有、任意の cloud への委任を確認しました。

## リリースしたもの

ワークショップには、更新された `main` からそれぞれ作成した 2 つの PR があります。

1. **星評価:** 既存の `starRating` と、評価がないことを示す状態をゲーム カードに表示します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、instruction を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルタリングの計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップの手順を簡潔にするため、これらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を介した Copilot のブラウザー操作でコードを確認しました。`quality-checks` skill は unit test、lint、型チェックを実行し、カスタマイズした形式で結果を報告しました。QA agent は PR の前に、要件とテスト カバレッジをレビューして結果をまとめました。

追加したテストは、実際の不足を埋めるものにします。新しいテストが不要と判断される QA の実行も適切な場合があります。マージを承認する前にコードと検証結果をレビューし、変更後は影響を受ける検証結果を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストと tool が、その作業に影響します。このワークショップでは、instruction を更新し、skill をカスタマイズし、QA プロファイルを作成し、MCP server を設定しました。会話をまたいでこれらのカスタマイズを再利用し、チームのニーズに合わせて調整してください。instruction は標準を設定し、skill は繰り返し行う作業を説明し、custom agent は専門的な役割を定義し、MCP server は外部 tool に接続します。エージェントの要約だけでなく、実際の変更と tool の結果をレビューしてください。

タスクに合った **モードとモデル** を選びます。実装前の検討には **Plan**、範囲を絞った変更を一緒に進めるには **Interactive**、明確にスコープが定まった作業には **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

基盤と同じくらい、コンテキストも重要です。何を、なぜ、どのように作りたいかを明確に伝えると、出力が大きく変わります。

## さらに確認する

基本的なワークフローを学びました。Copilot CLI には、ほかにも確認しておきたい機能があります。

- `/review` で、code review agent に変更の分析を依頼します。
- `/rubber-duck` で、問題について話し合い、別の視点を得ます。
- `/fleet` で、独立したサブタスクを並行して実行します。
- `/worktree` で、別のタスクを独立した環境に分けます。
- `/delegate` で、タスクを Copilot cloud agent に渡します。

## 次のステップ

どのツールでも、上達する一番の方法は使い続けることです。本番コード、趣味のコード、何年も前から考えていたけれどまだ作っていない小さなアプリなどで活用してください。学びをチームと共有し、チームからも学びましょう。いつものように、ドキュメントも確認してください。

ターミナルから Tailspin Toys をさらに拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境も試す場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI で pull request を管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
