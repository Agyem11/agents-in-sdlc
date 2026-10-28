---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加といった作業を自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せることで、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを学びます。

- コード生成環境をカスタマイズする方法
- 安全に操作を実行する方法
- Issue の範囲を明確に定義する重要性
- Copilot に Issue を割り当てる方法

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを作成した委託先は、ドキュメントを十分に整備していませんでした。まず、アプリケーション内のエクスポートされた関数に TSDoc コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのゲームを表示し、閲覧を続けられるようにします。UX の受け入れテストに使える状態になればよく、まだ完成度の高い実装は必要ありません。この機能は現在のブロッカーですが、より優先度の高い Issue もあります。

どちらも、優先度が下がりやすい作業の例です。Copilot cloud agent に割り当てれば、非同期で作業してもらい、その間にほかのタスクへ集中できます。作業が終わったら、結果をレビューして期待どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業できます。[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、複数の方法で作業を依頼できます。割り当て後、Copilot は進捗を追跡するドラフト プル リクエストを作成し、環境をセットアップして、タスクに取り組みます。作業中または完了後にセッションを確認できます。提案された解決策のレビュー準備ができると、プル リクエストで通知されます。

## 適切な範囲の指示が重要

GitHub Copilot に魔法はありません。考える手間を省けるわけではありません。一見簡単な操作でも、詳しく見ると複雑さが分かります。[Copilot cloud agent に依頼するタスクの範囲を明確にする][cloud-agent-best-practices]ことが重要です。AI ペア プログラマーとして扱い、段階的に進め、学び、試し、必要に応じて調整してください。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示とその働きを学びました。すでにカスタム指示の演習を済ませた場合は `.github/copilot-instructions.md` の効果を確認しています。まだの場合は、この機会に見てみましょう。Copilot cloud agent に作業を割り当てる前に、リポジトリに含まれる指示ファイルを読み取り専用で確認して、後でその影響を見つけられるようにします。

リポジトリの GitHub Web UI、または起動済みの codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript 規約と TSDoc コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターで、`**/*.test.ts` ファイルに指示が適用されることを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てる際、生成されるプル リクエストで TSDoc コメント、TypeScript 規約、コメント ヘッダーを確認します。後の演習で PR をレビューするときにも、改めて取り上げます。

## Copilot cloud agent の開発環境を設定する

コードの作成には、通常、特定の環境や事前に実行するセットアップ スクリプトが必要です。タスクを Copilot に割り当てる場合も同じです。Copilot はソフトウェア エンジニアのように作業を進めます。

Cloud agent は [GitHub Actions][github-actions] を使って作業環境を構築します。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業前に必要な開発ツールや依存関係をインストールできます。このラボのために、セットアップはあらかじめ用意されています。Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、単一 Astro アプリ用に移行とシードが済んだ SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限のアクセス許可を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。クローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
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

      # ビルドとテストで使用するデータを用意するため、ローカル SQLite データベースを移行してシードします。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub Actions ワークフローと同様に見えますが、次の点に注目してください。

- `copilot-setup-steps` という 1 つのジョブがあります。このジョブは、Copilot がプル リクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブからワークフローを手動実行できるため、Copilot が実行するまで待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古かったり、まったくなかったりするプロジェクトも少なくありません。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成に役立つため、cloud agent に依頼するのに適したタスクです。次の演習で、生成されたドキュメントをレビューします。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。

    ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプル リクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot がタスク リストを作成したことを確認します。

> [!NOTE]
> Copilot のタスク リストが PR に表示されるまで数分かかることがあります。Copilot は環境を構築し (先ほどのワークフローを実行します)、プロジェクトを分析して、最適な進め方を決めます。

13. タスクリストと実行予定の作業を確認します。
14. プル リクエストのタイムラインを下へスクロールし、Copilot が Issue の作業を開始したことを確認します。
15. **View session** ボタンを選択します。

    ![Copilot のセッション ビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ウィンドウを更新してください。

16. ライブ セッションをスクロールして、Copilot が問題を解決する様子を確認します。コードを調べて状態を理解し、計画を考えて判断した後、コードを作成する流れを確認できます。

完了まで数分かかることがあります。Copilot cloud agent の主な目的は、作業を非同期で実行して、その間に別の作業を進められるようにすることです。この機能を使って、もう 1 つのタスクも Copilot cloud agent に割り当て、その間にアプリケーションの機能を作成しましょう。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分担できることです。デザイン チーム向けに関連ゲームのセクションを追加する作業は短時間で終わるかもしれませんが、その時間をほかのタスクに使えます。この作業も Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
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
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。

    ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことを示します。

![Copilot が Issue に取り組んでいることを目の絵文字で示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェア エンジニアのように動作するため、常に監視する必要はありません。作業が終わってからレビューできます。次は、カスタム エージェントの作成と使用を見ていきましょう。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に Issue を割り当て、非同期で作業を依頼できます。技術的負債の解消、新機能の作成、フレームワーク間のコード移行などに利用できます。

この演習では、次のことを確認しました。

- コード生成環境をカスタマイズする方法
- 安全に操作を実行する方法
- Issue の範囲を明確に定義する重要性
- Copilot に Issue を割り当てる方法

cloud agent がバックグラウンドで作業している間に、次はカスタム エージェントを作成して使います。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も使用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
