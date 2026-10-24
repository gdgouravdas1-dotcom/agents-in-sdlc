---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、関連リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローを進めました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを調べ、用意されたフィルタリング Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリング Issue から始め、Plan モードで方法を決め、Autopilot モードで構築し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを試しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、テスト範囲、スキルの実行結果、ブラウザーの証拠を評価しました。
- フィルタリングの変更全体をレビューし、フィルタリング PR で Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意の cloud agent への委任に使うスラッシュ コマンドを確認しました。

## リリースした内容

このワークショップでは、更新済みの `main` から分岐した別々のブランチで、2 つの PR を作成します。

1. **星評価:** 既存の `starRating` と、評価がない場合の明示的な表示をゲーム カードに追加します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連テストを追加します。

フィルタリングの計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、作業をひとつの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー操作など、複数の方法でコードを検証しました。`quality-checks` スキルは、ユニット テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。PR の前には QA が要件とテスト範囲のレビューを組み合わせました。

テストは実際の不足を補うために追加します。新しいテストを必要としない QA の結果も正しい場合があります。マージを許可する前にコードと証拠をレビューし、変更後には影響を受ける証拠を再確認します。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが作業を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。カスタマイズを会話間で再利用し、チームのニーズに応じて調整してください。指示は標準を設定し、スキルは繰り返し可能な作業を説明し、カスタム エージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの概要だけでなく、実際の変更とツール結果を確認してください。

タスクに合った**モードとモデル**を選びます。構築前の検討には **Plan**、焦点を絞った変更を対話的に進めるには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同様に、コンテキストも重要です。何を、なぜ、どのように作るかを明確に伝えると、出力が大きく変わります。

## さらに試すこと

主要なワークフローを学びました。ほかにも次の CLI 機能があります。

- `/review` でコード レビュー エージェントに変更を分析させる。
- `/rubber-duck` で問題を話し合い、別の視点を得る。
- `/fleet` で独立したサブタスクを並行して調整する。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送信する。

## 次のステップ

ツールを上達させるには、使い続けるのが一番です。実運用のコード、趣味のコード、ずっと作りたかった小さなアプリなどに使ってみてください。学んだことをチームと共有し、ほかの人からも学びましょう。ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張するには、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
