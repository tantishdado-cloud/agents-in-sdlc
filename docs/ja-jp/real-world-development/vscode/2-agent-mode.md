---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションを少し更新するだけでも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は、必要なファイルを見つけ、変更を行い、テストを実行し、デバッグし、変更し忘れたファイルを探し、さらに更新するという作業を繰り返します。

こうした作業に役立つのが Copilot エージェントモードです。

Copilot エージェントモードは、IDE 内でより自律的に作業します。開発者と同じように既存のプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行して、見つかった間違いを自動的に修正します。エージェントモードを使って Web サイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードとクラウドエージェントは異なる用途向けです。エージェントモードは IDE 内で動作するため、すばやいフィードバックと対話を通じて作業できます。クラウドエージェントはチームメンバーのように非同期で作業し、イシューとプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot エージェントモードがプロジェクトを調べ、関連ファイルを特定し、複数の変更を調整する方法。
- GitHub Copilot エージェントモードで UI とデータレイヤーに新機能を実装する方法。
- Copilot エージェントモードが生成した変更とテストをレビューしてからコードベースにマージする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと販売元で絞り込めるようにしたいと考えています。前の演習では販売元ヘルパーを追加しました。次は Copilot エージェントモードを使い、残りのデータレイヤー、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームを一覧表示し、その詳細を確認できます。単一の Astro アプリで、ビルド時に静的 HTML としてページをレンダリングします。ページは Drizzle ORM を通じてフロントマターからローカル SQLite データベースを直接参照します。独立したバックエンド API やクライアントサイド UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、必要なインタラクティブ機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるように、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。次の手順で GitHub Codespace から実行できます。

1. Codespace に戻ります。前の演習の後なので、`main` にいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルウィンドウを開きます。
3. 絞り込み機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro の開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を選択します。選択するとき、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押したままにします。

> [!NOTE]
> Codespace のターミナルで localhost URL のリンクを選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Web サーバーを実行している Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主なページは次のとおりです。

- **ホームページ**: タイトル、画像、説明とともにボードゲームを一覧表示します。
- **ゲームの詳細ページ**: ゲームを選択すると、タイトル、説明、販売元、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善しましょう。まずバックログを確認します。テンプレートからプロジェクトをセットアップしたとき、イシューのバックログも自動的に作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェントのドロップダウンから **Agent** を選択します。**Agent** は、ファイルをまたぐ変更を自律的に計画・実装し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨する意図はありません。ワークショップ作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高め、学習しやすくすることを目指しています。ただし、LLM の動作は確率的であるため、ワークショップに記載された内容と異なる提案が出る場合があります。これは通常の動作であり、想定されています。

> [!NOTE]
> LLM の確率的な性質により、Copilot は別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、イシューのバックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべてのイシューを一覧表示するコマンドを実行します。
5. 生成されたイシュー一覧を確認します。

Copilot が、ユーザーに最も役立つと考えた内容に基づいて優先順位も付けていることを確認します。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、Copilot にコンテキストを与える指示ファイルを確認しましょう。Web サイトに機能を追加する方法を説明する[ユーザーインターフェイス (UI) の指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - 次のようなファイル形式別の指示へのリンク。
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、複雑な内容を管理しやすい単位に分け、Copilot に例やテンプレートを示せます。

## 絞り込み機能を実装する

絞り込み機能を完成させるには、少なくとも次の 3 つを変更する必要があります。

- データレイヤー (`src/lib/`) に残りの絞り込みロジックを追加するか、調整する。
- 絞り込み動作のテストを追加または更新する。
- ゲーム一覧ページに絞り込み UI を追加する。

さらに、コードベースにマージする前にテストを実行して成功させる必要があります。Copilot エージェントモードにこれらの作業を依頼しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. バックログ内の関連イシューに基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを探す様子を確認します。データレイヤーのヘルパー、UI、テストを見つけてファイルを変更し、テストを実行します。

   ![Copilot がプロジェクトファイルを調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの編集、テストの実行などを行います。タスクやコードベースの複雑さによっては数分かかります。作業中、コードエディターに **Keep** と **Undo** ボタンが表示される場合があります。作業が完了すると、変更ごとに **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![テスト実行の確認を求める Copilot Chat ペインのダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はタスクを完了してエラーがないと判断するまで、コード生成とテストを繰り返します。

   ![Copilot エージェントモードの完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を試します。
11. すべてが動作し、コードをレビューできたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot エージェントモードを使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot エージェントモードで UI とデータレイヤーに新機能を実装する方法。
- Copilot エージェントモードがプロジェクトを調べ、関連ファイルを特定し、複数の変更を調整する方法。
- Copilot エージェントモードが生成した変更とテストをレビューしてからコードベースにマージする方法。

次は[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の探索演習 — ページ分割を実装する

ゲーム一覧が増えると、ページ分割が必要になります。この演習で学んだことを使い、Copilot にページ分割を追加するよう依頼してみましょう。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定にするかなど、ページ分割の方法を検討します。
- 希望する実装に必要なガイダンスをプロンプトで Copilot に伝えます。
- Copilot にコンテキストを与えて変更を依頼し、何度かやり取りすることがあります。Copilot との作業では通常の流れです。

## リソース

- [Copilot の Ask、Edit、Agent モード: それぞれの役割と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
