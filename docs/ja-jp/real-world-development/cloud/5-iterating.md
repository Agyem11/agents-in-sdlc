---
title: "演習 5: GitHub Copilot の作業を改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の向上や機能追加を目的に、GitHub Copilot にいくつかのタスクを依頼しました。コードへのドキュメント追加、デザイン チームが改善を検討できる関連ゲーム機能の作成、ハイ コントラストとライト モードの切り替えを含むアクセシビリティ機能の実装を依頼しました。変更内容を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成コードをレビューし、通常の DevOps プロセスに沿って進めます。チームでのレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期で、監視されずにタスクを実行するため、安全性を保つためのセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスはコード用ブランチに限られます。
- Cloud agent は GitHub Actions 内で動作し、作業には独立した一時的な環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- [MCP サーバーを含む外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

GitHub Copilot cloud agent が作成した最初のプル リクエスト (PR)、つまりコードへのドキュメント追加を確認します。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するときに、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot の外部リソース (外部 MCP サーバーへの呼び出しを含む) へのアクセスは既定で制限されているため、これは**想定どおり**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が付いています。その場合は作業の完了を待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc コメントやその他のドキュメントを含むコードを確認します。具体的な変更内容は状況によって異なります。

    変更を確認するときは、TSDoc コメント、TypeScript 規約、コメント ヘッダーを確認します。これらは演習 2 の最初に確認したカスタム指示ファイルに基づいています。

6. 更新内容に問題がなければ **Conversation** タブに戻り、下へスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローがキューに登録されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

Copilot と PR でやり取りする方法は、結果を受け取るだけではありません。チーム メンバーに依頼するのと同じように、PR のコメントやコードのインライン コメントで Copilot にメンションできます。Copilot はそのコメントを読み、対応する新しいセッションを開始します。結果は確定的ではないため、必ずしも適切な依頼文を提示できるわけではありません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、役割を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. PR またはコードのコメントで **@copilot** をメンションし、生成されたドキュメントへの変更を依頼します。上記の例や、コードベースに追加したい内容を使います。
2. **View Session** を選択し、Copilot が作業する様子を確認します。更新のために新しいセッションが始まります。
3. **Back to pull request** を選択すると PR に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更をレビューします。

満足するまで反復できます。準備ができたら、ドラフト PR をレビュー可能な状態に変更して、`main` ブランチにマージします。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue を解決するため、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容に問題がなければ **Conversation** タブに戻り、下へスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローがキューに登録されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace でターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます。例: **copilot/fix-8**)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に更新を依頼して、反復的に改善できます。たとえば、表示する関連ゲームの数を変更したり、指示の更新前に割り当てたことを踏まえて、コメント ヘッダーや TSDoc コメントの追加を依頼したりできます。前と同じく、**Conversation** タブでコメントを追加すると、Copilot が読み取って新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントが実装した機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 で mission control から追加を依頼したライト モードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が付いています。その場合は、作業が完了するまで数分待ちます。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認するときは、特に次の点に注意します。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保持する local storage の使用
   - ハイ コントラストとライト モードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容に問題がなければ **Conversation** タブに戻り、下へスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローがキューに登録されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace でターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイ コントラストとライト モードの切り替えを試します。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ カスタム エージェントが、アクセシビリティのベスト プラクティスに沿った機能実装を Copilot に促したことを確認できます。アクセシビリティに関する懸念や改善点があれば、前の PR と同様にコメントで **@copilot** をメンションして更新を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent を活用するには、バックログのタスクを任せて別の作業に取り組みます。習慣にするために、リポジトリにいくつか Issue を追加して Copilot に割り当ててみましょう。次のような例があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent のハーネスを完了しました。これらのレッスンでは、次のことを行いました。

- このリポジトリにあるカスタム指示ファイルを確認し、cloud agent の出力に与える影響を調べました。
- Copilot cloud agent に Issue を割り当て、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタム エージェントを作成して使い、ハイ コントラストとライト モードの切り替え機能を追加しました。
- Copilot agents ページを mission control として使い、アクセシビリティ セッションを監視して方向を調整しました。
- cloud agent のプル リクエストをレビューして改善し、`@copilot` に変更を依頼してワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent のハーネスを完了しました。さらに学びたい場合は、ほかのハーネスで関連するシナリオを体験できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat の agent モードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェント スキル、QA カスタム エージェント、Playwright MCP、Agent Merge、cloud agent への任意の `/delegate` ブリッジを使い、ターミナルでレビュー済みの変更を 2 つリリースします。

自分のリポジトリでは、次のアイデアを試してみてください。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てます。
- 別の分野に特化したカスタム エージェントを作成します。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
