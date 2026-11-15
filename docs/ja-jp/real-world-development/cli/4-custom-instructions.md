---
title: "レッスン 4 - custom instruction で Copilot を導く"
description: "リポジトリの instruction を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを完了する必要があるなら、そのガイダンスを Copilot が参照できるようにします。[instruction file][instruction-files] では、必要なコードだけでなく、その構成方法も説明できます。フィルタリングを実装したので、Copilot が使った instruction を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの instruction と、パス単位で適用される instruction file がエージェントに届く仕組みを確認します。
- コーディング標準に沿うよう instruction file を更新します。
- instruction file がコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントには、コードの繰り返しではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` の export 関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。`db` 引数を注入できる場合は、その引数についても記述します。
- 再利用可能な Astro component には `Props` の仕様を記述します。また、関連コードが変更された際にコメントを最新の状態に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

instruction file を使って、これらのプラクティスに沿うタスクを Copilot が実行できるように情報を渡します。

## Instruction file

custom instruction を使うと、コンテキストや設定を Copilot に渡せるため、コーディング スタイルや要件をより正確に理解させられます。好みのコーディング規約、ライブラリ、コードに含めたいコメントの種類を指定し、関連性の高い提案やコード スニペットを得られます。instruction はリポジトリ全体に対しても、特定のファイル種別に対するタスク レベルのコンテキストとしても作成できます。

instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送られる単一の instruction file です。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けのガイドラインを提供します。

> [!NOTE]
> 環境によって、ほかの instruction 形式やサポート状況は異なります。特定の形式に依存する前に、[custom instruction のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトの custom instruction file を確認する

スターター プロジェクトには、すでに instruction file が用意されています。変更を加える前に、既存の内容を確認して、その効果を把握しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この instruction は、このリポジトリで Copilot を使うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro、Drizzle データ レイヤー、テストなどに関する instruction があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction file や既存のプロジェクト ファイルへのリンクを確認します。大きな instruction セットを小さく再利用しやすいファイルに分け、Copilot に例を示せます。

## チームのガイダンスに合わせて instruction file を更新する

既存のファイルはよい出発点ですが、追加したい標準があります。TypeScript を新規生成する際に TSDoc コメントを含めるよう、`copilot-instructions.md` を更新します。

1. `.github/copilot-instructions.md` で、35 行目前後にある **Code formatting guidance** セクションを探します。
2. 次の項目を、セクションの最後に追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの instruction を読み込みます。instruction を保存した後、フィルタリングの会話を終了して再開すると、機能のコンテキストを保ったまま新しいガイダンスを読み込めます。

1. フィルタリングの会話を開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチ上のリポジトリ ルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合は、フィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のプロンプトで、更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が instruction file からコンテキストを取得する仕組みを確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの instruction と、パス単位で適用される instruction file がエージェントに届く仕組みを確認しました。
- コーディング標準に沿うよう instruction file を更新しました。
- instruction file がコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` skill を[カスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI 用 custom instruction の追加][instruction-files]
- [custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
