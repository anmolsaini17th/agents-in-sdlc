---
title: "レッスン 2 - 星評価の追加: すぐにできる改善"
description: "Copilot CLI でゲーム カードを変更し、転送されたブラウザーで確認して、最初のプル リクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、次はプロジェクトに最初の変更を加えます。小さな変更から始めましょう。ゲームのデータには星評価がありますが、ホーム ページのゲーム カードにはまだ表示されていません。エージェントに評価を表示させ、変更をレビューし、最初のプル リクエストとしてマージします。

このレッスンでは、次の操作を行います。

- 機能ブランチで Copilot の会話を開始します。
- エージェントに小さな変更を依頼します。
- `/diff` で変更をレビューします。
- アプリを実行し、転送されたブラウザーで変更を確認します。
- 最初のプル リクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲームの詳細ページにはすでに表示されています。一方、ホーム ページのゲーム カードに表示されるのはタイトル、カテゴリ、発行元、説明だけです。練習として、各カードに既存の評価を表示するようエージェントに依頼します。小さく完結した変更なので、最初のセッションに適しています。

## 会話の構成

**会話**では、Copilot CLI を使ってタスクに取り組みます。Copilot app とは異なり、通常の CLI 会話では、ターミナルで現在チェックアウトしているリポジトリと Git ブランチを使用し、専用の worktree は作成しません。保存した会話を再開できますが、ファイルとブランチは通常の Git の状態としてディスク上にあります。

会話では、プロンプトとエージェントの応答、ファイルを調査して編集するエージェントのツール操作、`/diff` で確認できる変更の 3 つを確認できます。

## 会話を開始して変更を依頼する

機能の実装に向けて、新しい会話を始めましょう。

1. Codespace に戻ります。
2. まだターミナルを開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. Copilot を起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュ コマンド `/new` を実行して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot がプロジェクトを調べ、ゲームの詳細を表示するファイルを見つけて、必要なコードを作成します。Copilot CLI で新機能を追加できました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はすべてマージ前にレビューします。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. ゲーム カードに評価がある場合は数値が、`starRating` が `null` の場合は `No rating yet` が表示されることを確認します。
3. どちらの状態もテストされていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> Copilot を含む生成 AI ツールは確率的に動作するため、生成されるコードは毎回異なる場合があります。特定の実装を期待するのではなく、動作を確認してください。

## 変更を確認する

コードを読んだだけで動作すると判断するのではなく、Copilot に Web サイトを起動してもらい、Codespaces で転送されたブラウザーから更新後のユーザー インターフェイス (UI) を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択します。
3. ゲーム カードに 5 段階の評価が表示されていることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプル リクエストを作成してマージする

機能を作成できました。次はプル リクエスト (PR) を作成し、新しいコードをプロジェクトにマージします。

1. 既定のエージェントに、変更をコミットするよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を実行します。Copilot CLI は PR の作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. Copilot CLI に表示された URL を <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、PR を開きます。
4. 変更されたファイルとチェックをレビューします。
5. 準備ができたら **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

おめでとうございます。GitHub Copilot CLI を使って最初の変更をリリースしました。具体的には、次の操作を行いました。

- Copilot の会話を機能ブランチで開始しました。
- エージェントにゲーム カードの小さな変更を指示しました。
- `/diff` で変更をレビューしました。
- アプリを実行し、転送されたブラウザーで星評価を確認しました。
- 最初のプル リクエストを作成してマージしました。

次は[絞り込み機能の Issue から始め、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を実装します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
