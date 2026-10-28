---
title: "レッスン 2 - 星評価の追加: 小さな成果を得る"
description: "Copilot CLI でゲーム カードに小さな変更を加え、転送されたブラウザーで確認して、最初のプル リクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、プロジェクトに最初の変更を加えましょう。小さな変更から始めます。ゲームのデータには星評価がすでに含まれていますが、ホーム ページのゲーム カードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプル リクエストとしてマージします。

このレッスンでは、次の操作を行います。

- 機能ブランチで Copilot の会話を開始します。
- エージェントにプロジェクトの小さな変更を依頼します。
- `/diff` で変更をレビューします。
- 転送されたブラウザーでアプリを実行し、変更を確認します。
- 最初のプル リクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲームの詳細ページにはすでに表示されています。一方、ホーム ページのゲーム カードには、タイトル、カテゴリ、発行元、説明だけが表示されています。ウォームアップとして、各カードに既存の評価を表示するようエージェントに依頼します。最初のセッションに適した、小さく独立した変更です。

## 会話の構造

**会話**は、Copilot CLI でタスクに取り組む場所です。Copilot app とは異なり、通常の CLI 会話では、ターミナルで現在チェックアウトしているリポジトリと Git ブランチを使用します。保存した会話に戻って同じやり取りを続けられますが、ファイルとブランチは通常の Git の状態としてディスク上に保持されます。

会話では、プロンプトとエージェントの応答、ファイルを調査・編集するエージェントのツール操作、`/diff` で確認できる変更の 3 つを確認できます。

## 会話を開始して変更を依頼する

機能の実装を始めるため、新しい会話を開始しましょう。

1. Codespace に戻ります。
2. まだターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. まだ起動していない場合は、次のコマンドを実行して Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. スラッシュ コマンド `/new` を入力し、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲームの詳細を表示するファイルを特定して、必要なコードを作成します。Copilot CLI を使って新機能を追加できました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はマージする前にレビューします。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたファイルをすべて確認します。
2. 評価がある場合は数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. どちらの状態もテストされていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を終了します。

> [!NOTE]
> Copilot を含む生成 AI ツールは決定論的ではなく確率的に動作するため、実際のコードは例と異なる場合があります。完全に同じ実装ではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動作すると判断するのではなく、Codespaces で転送されたブラウザーを使って、更新された UI を確認しましょう。

1. 次のプロンプトで Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces にポート `4321` が使用可能と表示されたら、**Open in Browser** を選択します。
3. ゲーム カードに 5 段階評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプル リクエストを作成してマージする

機能を実装できました。次は、コードをプロジェクトにマージするプル リクエスト (PR) を作成します。

1. 既定のエージェントに変更をコミットするよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. Copilot CLI に表示された URL を <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、PR を開きます。
4. 変更されたファイルとチェックをレビューします。
5. 準備ができたら **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って最初の変更をリリースできました。具体的には、次の操作を行いました。

- 機能ブランチで Copilot の会話を開始しました。
- エージェントにゲーム カードの小さな変更を依頼しました。
- `/diff` で変更をレビューしました。
- 転送されたブラウザーでアプリを実行し、星評価を確認しました。
- 最初のプル リクエストを作成してマージしました。

次は、[Issue から始めて Plan モードと Autopilot モードを使い][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
