---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成型人工知能 (生成 AI) を使うときは、コンテキストが重要です。タスクを特定の方法で行いたい場合や、Copilot に背景情報を伝えたい場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、コードを何のために、どのように作成するかを Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスを対象とする指示が、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい規約を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では指示の更新前後でプロンプトの結果を比較しません。Copilot cloud agent は GitHub Issue に対して非同期で作業するため、リアルタイムの比較が難しいためです。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

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

リポジトリに用意された、中心となる `copilot-instructions.md` と、タスクごとの `*.instructions.md` ファイルを確認します。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指針もあります。これらは Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ層、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` は、リポジトリのルートからの glob で適用対象を指定します。たとえば、`**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾まで読みます。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内のファイルへのリンクを確認します。パスは指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約がありますが、コード内のドキュメントに関する規約はまだありません。次の手順で TSDoc コメントとファイルヘッダーのルールを追加します。

## 新しいリポジトリ規約を追加する

ここで行う編集は、プロジェクト全体に適用する規約を追加する1か所です。後の演習で Issue を割り当てるとき、cloud agent はこの指針に従います。

編集の前に作業用のブランチを作成します。cloud agent は Issue の対象となるブランチから指示を読み込みますが、この変更はきちんとコミットします。

1. codespace のターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` にプロジェクトレベルの情報を記載し、コード提案の規約を追加します。

1. `.github/copilot-instructions.md` を開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。既存の規約はありますが、先ほどのヘルパーにドキュメントコメントがなかった理由として、コード内ドキュメントのルールがないことを確認します。
3. 既存の規約の直後に、ファイルヘッダーと TSDoc コメントを求める次の行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 指示ファイルは、リポジトリ全体向けの `.github/copilot-instructions.md` にも、特定の言語、ファイル形式、タスク向けの `*.instructions.md` にも配置できます。今回追加したドキュメントコメントのルールのような全体規約には、リポジトリレベルのファイルが適しています。

## 指示の更新をコミット、プッシュしてマージする

cloud agent は Issue の対象ブランチから指示ファイルを読み込みます。次の演習で Issue を割り当てると、Copilot は `main` からブランチを作成します。そのため、指示の変更を `main` に取り込む必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で Issue を割り当てるとき、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップでは、ブランチを作成してコミットする代わりに `main` へ直接作業してもかまいません。実際のプロジェクトに近い手順にするため、ここではブランチを使います。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の新しい規約を追加しました。このハーネスの後半で cloud agent が作成するプルリクエストに、追加した規約が反映されます。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
