---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に合わせた人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` にある Markdown ファイルでエージェントを定義し、対象を絞った指示、ベストプラクティス、コードパターン、分野固有の知識を Copilot に提供できます。チームの専門知識を再利用できるエージェントとしてまとめられます。たとえば、Web Content Accessibility Guidelines (WCAG) 準拠を確認するアクセシビリティエージェント、安全なコーディング規約に従うセキュリティエージェント、一貫したテストパターンを守るテストエージェントを作成できます。

カスタムエージェントは、プロジェクト内の `.github/agents` またはグローバルな `~/.copilot/agents` に置いた Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML (YAML Ain't Markup Language) frontmatter を記載し、その後にエージェントの動作、専門知識、指示を記述します。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルやほかのエージェントを調整しながら作業します。このラボでは、アクセシビリティカスタムエージェントがサイトをアクセシビリティガイドラインに照らしてレビューし、更新します。その作業の一部として、プルリクエストワークフローのスキルや、テストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法は1つではありません。AI のほかの機能と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるようにしたいと考えています。現在のダークテーマは、テキストと背景色のコントラストが十分でないため、読みづらいという意見が寄せられました。デザインチームは、ユーザーが切り替えられる高コントラストモードの実装を依頼しました。

アクセシビリティは重要なため、カスタムエージェントを使って機能をすばやく実装します。

この演習では、次のことを行います。

- アクセシビリティ用のカスタムエージェントを確認します。
- カスタムエージェントにタスクを割り当てます。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ向けのカスタムエージェントはすでに用意されています。内容を確認し、Copilot をどのように導くか理解します。

codespace に戻り、アクセシビリティカスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主な責任。
   - アクセシビリティのベストプラクティス。
   - HyperText Markup Language (HTML)、Cascading Style Sheets (CSS)、JavaScript のコード例。
   - よくある問題や間違い。

## Issue を作成して割り当てる

Mission control は、環境のエージェントを操作する中心的な場所です。Copilot cloud agent にタスクを割り当て、進行状況を監視し、追加の指示で作業を導けます。まず、高コントラストモードの実装を Copilot に依頼します。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

   ![カスタムエージェントと Accessibility が選択された cloud agent 割り当て画面のスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を始めたことを確認します。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。特定のタスクや分野に合わせた専門 AI アシスタントです。チームの専門知識や規約を再利用できるエージェントにまとめ、Copilot が作業するときの指針にできます。

このレッスンでは、次のことを行いました。

- カスタムエージェントの定義方法を確認しました。
- エージェントにタスクを割り当てました。

Copilot が高コントラストモードを実装している間に、次は Mission control から[エージェントセッションを監視して方向付けます][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを利用する準備][org-custom-agents]
- [Enterprise でカスタムエージェントを利用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
