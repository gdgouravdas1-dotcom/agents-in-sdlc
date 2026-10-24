---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定の作業や分野に合わせた専門 AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、焦点を絞った指示、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に与えることで、特定の作業をより効果的に実行させられます。チームの専門知識を再利用可能なエージェントとして形式化できます。たとえば [WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディング規約に従うセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントを作成できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` に Markdown ファイルとして定義します。各ファイルには少なくとも `name` と `description` を持つ YAML フロントマターがあり、その後にエージェントの動作、専門分野、指示を定義する Markdown プロンプトが続きます。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義され、AI に作業方法を伝えます。簡単に言うと、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業の一部としてスキルや別のエージェントを指揮します。このラボのアクセシビリティ カスタム エージェントは、アクセシビリティ ガイドラインに照らしてサイトをレビューし、更新します。その際、プルリクエスト ワークフローやテストの実行・管理などのスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成方法に唯一の正解はありません。AI を使うほかの作業と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタム エージェントを使って次のことを学びます。

- カスタム エージェントの定義方法
- カスタム エージェントへのタスクの割り当て方

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるように取り組んでいます。最近、現在のダーク テーマは背景色と文字色のコントラストが十分でなく、読みづらいという利用者の声がありました。このアクセシビリティの課題に対応するため、デザイン チームはユーザーが切り替えられる高コントラスト モードを求めています。

アクセシビリティは重要なので、カスタム エージェントを使って機能をすばやく実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ向けのカスタム エージェントはすでに作成されています。Copilot を導く方法を理解するため、その内容を確認します。

Codespace に戻り、アクセシビリティ カスタム エージェント ファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` を含むフロントマターが必要です。

3. 続くセクションを確認します。アクセシブルな Web サイトを生成する際の主要な責任、アクセシビリティのベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある問題が記載されています。

## Issue を作成して割り当てる

Mission control は、環境内のすべてのエージェントを操作する場所です。Copilot cloud agent にタスクを割り当て、監視し、追加の指示を送れます。まず、高コントラスト モードの作成を Copilot に依頼します。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択します。
8. 右側の **Assign to Copilot** を選択します。
9. カスタム エージェント一覧から **Accessibility agent** を選択します。

   ![cloud agent 割り当て画面でカスタム エージェントとアクセシビリティが選択された状態](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を始めます。

## まとめと次のステップ

このレッスンでは、特定の作業や分野に合わせた専門 AI アシスタントである[カスタム エージェント][custom-agents]を確認しました。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot の作業を導けます。

次のことを学びました。

- カスタム エージェントの定義方法
- カスタム エージェントへのタスクの割り当て方

Copilot が高コントラスト モードを実装している間に、mission control から[エージェント セッションを監視して誘導][next-lesson]しましょう。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使う準備][org-custom-agents]
- [エンタープライズでカスタム エージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
