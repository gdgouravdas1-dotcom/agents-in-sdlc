---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキスト管理、モデル選択、セッション共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

一般的な CLI ツールと同様に、GitHub Copilot CLI には操作用のスラッシュ コマンドが多数あります。高度な機能、内部情報、追加設定を利用できます。すでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で Copilot が会話コンテキストを管理する方法を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認します。

## シナリオ

CLI の主要なワークフローを完了しました。ここでは、コンテキスト管理、モデルの切り替え、セッション共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を見ていきます。

## Copilot CLI のコンテキストを確認する

大規模または複雑な作業では、モデルの最大コンテキスト ウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュ コマンドで自分で確認したり、圧縮したりすることもできます。

1. Codespace に戻り、まだ起動していない場合はリポジトリ ルートから Copilot CLI を起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き容量へのコンテキストの分割を確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. 再び `/context` を入力して結果を比較します。会話が短い場合、大きな変化がないこともあります。

> [!NOTE]
> Copilot CLI はコンテキスト ウィンドウが埋まると自動的に圧縮します。圧縮するタイミングを選びたい場合は `/compact` を使います。関係ない作業に切り替えて新しい会話にしたい場合は `/clear` または `/new` を使います。

## モデルを選ぶ

モデルによって得意分野が異なり、開発者によって好みも違います。Copilot CLI ではモデルの一覧表示と選択ができます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用量の情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、AI ツールの活用を改善できます。`/share` コマンドでは、Markdown または HTML ファイルとしてセッションをエクスポートし、共有リンクを作成し、GitHub gist として公開できます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、たとえばローカル Markdown にエクスポートする `/share file` など、目的に合った方法を選びます。
3. エクスポートした内容を送信または公開する前に確認します。セッションのエクスポートにはプロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を並行化または委任する

ワークショップの主要部分は完了しました。Copilot CLI には大きな作業向けのコマンドもあります。

- `/fleet` は独立したサブタスクを複数のサブエージェントに分け、並行して実行できます。
- `/worktree` は別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は非同期で作業し、プルリクエストを作成する場合もある Copilot cloud agent にタスクを送信できます。

これらのコマンドは追加の worktree やリモート作業を作る場合があるため、任意です。使用する場合は範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューします。非同期エージェントの作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、設定、共有、内部動作の確認ができます。

- `/context` と `/compact` で Copilot が会話コンテキストを管理する方法を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認しました。

Copilot CLI にはほかにも多くのスラッシュ コマンドがあります。最後に[学んだこと][next-lesson]と、学習を続けるための次のステップを確認しましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
