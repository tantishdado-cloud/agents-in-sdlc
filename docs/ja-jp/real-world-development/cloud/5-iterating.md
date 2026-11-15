---
title: "演習 5: GitHub Copilot の作業を改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザーエクスペリエンスの改善や機能追加について、GitHub Copilot といくつかのタスクに取り組みました。コードにドキュメントを追加し、デザインチームが改善できる関連ゲーム機能を作成し、ハイコントラストモードとライトモードを切り替えるアクセシビリティ機能を実装するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善しましょう。

### シナリオ

これまでに何度も説明したように、生成 AI を導入しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードは必ずレビューし、通常の DevOps プロセスに沿って作業します。チーム全員にレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が作成した提案を確認しましょう。

## セキュリティと GitHub Copilot クラウドエージェント

Copilot クラウドエージェントは非同期かつ監視されずにタスクを実行するため、安全を保つためにセキュリティ上の制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスはコードの作業に使うブランチに限られます。
- クラウドエージェントは GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローの実行には、人間による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは、デフォルトで制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot クラウドエージェントが作成した、コードへのドキュメント追加に関するプルリクエスト (PR) を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされているという警告が表示されることがあります。Copilot の外部リソース (外部 MCP サーバーへの呼び出しを含む) へのアクセスは、デフォルトで制限されているため、これは**想定どおりの動作**です。必要であれば、[Copilot クラウドエージェントのファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエスト一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストに **[WIP]** が付いています。その場合は、Copilot が作業を終えるまで待ちます。数分かかることもあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. プルリクエストの準備ができたら、**Files changed** タブを選択して変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新たに更新されたコードを確認します。TSDoc のドキュメントコメントなどが追加されています。実際の変更は異なる場合があります。

   変更を確認するときは、TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーに注目してください。これらの内容は、演習 2 の最初に確認したカスタム指示ファイルに基づいています。

6. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下へスクロールします。
7. ワークフローの承認待ちを示すメッセージがあることを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot とやり取りする方法は、一方通行ではありません。プルリクエストのコメントやコードのインラインコメントで、チームの他のメンバーと同じように Copilot をタグ付けできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は予測できないため、必ず特定の文面を使うようには指定できません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、内容を簡潔に説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例などを参考に、コードベースに追加したいドキュメントの変更をコメントで依頼し、他のユーザーと同じように **@copilot** をタグ付けします。
2. **View Session** を選択し、Copilot が作業する様子を確認します。更新のために新しいセッションが始まります。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

   ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで改善を繰り返せます。問題がなければ、PR をドラフトからレビュー可能な状態に変更して、メインブランチにマージします。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲームの詳細ページに関連ゲームを表示するイシューに対して、Copilot が作成した PR を確認しましょう。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して生成されたコードをレビューします。
5. 更新内容に問題がなければ、**Conversation** タブに戻って下へスクロールします。
6. ワークフローの承認待ちを示すメッセージがあることを確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えてください。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。前と同じようにクラウドエージェントに変更を依頼して、反復的に改善できます。たとえば、表示する関連ゲームの数を変更したり、コメントヘッダーや TSDoc のドキュメントコメントを追加したりできます。(このイシューはカスタム指示を更新する前に割り当てたことを思い出してください。) 前と同じように、**Conversation** タブでコメントを追加すると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントが実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 のミッションコントロールで追加を依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストに **[WIP]** が付いています。その場合は、Copilot が作業を終えるまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認するときは、特に次の点に注意します。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保存するためのローカルストレージの利用
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容に問題がなければ、**Conversation** タブに戻って下へスクロールします。
7. ワークフローの承認待ちを示すメッセージがあることを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名に置き換えてください。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードを切り替えて、期待どおりに動作し、ページを再読み込みしても設定が維持されることを確認します。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに沿って機能を実装するよう Copilot をガイドしたことを確認できます。アクセシビリティに関する懸念や改善点があれば、前の PR と同様にコメントで **@copilot** をタグ付けして変更を依頼できます。

## 任意の演習 — 委任を続ける

実際のバックログ作業をエージェントに任せ、自分は別のことに取り組めるとき、クラウドエージェントの力を活用できます。習慣を身につけるため、リポジトリにイシューをいくつか追加し、Copilot に割り当ててみましょう。たとえば、次のような内容があります。

- ゲームの詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

クラウドエージェントの実習環境を完了しました。各レッスンでは、次のことを行いました。

- **このリポジトリに用意されたカスタム指示ファイルを確認し、後からクラウドエージェントの出力への影響を確認できるようにしました。**
- **Copilot クラウドエージェントにイシューを割り当て、環境のセットアップ、計画、非同期の実行を確認しました。**
- **アクセシビリティ用カスタムエージェントを作成して使い、ハイコントラストモードとライトモードの切り替えを追加しました。**
- **Copilot エージェントページをミッションコントロールとして使い、進行中のアクセシビリティセッションを監視して方向付けしました。**
- **クラウドエージェントのプルリクエストをレビューして改善し、`@copilot` をタグ付けして変更を依頼し、ワークフローを承認しました。**

## レビューと次のステップ

クラウドエージェントの実習環境を完了しました。学んだ内容をさらに広げるには、次の実習環境も確認してください。

- 🖥️ **[VS Code 実習環境](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI 実習環境](../../cli/)** — Plan モードと Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、任意の `/delegate` によるクラウドエージェントとの連携を使って、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリで、次の内容にも取り組んでみましょう。

- リファクタリングやテストカバレッジのイシューをクラウドエージェントに割り当てる。
- 別の分野を担当するカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub イシューを Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot クラウドエージェントのファイアウォールを設定する][agent-firewall]

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
