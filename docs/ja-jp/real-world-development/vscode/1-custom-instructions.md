---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で行いたい場合や、Copilot に背景情報を伝えたい場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]は、コード生成の指針を Copilot に伝える方法の1つです。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスを対象とする指示が、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約を Copilot に伝える方法を確認します。
- 現在の指示を使って、フィルター機能のデータ部分となる publishers ヘルパーを生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する規約を追加します。
- 同じプロンプトを再実行し、生成コードが新しい規約に従う様子を確認します。
- 指示の更新とフィルター機能の一部を `main` にコミットし、次の演習で更新後の指針を使えるようにします。

> [!CAUTION]
> 生成コードが設定した規約の一部に従わない場合があります。Copilot は確率的に動作します。この演習では、出力を一字一句一致させるのではなく、指示の更新による傾向の変化を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインがあります。

- データ層には必ずユニットテストを用意します。
- ユーザーインターフェイス (UI) はダークモードで、現代的な見た目にします。
- コードのドキュメントとして TSDoc コメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらのガイドラインに沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示は、コーディングスタイルや要件を Copilot に伝えるコンテキストです。プロジェクト全体にも、特定のファイル形式やタスクにも指示を設定できます。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に渡されます。技術スタック、プロジェクトの概要、ベストプラクティスなど、プロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル形式、タスクに指針を設定します。たとえば、TypeScript、Astro、UI コンポーネント、ユニットテスト向けの指示を記載できます。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方をコード生成時に使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。詳細は[カスタム指示の対応状況][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

サンプルプロジェクトには、指示ファイルの実用的な構成例があります。概要は次のとおりです。

- `copilot-instructions.md` には、構築するもの、プロジェクト構成、全体に適用するコーディング規約など、プロジェクトレベルの情報を記載します。
- `*.instructions.md` には、ファイル形式 (ユニットテスト、Astro コンポーネント、データ層など) や特定のタスク向けの指示を記載します。
- 自然な言葉を使い、分かりやすい指針とコードのよい例・悪い例を示します。

指示ファイルの唯一の正解はありません。プロジェクトで試しながら、最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示があります。
>
> Copilot に指示ファイルの生成を依頼することもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、環境によって操作が異なります。必要な場合は、該当する環境のレッスンで手順を説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## プロジェクトのカスタム指示ファイルを確認する

リポジトリには、中心となる `copilot-instructions.md` と、タスクごとの `*.instructions.md` ファイルが含まれています。エディターまたは GitHub の Web ユーザーインターフェイス (UI) で確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指針もあります。これらは Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ層、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` は、リポジトリのルートからの glob で適用対象を指定します。たとえば、`**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾まで読みます。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内のファイルへのリンクを確認します。これらのパスはリポジトリのルートではなく、指示ファイルの位置からの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約がありますが、コード内のドキュメントに関する規約はまだありません。次の手順で TSDoc コメントとファイルヘッダーのルールを追加します。

## 変更用のブランチを作成する

変更を加える前に、ベストプラクティスに従って専用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

指示ファイルの効果を確認するため、まず現在の指示を使ってプロンプトを送信します。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習で使ったエディターのタブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に知らせます。
3. Chat ビューのエージェント選択メニューで **Agent** を選び、ファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニューのスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べてコードを更新する様子を確認します。ヘルパーファイルとテストなど、複数のファイルが変更されることがあります。
6. 提案されたヘルパーが、`db` クライアントを最初の引数として受け取り、型付きの出版社配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層の規約に基づいています。
7. 提案されたコードに TSDoc コメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示しなくても TSDoc コメントを追加する場合があります。その場合も問題ありません。指示を更新した後の一貫性の改善が、この演習の目的です。

## リポジトリ全体に適用する規約を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を伝えるファイルです。コード提案を改善するために、リポジトリのコーディング規約を追加します。

1. `.github/copilot-instructions.md` を開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。既存の規約はありますが、先ほどのヘルパーにドキュメントコメントがなかった理由として、コード内ドキュメントのルールがないことを確認します。
3. 既存の規約の直後に、ファイルヘッダーと TSDoc コメントを求める次の行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで学んだように、指示ファイルはリポジトリ全体向けの `.github/copilot-instructions.md` にも、特定の言語、ファイル形式、タスク向けの `*.instructions.md` にも配置できます。今回追加したドキュメントコメントのルールのような全体規約には、リポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

更新後の指示を読み込んだ状態で、先ほどと同じ依頼を Copilot に送ります。

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト規約に従うよう Copilot を導き、次の演習でも使う実際のコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもほかのリポジトリ資産と同じようにソース管理します。更新した指示とヘルパーの土台をリポジトリにコミットしてプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. 次のコマンドを実行し、指示とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基礎作成で Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント規約に関する Issue に関連付けたプルリクエスト (PR) を作成します。GitHub ツールがプロジェクトに接続されているため、Copilot に Issue を探して PR を作成し、マージするよう依頼できます。

> [!NOTE]
> この演習では、Copilot が初めて GitHub 上の操作を行います。プロジェクトのテンプレートでは GitHub のツールがあらかじめ設定されています。Copilot が初めてツールを使うとき、VS Code から GitHub へのサインインを求められることがあります。プロンプトに従って許可してください。この接続の仕組み (Model Context Protocol) は後の演習で扱います。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 次のプロンプトを送り、指示の更新と publishers ヘルパーの土台を説明し、現在のブランチから PR を作るよう依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成する様子を確認します。
5. GitHub の操作を許可するよう求められた場合は、コマンドを確認して必要な操作を **Allow** します。
6. PR が作成されたら、次のプロンプトで PR をマージし、ローカルブランチを `main` に戻して最新状態にするよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作やシェルコマンドの実行を許可するよう求められた場合は、コマンドを確認して必要な操作を **Allow** します。

これで、GitHub Copilot の支援を受けてプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示でコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の規約を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい規約に従う様子を確認しました。
- 指示の更新とヘルパーの基礎をコミットし、次の演習で使えるよう `main` に反映しました。

次は、コードベース全体に新機能を追加しながら[エージェントモードで指示を活用します][next-lesson]。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
