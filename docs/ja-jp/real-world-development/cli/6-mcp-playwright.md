---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスを扱い、Copilot が利用できる自動化を追加する必要があります。そこで MCP サーバーを使います。MCP サーバーを使うと、Copilot CLI に組み込まれている機能に加え、ツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を学びます。
- Playwright MCP サーバーがまだ利用できない場合に追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を試します。

## シナリオ

ユニット テストやエンドツーエンド テストは重要ですが、UI の更新を検証するには実際に UI を操作する必要があります。Copilot にユーザーと同じように Web サイトを使わせることで、変更の自動化をさらに進め、意図どおり動作する確信を高めます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部ツールやサービスと通信する方法を提供します。MCP を使うと、AI エージェントはリアルタイムで最新情報にアクセスし、ユーザーに代わって操作できます。

これらのツールやリソースには、AI エージェントと外部ツールやサービスをつなぐ MCP サーバーを通じてアクセスします。各 MCP サーバーは、AI エージェントが利用できる別のツールとリソースを提供します。

よく使われる MCP サーバーをいくつか紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエスト管理用の API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザー自動化機能を利用できます。

ほかにも多くの MCP サーバーがあります。GitHub は見つけやすさとエコシステムへの貢献を促す [MCP registry][mcp-registry] をホストしています。

> [!CAUTION]
> MCP サーバーはプロジェクトのほかの依存関係と同様に扱ってください。利用前にソース コードを確認し、発行元を検証して、セキュリティ上の影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーのように Web サイトを操作できるよう追加します。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、セットアップをスキップして次に進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類 **STDIO** を確定し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

これまでは機能が期待どおりに動作することを手動で確認しました。今度は Copilot が利用できる Playwright MCP サーバーを使います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に指示します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、自分で MCP サーバーの利用方法を判断できるため、明示的に伝える必要はありません。使用するサーバーが分かっている場合は、指示すると結果が安定し、トークンを節約できます。

2. Copilot がブラウザーで機能を確認するために実行する手順を確認します。
3. 結果を読み、すべて期待どおりに動作するか確かめます。

Copilot はサーバーを起動し、Playwright で Web サイトを操作し、サーバーを停止して、結果を報告します。

## まとめと次のステップ

Playwright MCP サーバーを使い、Copilot CLI から実際のブラウザーで機能を確認しました。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を学びました。
- Playwright MCP サーバーがなければ追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を試しました。

次はスキルとブラウザー ツールを組み合わせる[QA カスタム エージェントを作成][next-lesson]します。

## リソース

- [MCP とは何か、なぜ話題なのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
