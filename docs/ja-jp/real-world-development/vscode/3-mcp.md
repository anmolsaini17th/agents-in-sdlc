---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードで絞り込み機能を作成しました。プル リクエストを開く前に、ブラウザーで実際に動作することを確認します。アプリを手動で操作する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーから機能をテストしてもらいましょう。その後、ブランチを公開して PR を作成します。

この演習では、次の操作を行います。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する方法を確認します。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーで絞り込み機能を手動でテストしてもらいます。
- ブランチを公開し、絞り込み機能のプル リクエストを作成します。

## Model Context Protocol (MCP) とは

エージェント モードは、エディターの外部にあるツールを使えると、さらに強力になります。Copilot が外部ツールを利用する方法が Model Context Protocol (MCP) です。MCP は、エージェントが外部のツールやサービスと通信するための標準です。

![エージェント モードの内部と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための手段を提供します。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムにやり取りし、最新情報 (resources) にアクセスしたり、ユーザーに代わって操作 (tools) を実行したりできます。

ツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じてアクセスします。MCP サーバーは、AI エージェントと外部ツール (既存の API や NPM パッケージなどのローカル ツール) 間の通信を管理します。それぞれの MCP サーバーから、AI エージェントが使用できるツールやリソースにアクセスできます。

よく使われている MCP サーバーには、次のようなものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。AI エージェントは、新しいリポジトリの作成、既存リポジトリの更新、Issue やプル リクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントは Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

ほかにも、さまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーを見つけやすくし、貢献を促進するために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクト内のほかの依存関係と同様に扱ってください。使用前にソース コードを慎重に確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密性の高いリソースや操作へのアクセスを許可するときは注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで使用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに構成されていることを確認します。

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

この `github` エントリはプロジェクト テンプレートに含まれているため、前の演習で Copilot が Issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次は 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作できるブラウザーが使えるようになります。機能をテストするために必要なツールです。

1. `.vscode/mcp.json` の `github` と並べて `playwright` エントリを追加し、ファイルが次のようになるようにします。

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

リモート サーバーの `github` と異なり、`playwright` は**ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行してサーバーを起動します。`--headless` フラグは、表示用デスクトップ環境がない Codespace でブラウザーを画面表示せずに実行するために必要です。

> [!NOTE]
> Tailspin Toys ではすでにエンドツーエンド テストに Playwright を使っているため、通常はブラウザーもインストールされています。Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行して再試行するよう依頼します。

## サーバーを起動して信頼する

VS Code は、Copilot がサーバーのツールを初めて必要としたときに MCP サーバーを**オンデマンドで**起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code でサーバーを信頼するか確認された場合は、構成を確認し、サーバーを起動できるように信頼します。

手動で起動する必要はありません。スキップすると、次の手順で Copilot がツールを初めて使うときに VS Code がサーバーを起動し、信頼の確認を求めます。

## 絞り込み機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作する代わりに、エージェントがページを開き、移動し、フィルターを適用し、結果を読み取って、確認した内容をまとめます。会話を中断せずに機能を確認できるため、すばやくテストできます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。ボタン、リンク、リスト項目などのラベル付き要素を構造化された形でエージェントが扱うため、支援技術と同じように情報を処理できます。簡単な機能チェックが、アクセシビリティの基本確認にもなります。

サーバーが接続され、アプリが起動している状態で、Copilot に新しく作成した絞り込み機能を試してもらいます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を試して結果を報告します。結果を Issue の受け入れ条件と照らし合わせます。動作に問題がある場合は、プル リクエストを作成する前に追加の質問をするか、コードを修正するよう依頼します。

> [!NOTE]
> このテストには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーのダウンロードが必要な場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再試行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプル リクエストを作成する

機能が動作することを確認できたので、チームがレビューできるようプル リクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![Copilot が加えた変更を示す Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プル リクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) など、プル リクエストを作成する方法はいくつかあります。GitHub Copilot を使っているので、PR の作成も依頼できます。Copilot は関連する Issue を探し、Issue と関連付けて PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. GitHub ツールを使用できるように、エージェント ドロップダウンで **Agent** が選択されていることを確認します。
3. Copilot に PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を収集して操作を実行するために求めた場合は、**Continue** を選択します。GitHub ツールを初めて使うときは、GitHub への認証を求められることがあります。指示に従って許可します。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択して PR をレビューします。**まだマージしないでください**。

## まとめと次のステップ

おめでとうございます。この演習では、次の操作を行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する方法を確認しました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーで絞り込み機能を手動でテストするために、Copilot に Playwright MCP を使ってもらいました。
- ブランチを公開し、絞り込み機能のプル リクエストを作成しました。

MCP サーバーを使って機能を*テスト*しましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティ レビューなどの作業を効率化する[カスタム エージェントを作成します][next-lesson]。

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
