---
title: "レッスン 6 - Playwright MCP による機能の検証"
description: "Customize から Playwright MCP を設定し、既存の機能用 worktree でフィルター機能をブラウザーから確認します。"
authors:
  - geektrainer
lastUpdated: 2026-07-09
---

すでに説明したように、コードを書く作業には、単にコードを書く以上のことが含まれます。データや外部サービスを操作し、Copilot で追加の自動化を利用できるようにする必要があります。そこで役立つのが MCP サーバーです。MCP サーバーを使うと、Copilot はアプリに組み込まれた機能を超えて、さらに多くのツールやサービスを利用できます。

このレッスンでは、次の操作を行います。

- Model Context Protocol (MCP) の概要と、GitHub Copilot app での使用方法を確認します。
- Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

単体テストとエンドツーエンド テストは重要ですが、UI の更新を検証するには、実際に UI を操作する必要があります。変更作業をさらに自動化し、更新が期待どおりに動作するという確信を高めるために、ユーザーと同じように Copilot が作業中の Web サイトを使えるようにします。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための手段を提供します。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムで通信できます。その結果、最新情報へのアクセス (resources を使用) や、ユーザーに代わる操作 (tools を使用) が可能になります。

これらのツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じてアクセスします。MCP サーバーは、AI エージェントと外部ツール (既存の API や NPM パッケージなどのローカル ツール) 間の通信を管理します。各 MCP サーバーは、AI エージェントがアクセスできる異なるツールやリソースのセットを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API セットにアクセスできます。AI エージェントは、新しいリポジトリの作成、既存リポジトリの更新、Issue やプル リクエストの管理などを実行できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントは、Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

さまざまなツールやリソースにアクセスできる MCP サーバーがほかにもあります。GitHub は、エコシステム内での発見と貢献を促進するために [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内のほかの依存関係と同様に扱ってください。使用前にソース コードを慎重に確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密性の高いリソースや操作へのアクセスを許可するときは注意してください。

## Playwright MCP サーバーを追加する

サイドバーの **Customize** で MCP サーバーを管理します。リポジトリや Copilot CLI 向けに設定されたサーバーはアプリでも利用できる場合があるため、重複して追加する前に確認してください。利用できる選択肢については、[アプリのカスタマイズに関するドキュメント][customize-app]を参照してください。

1. サイドバーで **Customize** を選択します。
2. **All types** ドロップダウンを開いて **MCP** を選択し、**Installed** で既存の Playwright サーバーを確認します。
3. Playwright がまだインストールされていない場合は、利用可能なサーバーの中から **Playwright** を見つけ、**Add server** を選択します。
4. 発行元、構成、インストール時の確認事項をレビューしてから承認します。案内に従ってサーバーを追加します。組織のポリシーや前提条件の不足により、セットアップがブロックされることがあります。
5. **Interactive** モードでフィルター機能のセッションに戻り、Playwright MCP のツールが利用できることを確認します。

セットアップに失敗した場合は、続行する前に構成や権限の問題を解決します。

## Playwright で機能を確認するよう Copilot に依頼する

Issue と計画時の決定事項は、すでにコンテキストに含まれています。Copilot にサーバーの起動を依頼する前に、以前自分で起動した開発サーバーを停止します。

1. 次のプロンプトを使い、新しい機能を検証するよう Copilot に依頼します。

    ```plaintext
    Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
    ```

> [!NOTE]
> Copilot に特定の MCP サーバーを指定する必要はありません。通常は現在のコンテキストに基づいて適切なサーバーを見つけます。ただし、重要だと考える情報を伝えても問題ありません。

2. 動作を見守ります。

Copilot はサーバーを起動してブラウザーを開き、Web サイトを操作します。完了するとサーバーを停止し、レポートを提示します。

## まとめと次のステップ

GitHub Copilot app から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認しました。次の操作を行いました。

- Model Context Protocol (MCP) の概要と、GitHub Copilot app での使用方法を確認しました。
- Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を確認しました。

次は、スキルとブラウザー ツールを専門家の役割で組み合わせる[QA カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub Copilot app で MCP サーバーを構成する][customize-app]

[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[customize-app]: https://docs.github.com/copilot/how-tos/github-copilot-app/customize-github-copilot-app
