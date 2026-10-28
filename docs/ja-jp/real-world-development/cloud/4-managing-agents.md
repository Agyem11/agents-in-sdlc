---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前のいくつかの演習では、ユーザー体験の向上や機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。GitHub.com の[agents ページ][agents-page]もその 1 つです。この mission control では、オープンなプル リクエスト (PR) があるすべてのエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を与えたりできます。

このレッスンでは、次の操作を行います。

- agents ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

エージェントにハイ コントラスト モードを依頼した後、チームはライト モードも追加するとよいと気付きました。サイトのスタイルと切り替え機能をすでに更新しているため、同じ作業に含めるのが自然です。エージェントの作業にライト モードも追加されるよう、方向を調整します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインの両方にタスクが表示されます。ドキュメントの更新、ゲームを変更する API の生成、Web サイトへのハイ コントラスト モード追加など、Copilot に割り当てたタスクを確認します。
3. 実行中のタスクを 1 つ選択し、Copilot が実行した作業を確認します。次のような操作が含まれる場合があります。
   - リポジトリのチェックアウト
   - Copilot の作業環境の作成
   - MCP サーバーの設定
   - タスク完了に必要な各種手順

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプル リクエスト (PR) ペインも確認します。PR や変更されたファイルを確認して、作業を監視できます。

## cloud agent に追加の指示を出す

実行中のタスクを確認できたら、Copilot がハイ コントラスト モードを作成している間に、ライト モードの切り替え機能も追加するよう依頼します。

1. ハイ コントラスト モードの追加に関連するセッションを選択します。表示されるタイトルは Copilot が付けた名前や作業状況によって異なります。

    ![mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始したことが表示されるまで数分待ちます。次のようなメッセージが表示されたら作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![agents ページの cloud agent タスク。作業中のセッションに指示するダイアログが強調表示されている](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを確認し、作業に取り入れる様子を確認します。

## Copilot に作業を続けてもらう

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中の手順を終えた後、新しい依頼をワークフローに取り込みます。

前と同様に完了まで数分かかります。ここで少し休み、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot cloud agent のタスクを監視し、方向を調整するための中心的な場所である agents ページについて学びました。この mission control では、進行中および完了したタスクの追跡、実行内容の確認、進行中のタスクの範囲調整や追加の指示ができます。

このレッスンでは、次のことを確認しました。

- mission control と agents ページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は Copilot が作成したプル リクエストを[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
