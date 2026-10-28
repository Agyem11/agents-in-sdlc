---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の作業を効果的に行うための指示、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に伝えます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントなど、チームの知識を再利用可能なエージェントにできます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーにある Markdown ファイル、またはユーザー共通の `~/.copilot/agents` にあるファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を記述する Markdown プロンプトが必要です。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、役割の重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡潔に言えば、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業の一環としてスキルやほかのエージェントを使えます。このラボでは、アクセシビリティのカスタム エージェントがサイトをアクセシビリティ ガイドラインに照らして確認し、更新します。その作業の一環として、プル リクエストのワークフロー スキルやテスト実行スキルなどを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成方法に唯一の正解はありません。AI のほかの用途と同様に、利用環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のユーザー フィードバックでは、テキストと背景色のコントラストが十分でないため、現在のダーク テーマが読みにくいという意見がありました。このアクセシビリティの問題に対処するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装します。Copilot Chat でアクセシビリティのカスタム エージェントを使います。

この演習では、次の操作を行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot Chat のアクセシビリティ エージェントを使ってハイ コントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。Copilot をどのように導くかを理解するため、内容を確認しましょう。

Codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用のブランチを作成します (演習 3 のフィルター PR とは分けておきます)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主な責任
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題と間違い

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント ドロップダウンには、`.github/agents` に定義されたすべてのカスタム エージェントが表示されます。カスタム エージェントを選択すると、その指示とツールに合わせてチャット セッションが設定されます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

1. Chat ビューのエージェント ドロップダウンで、**Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント ドロップダウンを選択します (既定では **default** が表示されています)。
3. 利用可能なエージェント一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクト ファイルを編集します。編集はファイル パスと差分とともにチャットに表示されます。
6. 編集が適用されると、**Files changed** の表示が更新され、エージェントが変更したファイルを確認できます。

> [!NOTE]
> この処理には数分かかることがあります。Copilot はリポジトリの実際のファイルを変更し、Astro コンポーネント、CSS、関連テストなどを更新します。

次の演習では、進行中の作業をレビューして方向を調整します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。特定のタスクや分野に合わせた専門 AI アシスタントを作成できます。チームの知識や標準を再利用可能なエージェントにして、Copilot が効果的に作業できるようにします。

この演習では、次のことを確認しました。

- カスタム エージェントの定義方法
- Copilot Chat の agent mode でカスタム エージェントを使う方法

次は、エージェントの変更を確認し、同じセッションにライト モードの切り替え機能を追加するよう依頼して、[作業を監視して方向を調整します][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
