---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルタリング コードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際はコンテキストが重要です。特定の方法でタスクを実行したい場合、その指針を Copilot が利用できるようにします。[指示ファイル][instruction-files]は、コードの「内容」だけでなく「構成方法」も伝えます。フィルタリング機能を実装したので、Copilot が使った指示を調べ、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス別の指示ファイルがエージェントに伝わる方法を確認します。
- コーディング標準を守るよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインがあります。

- コメントはコードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数は、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。該当する場合は注入可能な `db` 引数も記載します。
- 再利用可能な Astro コンポーネントでは `Props` 契約を文書化し、関連コードの変更に応じてコメントを最新に保ちます。
- 既存のフォーマットと lint の指針を維持します。

指示ファイルを使い、Copilot がこれらのプラクティスに沿って作業するために必要な情報を提供します。

## 指示ファイル

カスタム指示は、Copilot がコーディング スタイルや要件を理解するためのコンテキストや好みを伝えます。推奨する規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定のファイル形式に適用できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリ内のすべての依頼で Copilot に送信される単一の指示ファイルです。ほとんどの依頼に関係するプロジェクト レベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル形式、作業向けのガイドラインです。

> [!NOTE]
> 環境によって、ほかの形式の指示への対応状況は異なります。形式に依存する前に[カスタム指示のサポート状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング指針を確認します。これらは、このリポジトリで Copilot を使うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどのファイルを確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` を確認します。指示の適用対象を決める glob です。
6. `.github/instructions/drizzle.instructions.md` のリンク先にある指示ファイルや既存プロジェクト ファイルを確認します。大きな指示セットを小さく再利用可能なファイルに分け、Copilot に例を示せます。

## チームの指針に合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、改善の余地があります。TypeScript に TSDoc コメントを追加するよう、コアの `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` の 35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新した指針を適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示の保存後、フィルタリングの会話を終了して再開すると、新しい指示を読み込んだ状態で機能のコンテキストを保てます。

1. フィルタリングの会話が開いたままの場合は `/exit` を入力して閉じます。Copilot CLI は会話を自動保存します。
2. `game-filters-cli` ブランチのリポジトリ ルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたらフィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. 更新した指示を適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しく追加された TSDoc コメントがコードを正しく説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。

- リポジトリの指示とパス別の指示ファイルがエージェントに伝わる方法を確認しました。
- コーディング標準を守るよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は[再利用可能な quality-checks スキルをカスタマイズして実行][next-lesson]し、lint とテストを一貫して実施します。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート状況][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [awesome-copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
