---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント規約を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合、その指針を Copilot が参照できるようにします。[指示ファイル][instruction-files]には、求めるコードだけでなく、その構成方法も記述できます。フィルター機能を実装したので、Copilot が参照した指示を確認し、ドキュメントの規約を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスを対象とする指示が、エージェントにどう適用されるかを確認します。
- コーディング規約に従うため、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインがあります。

- コメントには、コードの内容を繰り返すのではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` にあるエクスポート関数には、その目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。`db` 引数を注入できる場合は、その説明も含めます。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述し、関連コードが変更された場合はコメントも更新します。
- 既存の書式設定と lint の指針を維持します。

指示ファイルを使い、Copilot がこうしたガイドラインに沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示は、コーディングスタイルや要件を Copilot に伝えるコンテキストです。プロジェクト全体にも、特定の種類のファイルやタスクにも指示を設定できます。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に渡される単一の指示ファイルです。多くのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル形式、タスクに指針を設定するファイルです。

> [!NOTE]
> ほかの指示形式への対応状況は環境によって異なります。形式に依存する前に、[カスタム指示の対応状況][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトに含まれる指示ファイルを確認し、変更前の状態を把握します。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング指針を確認します。この指示は、このリポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データ層、テストなどの指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、指示を適用するファイルを決める glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルやプロジェクト内のファイルへのリンクを確認します。指示を小さく再利用しやすいファイルに分け、Copilot が参考にできる例を示せます。

## チームの方針に合わせて指示ファイルを更新する

既存ファイルにはよい指針が含まれていますが、新しく生成される TypeScript に TSDoc コメントを追加するルールがありません。

1. `.github/copilot-instructions.md` を開きます。
2. **Code formatting guidance** セクション (35 行目付近) を探します。
3. 最後の箇条書きに次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新した指針を使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を読み込ませつつ機能のコンテキストを保つため、フィルター機能の会話を終了して再開します。

1. フィルター機能の会話が開いている場合は `/exit` を入力します。Copilot CLI は会話を自動保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に指針を適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を実行し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、その説明がコードに合っていることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを得る方法を確認し、フィルター機能に新しい規約を適用しました。このレッスンでは、次のことを行いました。

- リポジトリの指示とパスを対象とする指示が、エージェントにどう適用されるかを確認しました。
- 規約に従うために指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行します][next-lesson]。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示の対応状況][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
