---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で進め方に合意し、Autopilot で Issue に基づく絞り込み機能を実装して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初は小さな機能を追加しました。より大きな変更には、さらに確かな進め方が必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って動作し、必要なものを適切な方法で構築できるよう支援します。このレッスンから、Issue を起点に機能を作り、コードを検証し、動作を確かめて、プロジェクトにマージするまでのエージェント主導の開発プロセスを進めます。

> [!NOTE]
> 機能の作業を続ける間、同じ会話とブランチを使います。通常はファイルの種類ごとにブランチやプルリクエスト (PR) を分けますが、ここでは主な概念に集中するため手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から Copilot の新しい会話を始めます。
- Plan モードで要件を定義します。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、Model Context Protocol (MCP) による検証を追加して品質保証 (QA) エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がカテゴリと発売元でゲームを絞り込めるようにする必要があります。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、実装前に合意が必要な詳細もあります。Plan モードで決定事項を整理し、Autopilot には範囲を明確にした実装を依頼します。

## 背景

人工知能 (AI) コーディングエージェントを開発フローに導入しても、基本は変わりません。開発プロセスの重要性はむしろ増します。多くの開発者は、次のような流れで作業します。

1. 実装内容を記述した Issue から始めます。
2. 構築するものを計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって具体的な手順は異なりますが、ほとんどの開発フローはこの流れを基にしています。

この標準的な進め方に従うことで、AI が生成したコードが要件を満たし、手書きのコードと同じレビューを受けるようにできます。

## 会話モード

**会話モード**は、エージェントに与える自律性の度合いを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: 開発者とエージェントが協力して作業します。エージェントは変更を提案し、続行前に入力を待ちます。
- **Plan**: エージェントが最初に計画を作成し、プロジェクトファイルを編集できない状態になります。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

フィルター機能の作業を開始する前に、Codespace とリポジトリを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーを使い、**Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、機能の実装やタスクの実行を Copilot に任せる前に、そのアプローチを決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan モードに切り替えて計画を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 先ほど追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を検討して計画を作成します。

3. フィルターの動作について質問された場合は、希望に沿って回答します。選択肢に正解や不正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を読みます。
6. 自分の決定と合わない箇所があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を確認できたら、Autopilot を使って実装します。Autopilot は、タスクが完了したと判断するまで Copilot が問題に取り組み、反復できるようにします。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みバージョンに表示される同様の項目を選択します。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があります。先に計画を確認してください。依存関係の不足やポートの競合が報告された場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードの生成後は、ほかのコードと同様にマージ前にレビューします。コードを確認し、サイトを起動して動作も確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** または **Autopilot** が表示されていないことを確認します。
2. `/diff` を実行し、フィルターの実装とテストを確認します。
3. 結果を Issue と計画時の決定事項に照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新しい機能を試す

コードを確認できたら、Codespaces で転送されたポートを通じてサイトを開きます。

1. Copilot に次のプロンプトを送り、アプリを起動します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリ、発売元、および組み合わせで絞り込みを試します。
4. リセット時と結果がない場合の動作が、Issue と決定事項に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を構築し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から Copilot の新しい会話を始めました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は、コード生成が文書化された規約に従うよう、[カスタム指示を使います][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
