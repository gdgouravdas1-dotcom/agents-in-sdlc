---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルタリング、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、ひとつのブランチに保存されています。まとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡略化するため、フィルタリングと品質のワークフロー全体をひとつの会話とブランチにまとめ、この PR に含めています。

このレッスンでは、次のことを行います。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びます。
- 機能全体の変更と検証結果を確認します。
- フィルタリング PR を作成します。
- レビュー後にのみ Agent Merge を有効にして、PR がマージされたことを確認します。

## シナリオ

フィルタリング作業を通じて、Copilot で機能を計画、実装、検証しました。Tailspin Toys は、マージの許可を開発者の管理下に置いたまま、PR に関する残りの作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** はプルリクエストを取り込むために必要な残りの作業を自動化します。有効にすると、Copilot は PR をブロックしている問題に対応します。失敗した継続的インテグレーション (CI) チェックを修正し、レビュー コメントに対応し、必要に応じてリベースします。その後、リポジトリが許可している場合は GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せられますが、有効にする前に内容をレビューして、準備ができていると判断してください。

## Agent Merge で PR を管理する

コードが揃ったので、まとめてレビューし、PR を作成して、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべてのチェックが成功するとマージを実行します。
7. Copilot から `PR #14 was squash-merged successfully.` のようなメッセージが表示されることを確認します。

> [!IMPORTANT]
> Agent Merge は必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、必要な権限を回避しません。ブロックされた場合は理由を確認し、リポジトリで可能であればレビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コード生成、テストと検証に加えて、プルリクエストのプロセスも自動化しました。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びました。
- 機能全体の変更と検証結果を確認しました。
- フィルタリング PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意の cloud agent への委任に使える[便利な Copilot CLI スラッシュ コマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
