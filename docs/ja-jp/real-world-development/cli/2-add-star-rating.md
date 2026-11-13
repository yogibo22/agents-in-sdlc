---
title: "レッスン 2 - 星評価の追加: 小さな成果"
description: "Copilot CLI でゲームカードに小さな変更を加え、転送されたブラウザーで差分を確認して、最初のプルリクエストをマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次は、プロジェクトに最初の変更を加えます。変更は小さくします。ゲームのデータには星評価がありますが、ホームページのゲームカードには表示されていません。エージェントに評価を表示させ、変更を確認して、最初のプルリクエストをマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで Copilot との会話を開始します。
- エージェントに小さな変更を依頼します。
- `/diff` で変更をレビューします。
- 転送されたブラウザーでアプリを起動し、変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかし、ホームページのゲームカードには、タイトル、カテゴリ、発売元、説明しか表示されていません。最初のセッションに適した小さな変更として、各カードに既存の評価を表示します。

## 会話の構成

**会話**は、Copilot CLI でタスクに取り組む場所です。Copilot app とは異なり、通常の CLI 会話では、ターミナルで現在チェックアウトしているリポジトリと Git ブランチを使います。保存した会話は後で再開できますが、ファイルとブランチはディスク上の通常の Git 状態として保持されます。

会話では、プロンプトとエージェントの応答、ファイルを調べて編集する際のツール操作、`/diff` で確認できる変更の3つを扱います。

## 会話を始めて変更を依頼する

新しい会話を開始し、機能の実装に取りかかります。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. ターミナルで次のコマンドを実行し、Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. スラッシュコマンド `/new` を実行して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲーム詳細の表示に使うファイルを探して、必要なコードを作成します。Copilot CLI で最初の機能を追加できました。

## 差分をレビューする

小さな変更でも、人工知能 (AI) が生成した変更はマージ前にレビューします。Copilot CLI 内で変更を確認します。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. ゲームカードに数値評価が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. テストで両方の状態がカバーされていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を終了します。

> [!NOTE]
> Copilot を含む生成 AI ツールは確率的に動作するため、生成されるコードは例と異なる場合があります。特定の実装ではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動作すると判断せず、Codespaces で転送されたブラウザーから更新後の UI を確認します。

1. Copilot に次のプロンプトを送り、アプリを起動します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. ゲームカードに5段階の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。次は、変更をプロジェクトに取り込むプルリクエスト (PR) を作成します。

1. 既定のエージェントに次のプロンプトを送り、変更をコミットします。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を実行します。PR の作成時に Copilot CLI が既存のコミットをプッシュし、PR の URL を表示します。
3. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、Copilot CLI が表示した URL を選択して PR を開きます。
4. 変更されたファイルとチェック結果を確認します。
5. 準備ができたら **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って、最初の変更をリリースしました。このレッスンでは、次のことを行いました。

- 機能ブランチで Copilot との会話を開始しました。
- エージェントにゲームカードの小さな変更を依頼しました。
- `/diff` で変更をレビューしました。
- 転送されたブラウザーでアプリを起動し、星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は、[フィルター機能の Issue から始めて Plan モードと Autopilot モードを使います][next-lesson]。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
