---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有のバックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更を計画し、コードを作成して動作をテストし、プル リクエストをレビューし、エージェントを使って作業を管理します。

## シナリオ

架空の会社である Tailspin Toys に、新しい開発者として参加します。Tailspin Toys は、ボードゲームのクラウドファンディング サービスを提供しています。チームのバックログは GitHub Issue として登録済みで、作業を始められる状態です。バックログには、絞り込みやページネーションなどの機能だけでなく、アクセシビリティやコーディング標準などの品質改善も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、さまざまな作業環境で利用できます。自分の開発方法に合う環境を選んでください。各ワークショップは、それぞれ独自のセットアップ手順から始まり、共通の Tailspin Toys シナリオに取り組みます。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、命令、スキル、カスタム エージェント、Playwright MCP、Agent Merge、実用的なスラッシュ コマンドを活用します。

### [GitHub Copilot app][app]

デスクトップ アプリでエージェント セッションを並列に実行し、セッション モードを切り替え、Canvas で共同作業を行い、GitHub Issue とプル リクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業をガイドし、進行状況を監視して、エージェントが作成したプル リクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat のエージェント モード、MCP サーバー、カスタム エージェントを利用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
