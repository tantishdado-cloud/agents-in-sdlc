---
title: "演習 2 - GitHub Copilot クラウドエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティの問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot のクラウドエージェントは、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せて開発を加速し、開発者がより大きな課題に集中できるようにします。

Copilot クラウドエージェントを使って、次の内容を学びます。

- コード生成環境をカスタマイズする。
- 操作を安全に実行する。
- 範囲を明確に定めたイシューの重要性を理解する。
- Copilot にイシューを割り当てる。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。サイトの初期バージョンを作った請負業者が、ドキュメントを適切に整備しませんでした。実際、ドキュメントはまったくありません。まず、アプリケーションのすべてのエクスポート関数に TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のタイトルを「関連ゲーム」として表示し、閲覧を続けられるようにします。現時点では完成度の高い実装ではなく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロックされていますが、優先度がより高いイシューがほかにあります。

どちらも、優先度が下がりやすいタスクです。Copilot クラウドエージェントに割り当てるのに適しています。クラウドエージェントがバックグラウンドで非同期に作業する間、開発者は別のタスクに集中し、後から Copilot の作業をレビューして期待どおりか確認できます。

## GitHub Copilot クラウドエージェントの紹介

[GitHub Copilot クラウドエージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。たとえば、[GitHub イシューを Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法があります。割り当てると、Copilot は進捗を記録するドラフトのプルリクエストを作成し、環境を用意して作業を始めます。作業中でも完了後でも、Copilot のセッションを詳しく確認できます。提案された解決策をレビューできる状態になると、プルリクエストであなたに通知します。

## 範囲を適切に定めた指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単そうな作業にも、掘り下げると複雑な面があります。[Copilot クラウドエージェントに依頼するタスクの範囲に注意してください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、調整しましょう。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習ではカスタム指示と、その役割を紹介しました。カスタム指示を実際に試したことがあれば、`.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、ここで内容を簡単に見てみましょう。Copilot クラウドエージェントに作業を割り当てる前に、このリポジトリに用意された指示ファイルを読み取り専用で確認して、後からどのような影響があったか分かるようにします。

稼働中の Codespace があればそこで、なければリポジトリの GitHub Web UI で次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで **Code lacks documentation** イシューをクラウドエージェントに割り当てたら、生成された PR にある TSDoc コメント、TypeScript の規約、コメントヘッダーに注目してください。これらの内容は指示ファイルに基づいています。後の演習で PR をレビューするときにも確認します。

## Copilot クラウドエージェントの開発環境をセットアップする

誰がコードを書く場合でも、通常は特定の開発環境や、状態を整えるためのセットアップスクリプトが必要です。ソフトウェアエンジニアのように作業する Copilot にタスクを割り当てる場合も同じです。

クラウドエージェントは、作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定した[特別なセットアップワークフロー][setup-workflow]を作成すると、作業開始前に実行される環境をカスタマイズできます。これにより、必要な開発ツールや依存関係を利用できるようになります。このラボでは、演習を進めやすくするため、あらかじめ設定されています。単一の Astro アプリ用に Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシードが済んだ SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
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

      # ビルドとテストでデータを使えるよう、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローファイルと同じですが、重要な点がいくつかあります。

- `copilot-setup-steps` という 1 つのジョブが含まれています。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブからワークフローを手動実行できるため、Copilot の実行を待たずに正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントが重要だと理解していても、情報が古かったり、そもそも存在しなかったりするプロジェクトは少なくありません。これは、作業効率を下げ、コードベースの保守や新しい開発者の受け入れを難しくする技術的負債です。Copilot はドキュメントの作成を得意としているため、クラウドエージェントに割り当てるのに適したイシューです。バックグラウンドで必要なドキュメントを作成します。後の演習で結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しいイシューのダイアログを開きます。
4. **Blank issue** を選択してイシューを作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択してイシューを作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Copilot をイシューに割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` などになります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が ToDo リストを作成したことを確認します。

> [!NOTE]
> Copilot の ToDo リストが PR に表示されるまで、数分かかる場合があります。Copilot は前に説明したワークフローを実行して環境を作り、プロジェクトを分析して、問題への最適な取り組み方を検討しています。

13. リストと、実行するタスクを確認します。
14. プルリクエストのタイムラインを下へスクロールし、Copilot がイシューの作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された状態が表示されるよう、ウィンドウを更新する必要がある場合があります。

16. ライブセッションをスクロールして Copilot の作業を確認します。コードを調べて状態を理解し、いったん考えて適切な計画を決め、コードを作成していく様子を確認できます。

この作業には数分かかります。Copilot クラウドエージェントの主な目的の 1 つは、タスクを非同期で処理して、ほかの作業に集中できるようにすることです。この機能を活用するため、もう 1 つのタスクもクラウドエージェントに割り当て、その間にアプリケーションの機能を追加するコードを書きます。

## ゲームの詳細ページに関連ゲームを追加する

GitHub Copilot クラウドエージェントの利点の 1 つは、作業を分担できることです。デザインチーム向けに関連ゲームの表示を追加する作業には時間がかからないかもしれませんが、その時間を別の作業に使えます。Copilot クラウドエージェントに割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しいイシューのダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択してイシューを作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Copilot をイシューに割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を開始したことを示す 👀 が、イシューの最初のコメントに表示されます。

![Copilot が 👀 絵文字でイシューの作業中であることを示している](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot クラウドエージェントはソフトウェアエンジニアと同じように作業するため、完了まで常に監視する必要はありません。完了後に結果をレビューします。次はカスタムエージェントの作成と使用を見ていきましょう。

## まとめと次のステップ

このレッスンでは [GitHub Copilot クラウドエージェント][copilot-agents]を確認しました。イシューを Copilot に割り当て、非同期で作業させることができます。技術的負債への対処、新機能の作成、別のフレームワークへのコード移行などに活用できます。

次の内容を学びました。

- コード生成環境をカスタマイズする。
- 操作を安全に実行する。
- 範囲を明確に定めたイシューの重要性を理解する。
- Copilot にイシューを割り当てる。

クラウドエージェントがバックグラウンドで作業している間に、カスタムエージェントを作成して使う方法を確認しましょう。[Copilot クラウドエージェントは MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで扱ったカスタム指示にも対応しています。

## リソース

- [Copilot クラウドエージェントについて][copilot-agents]
- [GitHub イシューを Copilot に割り当てる][assign-issue]
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
