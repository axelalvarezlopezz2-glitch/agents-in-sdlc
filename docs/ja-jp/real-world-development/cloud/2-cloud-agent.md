---
title: "演習 2 - GitHub Copilot クラウドエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot のクラウドエージェントは、コードの更新や機能の追加といったタスクを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト pull request を作成します。面倒な作業を任せて開発を加速し、開発者はより大きな課題に集中できます。

Copilot クラウドエージェントを使って、次の操作を行います。

- コード生成環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- 範囲を明確に定めた issue の重要性を学びます。
- issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には、対応したい技術的負債があります。最初のサイトを開発した委託先は、ドキュメントを十分に作成していませんでした。まず、アプリケーションのすべてのエクスポートされた関数に TSDoc コメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示し、閲覧を続けられるようにします。まだ完成度の高い実装は必要なく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、優先度がより高い別の issue があります。

このようなタスクは優先順位が下がりやすく、Copilot クラウドエージェントに適しています。非同期で作業を進めてもらう間、開発者は別のタスクに集中できます。後から Copilot の作業をレビューし、期待どおりか確認できます。

## GitHub Copilot クラウドエージェントについて

[GitHub Copilot クラウドエージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)は、人間の開発者と同様にバックグラウンドでタスクを実行できます。[GitHub issue を Copilot に割り当てる][start-copilot-sessions]方法など、複数の方法で作業を依頼できます。割り当て後、Copilot は進捗を記録するドラフト pull request を作成し、環境を準備してタスクに取り組みます。作業中でも完了後でも、Copilot のセッションを確認できます。提案された解決策のレビュー準備が整うと、pull request で通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく見ると複雑さがあります。[Copilot クラウドエージェントに依頼するタスクの範囲を適切に定めてください][cloud-agent-best-practices]。AI ペアプログラマーとして、段階的に作業し、学び、試し、必要に応じて調整しましょう。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示と Copilot への影響を確認しました。カスタム指示の演習を完了している場合は、`.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、この機会に簡単に確認しましょう。Copilot クラウドエージェントに作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認してください。後で影響を確認できます。

リポジトリの GitHub Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターで、`**/*.test.ts` ファイルに指示が適用されることを確認します。

次のセクションで `Code lacks documentation` issue をクラウドエージェントに割り当てる際、生成された pull request で TSDoc コメント、TypeScript の規約、コメントヘッダーを確認してください。これらのルールは指示ファイルから適用されます。後の演習で pull request をレビューする際にも取り上げます。

## Copilot クラウドエージェントの開発環境を設定する

コードの作成には、通常、ツールや依存関係をインストールするための開発環境の準備が必要です。SWE と同様に作業を行う Copilot にタスクを割り当てる場合も同じです。

クラウドエージェントは [GitHub Actions][github-actions] を使って環境を構築します。`.github/workflows/copilot-setup-steps.yml` に設定した[専用のセットアップワークフロー][setup-workflow]を作成すると、作業を始める前に必要な開発ツールや依存関係を準備できます。このラボの流れを支援し、学習機会を提供するため、あらかじめ設定されています。単一の Astro アプリのために、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシード済みの SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *各手順*に必要な最小限の権限を設定します。Copilot は操作用の独自のトークンを受け取ります。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
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

      # ローカルの SQLite データベースをマイグレーションしてシードし、ビルドとテスト用のデータを用意します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じ形式ですが、次の点に注目してください。

- `copilot-setup-steps` というジョブが 1 つあります。このジョブは、Copilot が pull request の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントが重要だと誰もが理解していますが、多くのプロジェクトでは情報が古いままだったり、ドキュメントが不足していたりします。そのような技術的負債は対応されないままになり、開発の速度を落とし、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成を得意とするため、クラウドエージェントに割り当てるのに適した issue です。バックグラウンドで必要なドキュメントを作成し、後の演習で作業結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成された pull request (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が TODO リストを作成します。

> [!NOTE]
> Copilot が PR に TODO リストを表示するまで、数分かかることがあります。Copilot は環境を作成し（前述のワークフローを実行）、プロジェクトを分析して、課題に取り組む方法を決めています。

13. TODO リストと、Copilot が完了する予定のタスクを確認します。
14. pull request のタイムラインを下にスクロールして、Copilot が issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が見えるように、ページを更新する必要がある場合があります。

16. ライブセッションをスクロールしながら、Copilot の作業を確認します。コードを調べて状態を理解し、適切な計画を考えて判断し、コードを作成する過程を確認できます。

この作業には数分かかります。Copilot クラウドエージェントの主な利点は、非同期にタスクを実行して別の作業に集中できることです。この利点を活用するため、もう 1 つタスクを割り当ててから、アプリケーションの機能を追加するコードを作成します。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot クラウドエージェントを使うと、作業を分けて、別のタスクに集中している間にほかのタスクを進められます。デザインチーム向けの関連ゲームの追加に時間がかからないとしても、その時間をほかの作業に使えます。クラウドエージェントに割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
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

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業中であることを示す 👀 が issue の最初のコメントに表示されます。

![issue で作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot クラウドエージェントは SWE と同じように作業するので、常に監視する必要はありません。完了後にレビューすればよいのです。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot クラウドエージェント][copilot-agents]について学びました。クラウドエージェントに issue を割り当て、非同期に作業させることができます。技術的負債の解消、新機能の作成、別のフレームワークへのコード移行などに利用できます。

次の概念を確認しました。

- コード生成環境をカスタマイズする。
- 操作を安全に行う方法を確認する。
- 範囲を明確に定めた issue の重要性を学ぶ。
- issue を Copilot に割り当てる。

クラウドエージェントがバックグラウンドで作業している間に、カスタムエージェントの作成と使用に取り組みましょう。[Copilot クラウドエージェントでは MCP サーバー][cloud-agent-mcp]と、前のモジュールで学んだカスタム指示も使用できます。

## リソース

- [Copilot クラウドエージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
[start-copilot-sessions]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
