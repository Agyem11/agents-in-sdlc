---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

Agent mode でフィルター機能を構築しました。プル リクエストを開く前に、ブラウザーで実際に動作することを確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の操作を行います。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーでフィルター機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、フィルター機能のプル リクエストを作成します。

## Model Context Protocol (MCP) とは

Agent mode は、エディターの外部にあるツールへアクセスすると、さらに強力になります。Copilot が外部のツールやサービスと通信するための標準的な方法が、Model Context Protocol (MCP) です。

![agent mode の内部構造と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントが外部のツールやサービスと通信できます。AI エージェントはリソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作を実行したりできます。

これらのツールとリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを介してアクセスします。MCP サーバーは、既存 API や NPM パッケージなどのローカル ツールとの通信を管理します。各 MCP サーバーからは、エージェントが利用できる複数のツールやリソースが提供されます。

広く使われている MCP サーバーの例を紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。AI エージェントは新しいリポジトリの作成、既存リポジトリの更新、Issue やプル リクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化を提供します。Web ページの移動、フォームの入力、ボタンの選択などをエージェントが実行できます。

ほかにもさまざまな MCP サーバーがあります。GitHub が提供する [MCP レジストリ](https://github.com/mcp)を使うと、エコシステム内の MCP サーバーを見つけやすくなります。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはほかの依存関係と同様に扱ってください。使用前にソース コードを慎重に確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. codespace で `.vscode/mcp.json` を開きます。
2. 次のような `github` サーバーがすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれています。そのため、前の演習で Copilot が Issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供するため、機能のテストに適しています。

1. `.vscode/mcp.json` で `github` の隣に `playwright` エントリを追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル** サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、ブラウザー ウィンドウを表示せずに Playwright を実行します。デスクトップ環境がない codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンド テストに Playwright をすでに使っているため、通常は Playwright 用ブラウザーがインストール済みです。Copilot がブラウザーがないと報告した場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot がサーバーのツールを初めて使うときにサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** 操作が表示されます。選択して今すぐサーバーを起動し、構成が正しいことを確認します。
2. サーバーを信頼するか尋ねられた場合は、構成を確認し、サーバーの起動を許可します。

手動で起動する必要はありません。スキップしても、次の手順で Copilot がツールを初めて必要としたときに VS Code がサーバーを起動し、信頼を確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作する代わりに、エージェントがページを開き、移動し、フィルターを適用し、結果を読み取って報告します。会話を離れずに機能を確認できる方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。ボタン、リンク、リスト項目など、ラベル付きの構造化された要素をエージェントが理解します。これは支援技術と同じ方法であるため、簡単な機能テストをアクセシビリティの簡易確認にもできます。

サーバーが接続され、アプリが実行中であることを確認し、次のプロンプトで構築したフィルター機能を Copilot に試してもらいます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを使ってブラウザーを起動し、各手順を実行して結果を報告します。Issue の受け入れ基準と照らし合わせます。問題がある場合は、質問を追加するか、プル リクエストを開く前にコードの修正を依頼します。

> [!NOTE]
> テストするには、アプリが `http://localhost:4321` で動作している必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP サーバーを初めて使うとき、ブラウザーをダウンロードする必要がある場合があります。ブラウザーが見つからないと報告された場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプル リクエストを作成する

機能が動作することを確認できたら、チームのレビュー用にプル リクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** パネルに移動し、Copilot が加えた変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![変更内容が表示された Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プル リクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) など、プル リクエストを作成する方法はいくつかあります。GitHub Copilot を使って、PR の作成を依頼しましょう。関連 Issue を探し、PR に関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェントのドロップダウンで **Agent** が選択され、Copilot が GitHub ツールを使えることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 情報収集や操作の実行に必要な場合は **Continue** を選択して許可します。GitHub ツールを初めて使うときは、GitHub での認証を求められることがあります。画面の指示に従って認証します。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が作成したリンクを選択して PR をレビューします。ただし、まだマージしないでください。

## まとめと次のステップ

この演習では、次のことを学びました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組み
- Playwright MCP サーバーをワークスペースに追加する方法
- ブラウザーでフィルター機能を手動テストするために Copilot を使う方法
- ブランチを公開して、フィルター機能のプル リクエストを作成する方法

MCP サーバーを使って機能を**テスト**しました。MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティ レビューなどの作業を効率化する[カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
