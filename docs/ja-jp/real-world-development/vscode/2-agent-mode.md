---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は、必要なファイルを探し、変更を行い、テストを実行し、問題をデバッグして、変更が漏れたファイルを見つけ、さらに修正します。この作業は何度も続くことがあります。

Copilot のエージェントモードを使うと、この作業を支援できます。

Copilot のエージェントモードは、IDE 内でより自律的に作業します。開発者と同様に、まず既存のプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェントモードを使って、サイトに新しい機能を追加してみましょう。

> [!NOTE]
> エージェントモードとクラウドエージェントは名前が似ていますが、異なる体験を提供します。エージェントモードは IDE 内で動作するため、すばやいフィードバックとやりとりができます。クラウドエージェントはチームメンバーのように非同期で作業し、issue と pull request を介してやりとりします。

この演習では、次の操作を行います。

- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定し、複数のファイルを連携して変更する方法を学びます。
- UI とデータレイヤーにわたって GitHub Copilot エージェントモードで機能を実装します。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューします。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリやパブリッシャーで絞り込めるようにします。前の演習ではパブリッシャーヘルパーを追加しました。次は Copilot エージェントモードで、残りのデータレイヤー、UI、テストを仕上げます。

## Tailspin Toys の Web サイトを実行する

変更を行う前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームを一覧表示し、詳細を表示できます。Web サイトは単一の Astro アプリで、ビルド時にページを静的 HTML として生成します。ページは Drizzle ORM を使って、フロントマターからローカルの SQLite データベースを直接照会します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、標準 DOM API を使う小さな Astro `<script>` でインタラクティブな機能を追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるように、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. 前の演習を終えた Codespace に戻ります。`main` ブランチにいることを確認します。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルウィンドウを開きます。
3. フィルタリング作業用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルの Astro サーバーのアドレス `http://localhost:4321` を開くには、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択します。

> [!NOTE]
> Codespace のターミナルから localhost URL のリンクを選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは、Web サーバーを実行中の Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主なページは次のとおりです。

- **ホームページ**: タイトル、画像、説明とともにボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、publisher、カテゴリなどの詳細ページが表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。演習ごとにモードとモデルの選択が異なる場合があります。必要な場合は、各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善します。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップすると、issue のバックログも自動的に作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェントのドロップダウンから **Agent** を選択します。エージェントは自律的に計画し、複数のファイルを変更し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨するものではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高め、学習体験を保つことが目的です。ただし、LLM の動作は確率的であるため、ワークショップの説明と異なる提案が出る場合があります。これは正常な動作です。

> [!NOTE]
> LLM の動作は確率的であるため、Copilot が別の MCP コマンドを使う場合がありますが、タスクは同じように完了できます。

3. 次のプロンプトを Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が、ユーザーに最も役立つと思われる項目を優先順位付けしたことを確認してください。

## 指示ファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストを伝えるために使用する指示ファイルを確認します。Web サイトへの機能追加の進め方を説明する [ユーザーインターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の進め方に関する全体的な指針を確認します。次の内容が含まれています。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - ファイルの種類に固有の指示へのリンク。対象には次のものがあります。
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、複雑な内容を扱いやすい小さな単位に分け、例やテンプレートを示すことができます。

## フィルタリング機能を実装する

フィルタリング機能を完成させるには、少なくとも次の 3 つの変更が必要です。

- データレイヤー (`src/lib/`) に残りのフィルタリングロジックを追加または改善します。
- フィルタリング動作のテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

さらに、コードベースにマージする前にテストを実行して成功させる必要があります。Copilot エージェントモードに作業を依頼しましょう。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. バックログの関連 issue の要件に基づいて実装するよう、次のプロンプトを Copilot に送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを探す様子を確認します。データレイヤーのヘルパー、UI、テストを見つけた後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトファイルを調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの編集、テストの実行など、複数のタスクを行います。タスクやコードベースの複雑さによっては、数分かかることがあります。作業中、コードエディターに **Keep** と **Undo** が表示される場合があります。Copilot の作業中に選択する必要はありません。完了後、すべての変更に対して **Keep** または **Undo** を選べます。

7. Copilot から求められたら、**Continue** を選択してテストを実行します。

   ![テストの実行を確認する Copilot Chat のダイアログを示すスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に Copilot が一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はタスクを完了してエラーがないことを確認するまで、コード生成とテストを繰り返します。

   ![Copilot エージェントモードの完了した Chat セッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウの **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot エージェントモードを使って Tailspin Toys の Web サイトに機能を追加しました。次の操作を行いました。

- GitHub Copilot エージェントモードで UI とデータレイヤーに機能を実装しました。
- エージェントモードでプロジェクトを調べ、関連ファイルを特定し、連携して変更しました。
- コードベースにマージする前に、エージェントモードが生成した変更とテストをレビューしました。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、pull request を作成します。

### ボーナス演習 — ページ分割を実装する

ゲームの一覧が増えるとページ分割が必要になります。この演習で学んだことを使い、Copilot にページ分割機能の実装を依頼します。次の点を検討してください。

- 指示ファイルを使うなど、既存のベストプラクティスに従います。
- ユーザーがページサイズを選べるようにするか、値を固定するかなど、ページ分割の方法を検討します。
- 希望する実装に必要な情報を Copilot に伝えるプロンプトを作成します。
- Copilot に変更を依頼し、コンテキストを提供しながら反復が必要な場合があります。これは Copilot と作業する通常の流れです。

## リソース

- [Copilot の ask、edit、agent モード: 各モードの違いと使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
