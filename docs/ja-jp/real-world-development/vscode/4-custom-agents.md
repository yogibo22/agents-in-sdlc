---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` にある Markdown ファイルでエージェントを定義し、対象を絞った指示、ベストプラクティス、コードパターン、分野固有の知識を Copilot に提供できます。チームの専門知識を再利用できるエージェントとしてまとめられます。たとえば、Web Content Accessibility Guidelines (WCAG) 準拠を確認するアクセシビリティエージェント、安全なコーディング規約に従うセキュリティエージェント、一貫したテストパターンを守るテストエージェントを作成できます。

カスタムエージェントは、プロジェクト内の `.github/agents` またはグローバルな `~/.copilot/agents` に置いた Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter を記載し、その後にエージェントの動作、専門知識、指示を記述します。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルやほかのエージェントを調整しながら作業します。この演習では、アクセシビリティカスタムエージェントがサイトをアクセシビリティガイドラインに照らしてレビューし、更新します。その作業の一部として、プルリクエストワークフローのスキルや、テストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法は1つではありません。AI のほかの機能と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるようにしたいと考えています。現在のダークテーマは、テキストと背景色のコントラストが十分でないため、読みづらいという意見が寄せられました。デザインチームは、ユーザーが切り替えられる高コントラストモードの実装を依頼しました。

アクセシビリティは重要なため、カスタムエージェントを使って機能をすばやく実装します。

この演習では、次のことを行います。

- 既存のアクセシビリティカスタムエージェントを確認します。
- Copilot Chat のアクセシビリティエージェントで高コントラストモードを実装します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ向けのカスタムエージェントはすでに用意されています。内容を確認し、Copilot をどのように導くか理解します。

codespace に戻り、アクセシビリティ作業用の新しいブランチを `main` から作成して切り替えます。これにより、演習 3 のフィルター機能 PR を分けておけます。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主な責任。
   - アクセシビリティのベストプラクティス。
   - HyperText Markup Language (HTML)、Cascading Style Sheets (CSS)、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント選択メニューには、`.github/agents` に定義したカスタムエージェントが表示されます。エージェントを選択すると、そのエージェントの指示とツールに沿って会話できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

1. Chat ビューのエージェント選択メニューで、まだ選ばれていない場合は **Agent** を選びます。

   ![Chat ビューのエージェント選択メニューのスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント選択メニューを選びます。現在のエージェントが表示されます。既定では **default** です。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティエージェントに次のプロンプトを送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクトファイルに適用します。各編集はファイルパスと差分とともにチャット内に表示されます。
6. エージェントが変更した作業セットに合わせて、**Files changed** の表示が更新されることを確認します。

> [!NOTE]
> この作業には数分かかることがあります。Copilot はリポジトリを実際に変更し、Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集します。

次の演習では、進行中の作業をレビューして方向付けします。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]は、特定のタスクや分野に合わせた専門 AI アシスタントです。カスタムエージェントを使うと、チームの専門知識や規約を再利用できるエージェントにまとめ、Copilot が作業するときの指針にできます。

このレッスンでは、次のことを確認しました。

- カスタムエージェントの定義方法。
- Copilot Chat のエージェントモードでカスタムエージェントを使う方法。

次は、エージェントの作業を[監視して方向付けます][next-lesson]。変更を確認し、同じセッションにライトモードのトグルを追加するよう依頼します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [統合開発環境 (IDE) でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
