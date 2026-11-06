---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI の Playwright MCP を確認または構成し、ブラウザーで絞り込み機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

すでに説明したように、コードを書くだけでは開発は完了しません。データや外部サービスを扱い、Copilot が利用できる自動化も追加します。MCP サーバーを使うと、CLI に組み込まれた機能だけでなく、さらに多くのツールやサービスを Copilot から利用できます。

このレッスンでは、次の操作を行います。

- Model Context Protocol (MCP) と Copilot CLI での使用方法を確認します。
- まだ使用できない場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、絞り込み機能を確認します。

## シナリオ

単体テストとエンドツーエンド テストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。ユーザーと同じように Web サイトを操作する機能を Copilot に与えることで、変更が想定どおりに動作するか、さらに自動的に確認できます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、AI エージェントが外部のツールやサービスと通信できます。MCP によって、AI エージェントは外部ツールやサービスとリアルタイムにやり取りし、最新情報を取得したり、ユーザーに代わって操作を実行したりできます。

ツールやリソースには MCP サーバーを通じてアクセスします。MCP サーバーは、AI エージェントと外部ツールやサービスをつなぐ役割を担います。それぞれの MCP サーバーから、AI エージェントが使用できるツールやリソースにアクセスできます。

よく使われている MCP サーバーには、次のようなものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プル リクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を利用できます。

ほかにも多くの MCP サーバーが公開されています。GitHub は、エコシステム内の MCP サーバーを見つけやすくし、貢献を促進するために [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクトのほかの依存関係と同様に扱ってください。使用前にソース コードを確認し、発行元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

まだ Playwright MCP サーバーを利用できない場合は、追加して Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI で Playwright MCP をすでに構成している場合は、ここでのセットアップ手順をスキップして次に進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力して、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd>、次に <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

先ほどは機能が想定どおり動作することを手動で確認しました。次は、Copilot で使用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に伝えます。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、自動的に MCP サーバーを選択するため、使用するよう明示する必要はありません。ただし、使用するツールが分かっている場合は、指示すると結果の一貫性を高め、トークンを節約できます。

2. 機能が動作することを確認するために、Copilot がブラウザーで行う手順を確認します。
3. レポートを読み、すべてが想定どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認しました。ここまでに、次の操作を行いました。

- Model Context Protocol (MCP) と Copilot CLI での使用方法を確認しました。
- 使用できない場合は Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、絞り込み機能を確認しました。

次は、スキルとブラウザー ツールをまとめた QA カスタム エージェントを[作成します][next-lesson]。

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
