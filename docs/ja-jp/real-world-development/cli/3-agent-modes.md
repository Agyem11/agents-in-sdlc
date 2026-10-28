---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で進め方を合意し、Autopilot で Issue に基づくフィルター機能を実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

プロジェクトに小さな機能を追加しました。より大きな変更には、さらにしっかりとしたプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って動作するため、適切な方法で必要な機能を構築できます。このレッスンから、エージェントを使った一般的な開発プロセスを進めます。Issue から新機能を作成し、コードの妥当性と機能の動作を確認した後、プロジェクトにマージします。

> [!NOTE]
> 機能開発を続ける間は、同じ会話とブランチを使用します。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中するため手順を簡略化します。

このレッスンでは、次の操作を行います。

- GitHub Issue から Copilot の新しい会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

この機能開発を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えているため、訪問者がカテゴリと発行元でゲームを絞り込めるようにします。バックログの Issue には機能の概要が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細があります。Plan モードで要件を明確にし、範囲を限定した実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本的な考え方は変わりません。むしろ、より重要になります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を記述した Issue から始めます。
2. 実装する内容の計画を立てます。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プル リクエスト (PR) を作成します。
7. コードレビューが完了し、継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって具体的な手順は異なりますが、ほとんどの場合は上記の流れを基にしています。

この標準的な進め方に従うことで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビュー プロセスを通るようにできます。

## 会話モード

**会話モード**では、エージェントがどこまで自律的に作業するかを制御できます。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントが変更を提案し、次に進む前にユーザーの入力を待ちます。
- **Plan**: エージェントが計画を作成します。プロジェクト ファイルを編集することはできません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から開始する

フィルター機能の作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続行します。
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

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の Issue やプル リクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、Copilot に作業を依頼する前に、機能の実装方法やタスクの進め方を決められます。複雑な作業では、時間をかけて計画するのが効果的です。Plan モードに切り替えて、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べて、計画を作成します。

3. フィルターの動作について質問された場合は、希望に応じて回答します。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画全体を確認します。
6. 決めた内容と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューできたら、実装に進みます。Autopilot を使って Copilot に作業を任せます。

Autopilot では、タスクが完了したと判断するまで Copilot が自律的に反復します。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みのバージョンに表示される同様の選択肢を選択します。
2. プロンプトの下にあるモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があります。最初に計画をレビューしてください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェック完了と判断する前にセットアップの問題を解決してください。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同様にマージ前にレビューします。コードを確認し、サイトを実行して問題がないか確認しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** または **Autopilot** が表示されていないことを確認します。
2. `/diff` を入力し、フィルター機能の実装とテストを確認します。
3. 結果を Issue と計画で決めた内容と照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分ビューを終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を試す

コードに問題がないことを確認したら、Codespaces の転送ポートからサイトを開いて、実際に動作することを確認します。

1. 次のプロンプトで Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces にポート `4321` が使用可能と表示されたら、**Open in Browser** を選択します。
3. カテゴリ、発行元、および計画で決めた組み合わせでフィルターを試します。
4. リセットと結果がない場合の動作が、Issue と決定内容に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

会話モードを使い分けて、機能を構築してレビューしました。このレッスンでは、次の操作を行いました。

- GitHub Issue から Copilot の新しい会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は、コードが文書化された規約に従うよう、[カスタム指示を使って][next-lesson]コード生成を詳しく見ていきましょう。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
