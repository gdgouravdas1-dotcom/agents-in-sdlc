---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーからフィルタリングを確認しました。次はこれらの機能を、明確な QA プロセスを持つカスタム エージェントにまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが指示、スキル、MCP ツールを使う方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルタリング Issue と照らした結果をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に要件、コード品質、自動チェック、テスト範囲、ブラウザー上の動作を一貫して確認したいと考えています。カスタム エージェントを使うと QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の専門バージョンです。目的、指示、利用可能なツールを記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまで使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチーム標準を説明します。`quality-checks` スキルは繰り返し可能なチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、それらを使って要件を評価し、結果を報告する方法を Copilot に指示します。これらを置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能 PR を開く前に、再利用可能な QA プロファイルを作成するよう Copilot に依頼します。QA が行うチェックと従う制約の両方を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに次のカスタム エージェント作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

エージェントを使う前に、Copilot が意図した QA ワークフローと制約を設定したことを確認します。検証だけを依頼したのに、エージェントが不完全または過度に広い権限で実装を変更するのを防ぎます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを読みます。`description` は必須です。`name` は省略できますが、含めるとエージェントの表示名が明確になります。
3. プロファイルの指示を読み、要件を起点にし、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使うことを確認します。
4. 各要件について根拠とともに結果を報告し、実装コードを変更する前に確認を求め、変更をコミットしたり PR を開いたりしないことを確認します。
5. これらの責任や制約が不足している場合は、続行する前に既定のエージェントに修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話開始時にプロジェクト エージェントを読み込みます。プロファイルを作成した後に同じフィルタリングの会話を再開し、QA を選択すると、Issue と計画の決定を引き継げます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントは利用可能な一覧に表示されない場合がありますが、上記のコマンドで有効にできます。

2. 次のプロンプトで機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 完了したらレポートを確認します。

## まとめと次のステップ

再利用可能な専門家の役割を追加し、その作業をレビューしました。

- カスタム エージェントが指示、スキル、MCP ツールを使う方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルタリング Issue に対する結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証結果が揃いました。次は[機能 PR にまとめて Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用カスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
