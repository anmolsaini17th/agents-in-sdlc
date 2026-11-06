---
title: "演習 2 - Copilot エージェント モードで新しい機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの最も簡単な更新でも、複数のファイルを変更し、テストを実行するなどの作業が必要になることがあります。開発者は通常、必要なファイルを探して変更し、テストを実行し、問題をデバッグして、更新し忘れたファイルを見つけ、さらに変更を加えるといった流れで作業します。必要な作業は次々に増えていきます。

このような場面で役立つのが Copilot のエージェント モードです。

Copilot のエージェント モードは、IDE でより自律的に作業できるように設計されています。既存のプロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行して、見つかった問題を自動的に修正する点で、開発者と同様に動作します。エージェント モードを使って Web サイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる利用方法を想定しています。エージェント モードは IDE 内で動作し、すばやいフィードバックと対話を可能にします。Cloud agent はチームの一員のように非同期で作業し、Issue やプル リクエストを通じて連携します。

この演習では、次の操作を行います。

- Copilot エージェント モードでプロジェクトを調べ、関連ファイルを特定して、複数ファイルにまたがる変更を行います。
- GitHub Copilot エージェント モードで UI とデータ レイヤーにまたがる機能を実装します。
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューします。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリや発行元で絞り込めるようにします。前の演習では発行元ヘルパーを追加しました。次は、Copilot エージェント モードを使って、残りのデータ レイヤー、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者向けテーマのボード ゲームを扱うクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示できます。単一の Astro アプリで、ビルド時にページを静的 HTML として生成します。各ページの frontmatter から Drizzle ORM を使い、ローカル SQLite データベースを直接クエリします。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、インタラクティブな機能は標準 DOM API を使う小さなスコープ付き Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` タスクを実行すると、単一の Astro 開発サーバーが起動します。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習を終えて `main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 絞り込み機能の作業用にブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を含むバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは Web サーバーをホストしている Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: タイトル、画像、説明とともにボード ゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細ページが表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

Web サイトの初期実装は動作しますが、さらに機能を追加したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップすると、Issue のバックログが自動的に作成されます。GitHub Copilot に、作成済みの項目を表示してもらいます。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。エージェントは自律的に計画を立て、複数のファイルを変更し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューでエージェント ピッカーを表示したスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 使用できるモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨するものではありません。このワークショップは Claude Sonnet 4.5 を使って作成したため、手順に含めています。コード提案の一貫性を高め、学習しやすくすることを意図しています。ただし、大規模言語モデル (LLM) は確率的に動作するため、提案は手順の例と異なる場合があります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスク自体は実行できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログの Issue を確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーに最も役立つ項目をもとに優先順位も付けたことを確認します。

## 命令ファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストを与える命令ファイルを確認しましょう。Web サイトに機能を追加する方法を説明する [ユーザー インターフェイス (UI) 命令ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加のガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト可能性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle などのファイルに関する命令へのリンク。

> [!TIP]
> 命令ファイルから、ほかの命令ファイルやプロジェクト内のファイルを参照できます。パスは命令ファイルを基準とします。こうすることで、複雑な命令を再利用しやすい小さなファイルに分け、例やテンプレートを提示できます。

## 絞り込み機能を実装する

絞り込み機能を完成させるには、アプリケーションに少なくとも 3 つの更新が必要です。

- データ レイヤー (`src/lib/`) に残りの絞り込みロジックを追加または調整します。
- 絞り込み動作のテストを追加または更新します。
- ゲーム一覧ページに絞り込み UI を追加します。

さらに、すべてのテストを実行して成功を確認してからコードベースにマージする必要があります。Copilot エージェント モードに作業を任せましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、命令ファイルを追加します。

   ![UI 命令ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェント ピッカーを表示したスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関係するファイルを見つける様子を確認します。データ レイヤー、UI、テストを調べて、ファイルを変更し、テストを実行します。

   ![Copilot がプロジェクト ファイルを確認する様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、変更、テスト実行などを行うため、タスクやコードベースの複雑さによっては数分かかります。作業中、コード エディターに **Keep** と **Undo** が表示されることがあります。作業が完了すると、すべての変更に **Keep** または **Undo** の選択肢が表示されます。作業中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテストを実行してよいか確認するダイアログを示すスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に待ち時間が発生したり、テストが失敗したりする場合があります。問題ありません。Copilot はコード生成とテストを繰り返し、タスクが完了してエラーが見つからなくなるまで作業します。

   ![Copilot エージェント モードのチャット セッションが完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いたブラウザーに戻り、新しい機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot エージェント モードを使って Tailspin Toys の Web サイトに機能を追加しました。次の操作を行いました。

- GitHub Copilot エージェント モードで UI とデータ レイヤーにまたがる機能を実装しました。
- エージェント モードでプロジェクトを調べ、関連ファイルを特定し、複数ファイルにまたがる変更を行いました。
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューしました。

次は[Playwright MCP サーバーで機能をテストし][next-lesson]、プル リクエストを作成します。

### ボーナスの探索演習 - ページネーションを実装する

ゲームの一覧が増えると、ページネーションが必要になります。この演習で学んだ方法を使い、Copilot にページネーションを追加するよう依頼します。次の点を検討してください。

- 既存の命令ファイルを使うなど、既存のベスト プラクティスに従います。
- ユーザーがページ サイズを選べるようにするか、固定にするか検討します。
- 希望する実装に必要なガイダンスを Copilot に伝えるよう、プロンプトを作成します。
- Copilot に変更とコンテキストを伝え、反復が必要になる場合があります。これは Copilot と作業するときの通常の流れです。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
