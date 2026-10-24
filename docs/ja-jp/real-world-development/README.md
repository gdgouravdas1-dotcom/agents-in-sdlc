---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共通のバックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を体験します。変更を段階的に計画・実装し、動作をテストして、プルリクエストをレビューし、エージェントを活用して作業を管理します。

## シナリオ

架空の企業 Tailspin Toys に新しく加わった開発者として作業します。Tailspin Toys は、開発者をテーマにしたボードゲームのクラウドファンディングを提供しています。チームのバックログは GitHub Issue として登録済みなので、すぐに取り組めます。フィルタリングやページネーションなどの機能追加のほか、アクセシビリティやコーディング標準の改善も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、どの環境でも利用できます。自分に合った作業環境を選んでください。各ワークショップは個別のセットアップから始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、カスタム指示、スキル、カスタム エージェント、Playwright MCP、Agent Merge、実用的なスラッシュ コマンドを扱います。

### [GitHub Copilot app][app]

デスクトップ アプリでエージェント セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業し、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を案内し、進捗を監視して、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに Copilot Chat のエージェント モード、MCP サーバー、カスタム エージェントを活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
