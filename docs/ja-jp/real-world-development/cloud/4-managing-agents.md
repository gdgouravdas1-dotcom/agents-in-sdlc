---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習で Copilot cloud agent に、ユーザー エクスペリエンスの改善や機能追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

GitHub.com の[エージェント ページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがいくつかあります。この mission control では、オープンなプルリクエスト (PR) があるエージェント タスクを確認できます。実行内容を調べ、進行中のセッションに指示を送ることもできます。

このレッスンでは、次のことを行います。

- エージェント ページで cloud agent のタスクを監視します。
- 実行中のセッションに追加機能を依頼します。

## シナリオ

エージェントに高コントラスト モードを作成させた後、チームはライト モードも追加することにしました。サイトのスタイルと切り替え機能をすでに更新しているため、同時に実装するのが自然です。高コントラスト モードだけでなく、ライト モードも追加するようエージェントの作業を誘導します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左ペインにタスク一覧が表示されます。割り当てた次のタスクなどがあることを確認します。
   - コードベースのドキュメント更新
   - ゲーム変更用 API の生成
   - Web サイトへの高コントラスト モード追加
3. 実行中のタスクをひとつ選択し、Copilot が行った作業を確認します。次のような内容が含まれる場合があります。
   - リポジトリのチェックアウト
   - Copilot が作業する環境の作成
   - MCP サーバーのセットアップ
   - 割り当てられたタスクを完了するためのさまざまな手順

> [!NOTE]
> 表示される手順は、Copilot の作業状況や選択した方法によって異なります。

4. 右側の PR ペインも確認します。ここから PR と変更ファイルを確認し、追加で監視できます。

## cloud agent を誘導する

実行中のタスクを確認できたので、高コントラスト モードに取り組んでいる Copilot に、ライト モードの切り替えも追加するよう依頼します。

1. 高コントラスト モードを追加しているセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なります。

   ![mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セッションを数分確認し、セットアップを終えて作業を開始したことを示すまで待ちます。次のようなメッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot エージェント ページで、作業中のセッションへの指示ダイアログが強調表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が依頼を受け取り、作業に含めたことを確認します。

## Copilot に作業を続けてもらう

Copilot は更新された依頼に取り組みます。メッセージを送信した時点で実行していた手順が終わると、新しい要望を作業の流れに取り込みます。

作業には数分かかります。その間に、これまで学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent タスクを監視・誘導するための中心的な場所である Copilot agents ページを確認しました。実行中または完了済みのタスクを追跡し、作業内容を確認し、実行中のタスクの範囲変更や追加指示を行えます。

次のことを学びました。

- mission control とエージェント ページで cloud agent のタスクを監視する
- 実行中のセッションに追加機能を依頼する

Copilot がアクセシビリティ機能を実装している間に、作成されたプルリクエストを[反復的に改善][next-lesson]します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復的に改善 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
