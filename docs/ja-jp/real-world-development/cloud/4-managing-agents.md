---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでの 2 つの演習で、ユーザー エクスペリエンスの改善や機能追加に関する 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。[GitHub.com のエージェント ページ][agents-page]もその 1 つです。この mission control では、オープンなプル リクエスト (PR) があるすべてのエージェント タスクを確認できます。実行中のセッションで行われた操作を確認し、作業の方向を調整することもできます。

このレッスンでは、次の操作を行います。

- エージェント ページを使って cloud agent のタスクを監視します。
- 実行中のセッションに追加の機能を依頼し、作業の方向を調整します。

## シナリオ

エージェントにハイ コントラスト モードの作成を依頼した後、チームはライト モードも追加するよい機会だと考えました。サイトのスタイルを更新して切り替え機能を追加しているため、同時に実装するのが合理的です。エージェントにライト モードも追加するよう指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てられたタスクの現在の状態を確認しましょう。

1. [Copilot エージェントのページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインの両方に表示されるタスクの一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントの更新。
   - ゲームを更新する API の生成。
   - Web サイトへのハイ コントラスト モードの追加。
3. 実行中のタスクを選択し、Copilot が行った操作を確認します。たとえば、次の操作があります。
   - リポジトリのチェックアウト。
   - Copilot の作業環境の作成。
   - MCP サーバーのセットアップ。
   - 割り当てられたタスクを完了するためのさまざまな作業。

> [!NOTE]
> 表示される手順は、Copilot の作業の状態や実行方法によって異なります。

4. 右側に表示される PR ペインも確認します。ここでは、PR や変更されたファイルを確認できます。

## Cloud agent の作業の方向を調整する

実行中のタスクを確認したので、エージェントがハイ コントラスト モードを実装している間に、ライト モードも追加するよう依頼します。

1. ハイ コントラスト モードのタスクに関連するセッションを選択します。エージェントが選ぶタイトルや作業の状態によって、正確なタイトルは異なります。

   ![Mission control に表示されるアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで数分待ちます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![cloud agent タスクの画面で、作業中のセッションを誘導するダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に追加することを確認します。

## Copilot に作業を続けてもらう

前回と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を終えた後、新しい要求を追加します。

この作業には数分かかります。これまでに学習した内容を振り返りながら待ちます。

## まとめと次のステップ

Copilot cloud agent のタスクを監視して方向付ける中心的な場所である、Copilot エージェントのページを確認しました。この mission control では、進行中と完了済みのタスクを追跡し、実行中の作業をレビューし、必要に応じて作業範囲の変更や追加のガイダンスを指示できます。

このレッスンでは、次の操作を行いました。

- cloud agent のタスクを監視するために mission control とエージェント ページを確認しました。
- 実行中のセッションに追加の機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は Copilot が作成した[プル リクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot エージェントのページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
