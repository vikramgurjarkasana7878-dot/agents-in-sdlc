---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを通して、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用しましょう。変更の計画、コードの作成、動作のテスト、pull request のレビュー、エージェントを使った作業管理を反復して行います。

## シナリオ

あなたは、開発者向けテーマのボードゲーム クラウドファンディングを運営する架空の企業 Tailspin Toys に、新しい開発者として参加しました。チームのバックログは GitHub issue として登録済みで、すぐに着手できます。フィルタリングやページネーションなどの機能開発に加えて、アクセシビリティやコーディング標準の改善も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、普段使っている環境で利用できます。開発方法に合った環境を選択してください。どのワークショップも専用のセットアップから始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、instruction、skill、custom agent、Playwright MCP、Agent Merge、実用的な slash command を扱います。

### [GitHub Copilot app][app]

デスクトップ app でエージェント セッションを並行して実行し、セッション モードを切り替え、canvas で共同作業を行い、GitHub issue と pull request を管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期エージェントに割り当て、作業を案内して進捗を監視し、作成された pull request をレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに Copilot Chat の agent mode、MCP server、custom agent を利用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
