---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルタリング機能を検証するために使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことだけが開発ではありません。これまでは、コードが動作することを手動で確認し、指示ファイルを使って標準に従うようにしてきました。テストや lint、継続的インテグレーション (CI) に含まれるその他の作業はどうでしょうか。

このようなタスクには **agent skill** が適しています。スキルを使うと、テストなどの操作を適切に実行する方法を Copilot に理解させることができます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` スキルを使用しています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

Agent skill は、再利用可能なタスクの指示、実行可能なスクリプト、エージェントが必要に応じて読み込む補助リソースをまとめたものです。基本的には、スキル名のフォルダーと、その中にある `SKILL.md` ファイルで構成されます。Markdown ファイルには、スキルの名前と説明 (`name` と `description`) を定義する frontmatter、スキルの概要、呼び出すタイミングや実行方法のガイダンスが含まれます。スキルのフォルダーには、呼び出されたときに使用するスクリプトやその他のリソースを含めることもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにすでにあるコマンドを使用するため、`SKILL.md` のみで構成されています。

スキルは、チームで共有して再利用できるリポジトリの `.github/skills` フォルダー、またはユーザー スキル用の `~/.copilot/skills` フォルダーに配置できます。

## スキルを確認する

Tailspin Toys チームが単体テスト、lint、型チェックを実行するために作成した `quality-checks` スキルを確認します。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 冒頭にある `name` と `description` を読みます。`description` は、Copilot がスキルをいつ呼び出すかを判断するのに役立ちます。
3. 指示を読み、テストと lint の手順がどのように案内されているか確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

Copilot CLI から直接、または自然言語でスキルを呼び出せます。Copilot に 3 つのチェックを実行するよう依頼します。

1. 対話モードでフィルタリングの会話に戻ります。
2. 次のプロンプトを入力します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行した内容と成否に加えて、ツールが報告した内容もわかるレポートに改善しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

Copilot CLI は、会話を再起動せずに編集済みのスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されたレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使用しました。このレッスンでは、次のことを行いました。

- 単体テスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果の形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、フィルタリング機能とともに feature PR に含まれます。次は [Playwright MCP サーバーを使って][next-lesson]、Copilot がサイトを直接操作できるようにします。

## その他のスキルの例

以下のコミュニティの例は参考資料であり、追加の演習ではありません。

- [Agent Skills の仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図と同梱のエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
