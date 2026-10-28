---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーからフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

すでに説明したように、コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilot が利用できる自動化機能を追加することもあります。そこで役立つのが MCP サーバーです。MCP サーバーを使うと、Copilot CLI に組み込まれている機能以外のツールやサービスも利用できます。

このレッスンでは、次の操作を行います。

- Model Context Protocol (MCP) と Copilot CLI での使い方を確認します。
- Playwright MCP サーバーがまだ利用できない場合は追加します。
- エージェントにブラウザーを操作させ、フィルター機能を試します。

## シナリオ

ユニット テストとエンドツーエンド テストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。ユーザーと同じようにサイトを試す機能を Copilot に追加すれば、変更の動作を確認し、開発プロセスをさらに自動化できます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部のツールやサービスにリアルタイムでアクセスし、最新情報の取得や操作を行えます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを介してアクセスします。各 MCP サーバーからは、エージェントが利用できる複数のツールやリソースが提供されます。

広く使われている MCP サーバーの例を紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プル リクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化を行えます。

ほかにもさまざまな MCP サーバーがあります。GitHub が提供する [MCP レジストリ][mcp-registry]を使うと、エコシステム内の MCP サーバーを見つけやすくなります。

> [!CAUTION]
> MCP サーバーは、ほかの依存関係と同様に扱ってください。使用する前にソース コードを確認し、発行元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は追加して、Copilot がユーザーと同じようにサイトを操作できるようにします。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、セットアップをスキップして次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力して、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類に **STDIO** を選択し、<kbd>Enter</kbd> を押してから <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを終了します。

## Playwright で機能を試すよう Copilot に依頼する

前のレッスンでは、機能が期待どおりに動作することを手動で確認しました。今度は、Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行いましょう。

1. 次のプロンプトを使って、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、自動的に MCP サーバーを使います。そのため、明示的に指定する必要はありません。ただし、使用するツールが分かっている場合は、プロンプトに含めることで、より一貫した結果を得て、トークンを節約できます。

2. Copilot がブラウザーで機能を確認する手順を実行する様子を見ます。
3. 報告を読み、すべてが期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright でサイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を試すことができました。ここまでに行ったことを振り返ります。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学びました。
- Playwright MCP サーバーがまだ利用できない場合は追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を試しました。

次は、スキルとブラウザー ツールを組み合わせる [QA カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
