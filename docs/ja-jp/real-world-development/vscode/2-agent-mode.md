---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。必要なファイルを探し、変更し、テストを実行し、問題をデバッグして、変更が漏れたファイルを見つけてさらに更新する作業が続きます。

このような作業に役立つのが Copilot エージェントモードです。

エージェントモードは IDE 内でより自律的に作業します。開発者と同じように、まず既存のプロジェクト構成を調べ、必要なファイルを更新し、テストなどのタスクを実行して、発見した問題を自動的に修正します。エージェントモードを使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途向けです。エージェントモードは IDE 内で作業し、素早くフィードバックを受けて対話できます。cloud agent はチームメンバーのように非同期で作業し、Issue とプルリクエストを通じて開発者とやりとりします。

この演習では、次の方法を学びます。

- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定して、複数の変更を連携させます。
- UI とデータ層にまたがる新機能を実装します。
- 変更をマージする前に、Copilot が生成した変更とテストをレビューします。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと発売元で絞り込めるようにします。前の演習では publishers ヘルパーを追加しました。次は、エージェントモードを使って残りのデータ層、ユーザーインターフェイス (UI)、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームを一覧表示し、詳細を確認できます。単一の Astro アプリで、ビルド時にページを静的な HyperText Markup Language (HTML) として生成します。ページは frontmatter 内から Drizzle ORM を通じてローカルの SQLite データベースを直接参照します。独立したバックエンドのアプリケーションプログラミングインターフェイス (API) やクライアントサイドのユーザーインターフェイス (UI) フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、必要なインタラクティブ機能には標準の Document Object Model (DOM) API を使う小さな Astro `<script>` を使用します。

### Web サイトを起動する

`npm run dev` タスクで、単一の Astro 開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. 前の演習で使った Codespace に戻ります。`main` ブランチにいることを確認します。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押し、新しいターミナルを開きます。
3. `main` を更新し、フィルター機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を含むバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Web サーバーをホストしている Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、主な機能を確認します。

- **Home Page**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **Game Details Page**: ゲームを選択すると、タイトル、説明、発売元、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加します。テンプレートからプロジェクトを作成すると Issue のバックログも自動的に追加されます。Copilot にバックログを表示するよう依頼します。

1. Chat ビューのエージェント選択メニューで **Agent** を選びます。エージェントモードは変更を計画し、複数のファイルを編集し、ターミナルコマンドを実行してツールを利用できます。

   ![Chat ビューのエージェント選択メニューのスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデル一覧から **Claude Sonnet 4.5** を選びます。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップ作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高める意図がありますが、大規模言語モデル (LLM) は確率的に動作するため、説明と異なる提案が表示されることがあります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は別の MCP コマンドを使うことがありますが、タスクを完了できるはずです。

3. 次のプロンプトを Copilot に送ります。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. すべての Issue を一覧表示するコマンドを実行するため、**Continue** を選択します。
5. 生成された Issue の一覧を確認します。

Copilot はユーザーに最も役立つと思われる項目を優先順位付けします。

## 指示ファイルを確認する

コード生成をエージェントに依頼する前に、作業のコンテキストを与える指示ファイルを確認します。[user interface (UI)][ui-instructions] ファイルには、Web サイトの機能追加に関する指針があります。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能を追加する方法に関する全体的な指針を確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle など、ファイル形式ごとの指示へのリンク。

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用し、大きく複雑な内容を小さく分け、Copilot に例やテンプレートを示せます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の3種類の変更が必要です。

- データ層 (`src/lib/`) の絞り込みロジックを追加または調整します。
- フィルターの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。

コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot エージェントモードにこれらの作業を依頼します。

1. Copilot の現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルの選択例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択メニューで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニューのスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログ内の関連 Issue に基づき、次のプロンプトで実装を依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、関連ファイルを見つける様子を確認します。データ層のヘルパー、UI、テストを特定し、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトのファイルを調べている様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調べ、変更を行い、テストを実行します。コードベースの複雑さによっては数分かかります。処理中にコードエディターに **Keep** と **Undo** が表示されることがあります。作業中に選択する必要はありません。Copilot が完了すると、変更ごとに **Keep** または **Undo** を選べます。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat パネルでテスト実行の確認を求めるダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。Copilot はコード生成とテストを繰り返し、エラーを検出しなくなるまで作業します。

   ![Copilot エージェントモードの完了した Chat セッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを実行しているブラウザーに戻り、新機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェントモードを使って、Tailspin Toys の Web サイトに機能を追加しました。この演習では、次のことを学びました。

- UI とデータ層にまたがる新機能をエージェントモードで実装しました。
- エージェントモードでプロジェクトを調べ、関連ファイルを特定し、複数の変更を連携させました。
- コードベースにマージする前に、生成された変更とテストをレビューしました。

次は、[Playwright の Model Context Protocol (MCP) サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習 - ページ分割を実装する

ゲーム一覧が増えるにつれて、ページ分割が必要になります。この演習で学んだことを使って、Copilot にページ分割の実装を依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従います。
- ページサイズをユーザーが選ぶのか、固定にするのかなど、ページ分割の方法を決めます。
- 希望する実装に必要な情報をプロンプトで Copilot に伝えます。
- Copilot に変更を依頼し、コンテキストを追加しながら反復することがあります。これは Copilot を使う通常の開発フローです。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
