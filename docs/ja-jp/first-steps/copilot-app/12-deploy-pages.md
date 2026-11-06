---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app で Pages ワークフローを構成し、プル リクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages にデプロイして、Space Quiz を誰でもアクセスできるようにします。Copilot はワークフローを準備し、デプロイをガイドできますが、変更をレビューしてサイトが公開されたことを確認するのはユーザー自身です。

このレッスンでは、次の操作を行います。

- GitHub Pages のデプロイを Copilot に構成してもらいます。
- マージ前にワークフローとプル リクエストをレビューします。
- デプロイの状況を確認し、公開されたサイトを検証します。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、まだ誰でも簡単に試せる状態ではありません。GitHub Pages に単一ファイルのサイトを公開すると、サーバーやランタイム依存関係を追加せずに公開 URL からアクセスできるようになります。

## サイトのデプロイを Copilot に依頼する

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Space Quiz のプル リクエストがマージされた後、Build website、Upload artifact、Deploy to Pages の各ステップが成功した GitHub Actions の Deploy website ワークフローを経て、yourname.github.io/space-quiz/ でライブ サイトが表示されるまでの 3 段階を示す図。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトでは、エージェントにプル リクエストの作成とマージ、およびリポジトリの Pages 設定の変更を依頼します。続行する前に、差分、デプロイ権限、GitHub の承認要求を確認してください。

## ワークフローをレビューしてデプロイを確認する

1. README は、リポジトリにまだ存在しない場合にのみ追加されていることを確認します。
2. ワークフローのビルド手順とデプロイ手順を含め、プル リクエストの変更をレビューします。
3. 必須のプル リクエスト チェックが成功した場合にのみマージします。
4. GitHub Actions のデプロイ実行が成功するまで確認します。
5. 報告された URL を開き、Space Quiz が読み込まれることを確認します。

エージェントが Pages の有効化やプル リクエストのマージを実行できない場合は、エージェントの具体的な指示に従います。デプロイが成功し、ライブ サイトが読み込まれるまで、タスクを完了したと判断しないでください。

## まとめと次のステップ

Space Quiz をライブ サイトとして公開し、エージェントの報告をそのまま信じずにデプロイを確認しました。次は[レッスン 13: レビューと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
