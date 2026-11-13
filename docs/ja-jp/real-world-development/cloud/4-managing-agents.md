---
title: "演習 4 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の2つの演習では、ユーザー体験を改善して機能を追加する3つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、進行状況を監視することも重要です。

GitHub.com の[エージェントページ][agents-page]など、cloud agent に割り当てたタスクを管理する方法はいくつかあります。この Mission control では、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。作業内容を調べ、進行中のセッションに追加の指示を送ることもできます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションに追加の機能を依頼します。

## シナリオ

高コントラストモードの実装を依頼した後、チームはライトモードも追加したいと考えました。サイトのスタイルと切り替え機能を更新しているため、ライトモードも含めるのが合理的です。エージェントの作業を導き、高コントラストモードとライトモードの両方を追加します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインのタスクリストを確認します。次のような、Copilot に割り当てたタスクが表示されます。
   - コードベースのドキュメントの更新。
   - ゲームを変更するアプリケーション プログラミング インターフェイス (API) の生成。
   - Web サイトへの高コントラストモードの追加。
3. 実行中のタスクを1つ選択し、Copilot が実行した操作を確認します。次のような操作が表示されることがあります。
   - リポジトリのチェックアウト。
   - Copilot の作業環境の作成。
   - MCP サーバーの設定。
   - タスクを完了するための各種操作。

> [!NOTE]
> 表示される具体的な手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインを確認します。PR と変更されたファイルを監視できます。

## cloud agent の作業を方向付ける

実行中のタスクを確認したので、高コントラストモードの作業中にライトモードも含めるよう Copilot に依頼します。

1. 高コントラストモードの追加に関するセッションを選択します。表示される正確なタイトルは、Copilot が付けた名前や作業状態によって異なります。

   ![Mission control に表示されたアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セッションがセットアップを終えて作業を開始するまで数分待ちます。作業開始後は、その状態を示すメッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot の作業中にセッションを方向付けるダイアログを強調した、エージェントページのタスク画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が依頼を認識し、作業に取り込むことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行中だった処理が終わると、新しい依頼を作業の流れに取り込みます。

作業には数分かかります。その間に、これまでに学んだことを振り返ることができます。

## まとめと次のステップ

GitHub Copilot cloud agent のタスクを監視して案内する中心的な場所である Copilot agents ページを確認しました。Mission control では、実行中と完了済みのタスクを追跡し、作業内容をレビューして、進行中のタスクに追加の指示を送れます。

このレッスンでは、次のことを行いました。

- Mission control とエージェントページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加の機能を依頼しました。

Copilot がアクセシビリティ機能を作成している間に、次は[Copilot が作成したプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
