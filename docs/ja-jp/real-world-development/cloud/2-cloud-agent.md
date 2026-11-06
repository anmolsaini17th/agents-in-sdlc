---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

多くの組織が、何らかの技術的負債に直面しています。未解決のセキュリティ問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能リクエストなどがその例です。GitHub Copilot cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。こうした面倒な作業を任せることで、開発者はより大きな課題に集中し、開発を加速できます。

このレッスンでは、Copilot cloud agent を使って次のことを行います。

- コード生成の環境をカスタマイズします。
- 操作が安全に実行されるようにします。
- Issue の範囲を明確に定義する重要性を確認します。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には解消したい技術的負債があります。最初のサイトを作成した開発者は、ドキュメントを十分に整備していません。実際、ドキュメントがほとんどありません。最初の作業として、アプリケーションのエクスポートされたすべての関数に TSDoc のドキュメント コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリのほかのゲームを表示して、ユーザーが閲覧を続けられるようにします。まだ洗練された実装は必要なく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、ほかに優先度の高い Issue があります。

どちらも、優先度を下げられやすいタスクの例です。Copilot cloud agent に割り当てると、非同期に作業を任せられます。開発者は別のタスクに集中し、後から戻って作業をレビューできます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行します。たとえば、[GitHub Issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると、Copilot は進行状況を追跡するドラフトのプル リクエストを作成し、環境を準備して、タスクに着手します。実行中または完了後に Copilot のセッションを詳しく確認できます。提案された解決策のレビュー準備ができると、プル リクエストで通知されます。

## 範囲を明確にした命令の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単そうな操作でも、詳しく調べると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階的に作業し、学び、試し、必要に応じて調整します。生成 AI を利用しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習では、カスタム命令と、Copilot がその命令を使う方法を確認しました。カスタム命令の演習をすでに行った場合は、`.github/copilot-instructions.md` が機能することを確認できています。まだの場合は、cloud agent に作業を割り当てる前に、リポジトリに含まれている命令ファイルを読み取り専用で確認してください。後でその影響を確認できます。

GitHub の Web UI または起動中の Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクションを確認します。特に TypeScript の規則と TSDoc コメントに関するガイダンスを確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` フロントマターが `**/*.test.ts` ファイルに命令を適用することを確認します。

次のセクションで **Code lacks documentation** Issue を cloud agent に割り当てたら、プル リクエストに TSDoc コメント、TypeScript の規則、コメント ヘッダーが反映されているか確認してください。後のレッスンでプル リクエストをレビューするときにも、これらの命令に注目します。

## Copilot cloud agent の開発環境を設定する

開発者が誰であっても、コードの作成には特定の環境と、状態を整えるセットアップ スクリプトが必要です。ソフトウェア エンジニアのようにタスクを実行する Copilot に作業を割り当てる場合も同じです。

Cloud agent は GitHub Actions を使って作業環境を用意します。作業を始める前に実行する[セットアップ ワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成すると、必要な開発ツールや依存関係を利用できるようにできます。このラボの流れを支援し、学習機会を提供するために、すでに設定してあります。このワークフローは、Copilot が Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、単一 Astro アプリ用に移行およびシードされた SQLite データベースを利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL で構成されています)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるように、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じように見えますが、いくつか重要な点があります。

- `copilot-setup-steps` という 1 つのジョブがあり、Copilot がプル リクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot が実行するまで待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントは重要ですが、更新されていない、またはまったくないプロジェクトも少なくありません。このような技術的負債は放置されやすく、生産性を下げたり、コードの保守や新しいメンバーの受け入れを困難にしたりします。Copilot はドキュメントの作成を得意としているため、cloud agent に割り当てるのに適した Issue です。バックグラウンドで必要なドキュメントを生成してもらい、後の演習で作業をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプル リクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものになります。一覧に PR が表示されない場合は、少し待ってからブラウザー ウィンドウを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかることがあります。Copilot が先ほど説明したワークフローを実行して環境を作り、プロジェクトを分析して、問題への最適な取り組み方を判断しているためです。

13. リストを確認し、Copilot が実行するタスクを確認します。
14. プル リクエストのタイムラインを下へスクロールし、Copilot が Issue の作業を開始したことを確認します。
15. **View session** を選択します。

    ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が見えるよう、ウィンドウの更新が必要な場合があります。

16. ライブ セッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調べて状況を理解し、適切な計画を立て、コードを作成するまでの流れを確認できます。

この作業には数分かかる場合があります。Copilot cloud agent の主な目的は、非同期にタスクを実行して、ほかの作業に集中できるようにすることです。この機能を活用するため、もう 1 つのタスクを cloud agent に割り当ててから、アプリケーションへの機能追加に取り組みます。

## ゲームの詳細ページに関連ゲームのセクションを追加する

先ほど説明したように、Copilot cloud agent の大きな利点の 1 つは、作業を分担できることです。デザイン チーム向けの関連ゲームの追加に時間がかからないとしても、その時間をほかの作業に使えます。Copilot cloud agent に任せましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

その後すぐに、最初の Issue のコメントに 👀 が表示されることがあります。これは Copilot が作業中であることを示します。

![Copilot が Issue を処理中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同じように作業するため、常に監視する必要はなく、完了後にレビューできます。次はカスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に Issue を割り当てると、非同期に作業を実行できます。技術的負債の解消、新しい機能の作成、フレームワーク間のコード移行などに利用できます。

このレッスンでは、次の操作を行いました。

- コード生成の環境をカスタマイズしました。
- 操作が安全に実行されるようにしました。
- Issue の範囲を明確に定義する重要性を確認しました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、次はカスタム エージェントを作成して使用します。[Copilot cloud agent は MCP サーバーも使用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム命令も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
