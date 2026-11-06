---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルでエージェントを定義し、焦点を絞った指示、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に与えることで、特定の種類の作業をより効果的に行えるようにします。チームの専門知識を再利用可能なエージェントとして形にできます。たとえば、[WCAG][wcag] 準拠を徹底するアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントなどです。

カスタム エージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、そのエージェントの動作、専門知識、指示を定義する Markdown プロンプトが含まれます。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、役割の重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に操作方法を伝えます。最も分かりやすい違いは、**カスタム エージェントは作業者、スキルはツール**という点です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業の一環としてスキル (さらにはほかのエージェント) をオーケストレーションするよう設計されています。この演習のアクセシビリティ カスタム エージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一部として、プル リクエスト ワークフローのスキルや、テストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI を使うほかの場面と同様に、環境やシナリオに合わせて試行し、調整してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにすることに取り組んでいます。最近、現在のダーク テーマではテキストと背景色のコントラストが不十分で、読みにくいと感じるユーザーがいるというフィードバックが寄せられました。このアクセシビリティの課題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードの実装を依頼しました。

アクセシビリティは重要なので、できるだけ早く実装する必要があります。カスタム エージェントを使って機能を生成します。

この演習では、次の操作を行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントは、すでに作成されています。Copilot をどのように導くか理解するため、内容を確認しましょう。

Codespace に戻り、ターミナルを開いて、アクセシビリティ作業用に `main` から新しいブランチを作成して切り替えます (演習 3 の絞り込み PR とは別に作業します)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを確認します。次の内容が記載されています。
   - アクセシブルな Web サイトのコード生成を担う主要な責務。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある落とし穴や間違い。

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント ドロップダウンには、`.github/agents` で定義されたすべてのカスタム エージェントが表示されます。カスタム エージェントを選択すると、そのエージェントの指示とツールを使うよう、チャット セッションを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

1. Chat ビューのエージェント ドロップダウンで、まだ選択されていない場合は **Agent** を選択します。

   ![Chat ビューでエージェント ピッカーを表示したスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部にあるエージェント ドロップダウンを選択します (現在のエージェントが表示されます。既定では **default** です)。
3. 使用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティ エージェントに次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat が作業を始めます。コードベースを調べて変更を提案し、プロジェクト ファイルを編集します。編集内容は、ファイル パスと差分が表示された状態でチャットにインライン表示され、確認できます。
6. 編集が適用されると、エージェントが変更した作業ファイルを示す **Files changed** の表示が更新されます。

> [!NOTE]
> この作業には数分かかることがあります。Copilot はリポジトリに実際の変更を加えます。作業中、Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集します。

進行中の作業のレビューと指示は、次の演習で行います。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に特化した AI アシスタントである、GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。カスタム エージェントを使うと、チームの専門知識や標準を再利用可能なエージェントとして形にし、Copilot が作業をより効果的に行えるよう導けます。

この演習では、次のことを学びました。

- カスタム エージェントの定義方法。
- Copilot Chat のエージェント モードでカスタム エージェントを使う方法。

次は[エージェントの作業を監視して指示し][next-lesson]、変更を確認しながら、同じセッションにライト モードの切り替え機能を追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントを監視して管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
