---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: カスタム指示][previous-lesson]

多くの組織では、セキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能など、技術的負債に直面しています。GitHub Copilot cloud agent は、コードの更新や機能追加などのタスクを自律的に実行します。作業が終わると、人間の開発者がレビューできるドラフトのプルリクエスト (PR) を作成します。時間のかかるタスクを委任し、開発者がより大きな課題に集中できるようにします。

このレッスンでは、Copilot cloud agent で次のことを行います。

- コード生成環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- 範囲を明確にした Issue の重要性を学びます。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には解消したい技術的負債があります。最初のサイトを開発したチームは、コードのドキュメントをほとんど残していません。まず、アプリケーションのエクスポート関数すべてに TSDoc コメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示し、利用者がほかのゲームを見つけられるようにします。現時点では完成された実装ではなく、受け入れテストに利用できる機能を求めています。優先度の高いほかの Issue もあります。

どちらも、優先順位が下がりやすいタスクです。Copilot cloud agent に割り当てれば、開発者が別の作業を進めている間に非同期で処理し、後から結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドで作業できます。たとえば、[GitHub Issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると、Copilot は進行状況を追跡するドラフト PR を作成し、環境を準備してタスクに取りかかります。実行中または完了後にセッションを確認できます。提案された解決策のレビュー準備ができると、PR で通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく見ると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に進め、学び、試し、状況に応じて調整します。生成 AI が加わっても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、Copilot を導くカスタム指示について学びました。まだ `.github/copilot-instructions.md` を確認していない場合は、ここで読み取り専用で確認してください。次の演習で cloud agent に Issue を割り当てた後、指示の影響を確認します。

1. リポジトリの GitHub Web UI または起動済みの codespace で、次のファイルを開きます。
   - `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc コメントに関する指針を確認します。
   - `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter が `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで **Code lacks documentation** Issue を Copilot cloud agent に割り当てたら、作成された PR に TSDoc コメント、TypeScript の規約、ファイルヘッダーが反映されているか確認します。後の演習で PR をレビューするときに、改めて取り上げます。

## Copilot cloud agent の開発環境を設定する

コードの作成には、通常、ツールと依存関係を準備する環境が必要です。Copilot にタスクを割り当てる場合も、ソフトウェアエンジニアのように作業するための準備が必要です。

cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に[専用のセットアップワークフロー][setup-workflow]を作成すると、作業前にツールや依存関係を準備できます。このラボのためにワークフローはすでに設定されており、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用ブラウザー、単一 Astro アプリ用にマイグレーションとシードを行った SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*に必要な最小限の権限を設定します。Copilot の操作には Copilot 専用のトークンが使われます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカルの SQLite データベースをマイグレーションしてシードし、ビルドとテスト用のデータを用意します。
      - name: Set up the database
        run: npm run db:setup
```

GitHub Actions のワークフローと同じように見えますが、いくつか重要な点があります。

- `copilot-setup-steps` という1つのジョブがあります。Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するまで待たずに、正常に動作するか試せます。

## ドキュメントを追加する

ドキュメントは重要ですが、情報が古かったり、まったくなかったりするプロジェクトもあります。これは生産性を下げ、コードベースの保守や新しいメンバーの受け入れを難しくする技術的負債です。Copilot はドキュメントの作成にも役立ちます。まず、コードにドキュメントを追加する Issue を cloud agent に割り当て、後の演習で結果をレビューします。

1. 新しいブラウザータブで github.com の自分のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成された PR を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後に Copilot の TODO リストが表示されたことを確認します。

> [!NOTE]
> Copilot の TODO リストが PR に表示されるまで数分かかる場合があります。前の手順で説明したワークフローを実行して環境を準備し、プロジェクトを分析して、作業方法を検討しているためです。

13. TODO リストを確認し、Copilot が実行するタスクを確認します。
14. PR のタイムラインを下に移動し、Copilot が Issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 状態の更新を確認するため、画面の再読み込みが必要な場合があります。

16. ライブセッションを確認します。Copilot がコードを調べて状態を理解し、計画を考え、コードを作成する様子を確認できます。

数分かかることがあります。cloud agent の利点は非同期で作業を進められることです。別の作業を行っている間に、もう1つのタスクも Copilot cloud agent に割り当てます。

## ゲーム詳細ページに関連ゲームを追加する

Copilot cloud agent に作業を分けて任せると、自分は別のタスクに集中できます。デザインチーム向けに関連ゲームを追加する作業を cloud agent に割り当てます。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

Issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことを示します。

![Copilot が Issue の作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。完了後に結果をレビューできます。次は、カスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]を使って、Issue を Copilot に割り当て、非同期で作業させる方法を学びました。cloud agent は、技術的負債の解消、新機能の作成、フレームワーク間のコード移行などに利用できます。

このレッスンでは、次のことを行いました。

- コード生成環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- 範囲を明確にした Issue の重要性を学びました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも利用できます][cloud-agent-mcp]。前の演習で扱ったカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
