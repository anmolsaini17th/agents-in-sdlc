---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善や機能追加に関する複数のタスクを GitHub Copilot に依頼しました。コードへのドキュメント追加、デザイン チームが改善を検討する関連ゲーム機能の作成、ハイ コントラスト モードとライト モードを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善しましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに従います。チームのレビューを有効にする前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の提案を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、直接の監督を受けずにタスクを実行するため、安全を保つためのセキュリティ制約が設けられています。たとえば、次のような制約があります。

- Copilot はリポジトリを読み取れますが、書き込みできるのはコード用に使うブランチ**のみ**です。
- Cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions のワークフローを実行するには、人間による承認が必要です。
- MCP サーバーを含め、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が作成した、コードにドキュメントを追加するプル リクエスト (PR) を確認します。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するときに、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は MCP サーバーなどの外部リソースへのアクセスが既定で制限されているため、これは**想定される動作**です。必要であれば、[Copilot cloud agent のファイアウォールを構成または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation**、またはそれに似たタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあるため、休憩したり、これまでに学習した内容を振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc のドキュメント コメントや、ほかのドキュメントを含むコードを確認します。実際に行われた変更は状況によって異なります。

   変更を確認するときは、TSDoc のドキュメント コメント、TypeScript の規則、コメント ヘッダーに注目してください。これらは演習 2 の最初に確認したカスタム命令ファイルから反映されています。

6. 更新内容をレビューして問題がなければ **Conversation** タブに戻り、下へスクロールします。
7. ワークフローの承認待ちを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションでワークフローが実行待ちになっていることを確認します。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プル リクエストで Copilot と作業するのは一方通行ではありません。チームのメンバーに依頼するのと同じように、プル リクエストやコードのインライン コメントで Copilot に言及できます。Copilot はコメントを認識し、対応するための新しいセッションを開始します。結果は確定的ではないため、依頼する内容を固定することはできません。たとえば、次のような改善を依頼できます。

- 各コード ファイルの先頭に、ファイルの役割を説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメント コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. **@copilot** をユーザーと同じようにタグ付けして、生成されたドキュメントの変更をコメントで依頼します。上記の例を使うか、コードベースに必要な別のドキュメントの改善を提案します。
2. **View Session** を選択し、Copilot が作業を進める様子を確認します。更新を行う新しいセッションが始まります。
3. **Back to pull request** を選択すると、PR に戻れます。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の変更が完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更をレビューします。

必要であれば、納得できるまで反復して改善します。準備ができたら、ドラフト PR をレビュー可能な状態にして `main` ブランチにマージします。

![PR をレビュー可能な状態にする操作](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲームの詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page**、またはそれに似たタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下へスクロールします。
6. ワークフローの承認待ちを示す表示を探します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションでワークフローが実行待ちになっていることを確認します。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
9. **省略可能:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えます。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を実装しました。前と同じように、Copilot cloud agent にコメントを追加して更新を依頼し、反復して改善できます。たとえば、表示する関連ゲームの数を変更したり、(カスタム命令を更新する前に割り当てた作業であることを踏まえて) コメント ヘッダーや TSDoc コメントを追加したりできます。前回と同じように、**Conversation** タブにコメントを追加すると、Copilot が内容を確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントで実装した機能をレビューします。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 の mission control で追加依頼したライト モードの両方が含まれています。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website**、またはそれに似たタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。その場合は、完了するまで数分待ちます。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認するときは、特に次の点に注目します。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定を保存するためのローカル ストレージの使用方法。
   - ハイ コントラスト モードとライト モードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下へスクロールします。
7. ワークフローの承認待ちを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションでワークフローが実行待ちになっていることを確認します。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
10. **省略可能:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名に置き換えます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    続いてアプリケーションを起動し、ブラウザーでハイ コントラスト モードとライト モードの切り替えをテストします。ページを再読み込みしても設定が保持されることも確認します。

アクセシビリティ カスタム エージェントが、アクセシビリティのベスト プラクティスに従って Copilot の実装をガイドしたことを確認します。問題や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 — 委任を続ける

Copilot cloud agent は、バックログの実際の項目を任せて別の作業に集中できるときに役立ちます。習慣を身につけるために、リポジトリにいくつか Issue を作成して Copilot に割り当ててみてください。次のような内容が考えられます。

- ゲームの詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent のハーネスを完了しました。このレッスンでは、次の内容を学びました。

- このリポジトリに含まれるカスタム命令ファイルを確認し、cloud agent の作業に与える影響を確認しました。
- Copilot cloud agent に Issue を割り当て、環境を準備し、計画を立て、非同期に実行する様子を確認しました。
- アクセシビリティ用のカスタム エージェントを作成して使用し、ハイ コントラスト モードとライト モードを追加しました。
- Copilot エージェントのページを mission control として使い、進行中のアクセシビリティ セッションを監視して方向付けました。
- Copilot のプル リクエストをレビューして反復し、**@copilot** に更新を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent のハーネスを完了しました。学んだ内容をさらに深めるには、別のハーネスも役立ちます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェント モードと MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェント スキル、QA カスタム エージェント、Playwright MCP、Agent Merge、cloud agent への `/delegate` を使い、ターミナルから 2 つの変更をレビューしてリリースします。

自分のリポジトリでは、次のアイデアも試せます。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てます。
- 別の分野のカスタム エージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
