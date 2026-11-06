---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で実装方針を決め、Autopilot で Issue に基づく絞り込み機能を実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

まずは小さな機能をプロジェクトに追加しました。より大きな変更には、さらに確実なプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って動作するため、適切な方法で必要な機能を構築できます。このレッスンから数回にわたり、一般的なエージェント主導の開発プロセスを実践します。Issue から新機能を作り、コードの有効性と機能の動作を確認して、最後にプロジェクトへマージします。

> [!NOTE]
> 機能開発を続ける間は、同じ会話とブランチを使用します。通常はファイルの種類ごとに異なるブランチや PR を使用しますが、ここでは主な概念に集中できるよう簡略化します。

このレッスンでは、次の操作を行います。

- GitHub Issue から Copilot の会話を開始します。
- Plan モードで要件を決めます。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

この機能の作業を続けながら、リポジトリの命令を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能 PR を作成します。

## シナリオ

Tailspin Toys のカタログが増えてきたため、訪問者がゲームをカテゴリや発行元で絞り込めるようにする必要があります。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意しておくべき詳細があります。Plan モードでこれらを決めてから、範囲を限定した実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本的な原則は変わりません。むしろ、より重要になります。多くの開発者は、次のような流れで作業します。

1. 実装内容を説明する Issue から始めます。
2. 構築する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プル リクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 正確な手順はチームや組織によって異なりますが、多くの場合は上記の流れを調整したものになります。

この標準的な進め方に従うことで、AI が生成したコードが要件を満たし、手書きのコードと同じレビュー プロセスを通るようにできます。

## 会話モード

**会話モード**では、エージェントにどの程度の自律性を与えるかを設定します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントが変更を提案し、次に進む前にユーザーの入力を待ちます。
- **Plan**: エージェントが最初に計画を作成し、プロジェクト ファイルの編集は行いません。
- **Autopilot**: エージェントがコードを作成し、テストを実行して、タスクが完了するまで自律的に反復します。

最初に Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

絞り込み機能の作業を始める前に、Codespace に戻り、リポジトリとターミナルの準備を確認します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、絞り込み機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーを使って **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の Issue やプル リクエスト (PR) をコンテキストに追加できます。

## 絞り込み機能を計画する

計画を立てると、Copilot に作業を依頼する前に機能の実装方法やタスクの進め方を決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan モードに切り替えて、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 前の手順で追加した Issue への参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べて、計画を作成します。

3. 絞り込み機能の動作について質問された場合は、希望に基づいて回答します。決まった正解はありません。
4. 計画ができたら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールして確認します。
6. 自分の決定と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューできたら、実装に進みます。Autopilot を使って Copilot に作業を任せましょう。

Autopilot を使うと、Copilot はタスクが完了したと判断するまで問題に取り組み、反復します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンに表示される同様のオプションを選択します。
2. プロンプトの下のモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画を確認してください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェック完了と判断する前にセットアップの問題を解決してください。

## 実装をレビューして検証する

コードが生成されたら、ほかのコードと同じようにマージ前にレビューします。コードを確認し、サイトを実行して問題がないか確認しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力して、絞り込み機能の実装とテストを確認します。
3. 結果を Issue と計画時に決めた内容と比較します。
4. コードを確認したら <kbd>Esc</kbd> を押して差分ビューを閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新しい機能を試す

コードに問題がないように見えても、実際に動作するでしょうか。前回と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリや発行元による絞り込みと、それらの組み合わせを試します。
4. リセット時の動作と結果がない場合の動作が、Issue と決定内容に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

会話モードを切り替えて機能を構築し、レビューしました。このレッスンでは、次の操作を行いました。

- GitHub Issue から Copilot の会話を開始しました。
- Plan モードで要件を決めました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は[カスタム命令を使って][next-lesson]、コードが文書化された開発プラクティスに従うようにしましょう。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
