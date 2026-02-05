# Codex CLI学習計画

> 作成日: 2026-02-05
> 言語: Codex CLI（日本語）
> 環境: Mac, VSCode

## 学習目標

日々の開発作業を効率化するため、Codex CLIの機能を習得し、実務に直結する安全なワークフローを構築する。

## 進捗管理

各フェーズの完了後、以下のチェックリストを更新してください：

- [ ] フェーズ1: 導入 — 設定と準備
- [ ] フェーズ2: 基礎 — コンテキストの制御
- [ ] フェーズ3: 実践 — 対話実行と承認フロー
- [ ] フェーズ4: 管理 — セッションの保存と再開
- [ ] フェーズ5: 効率 — UIと操作の最適化
- [ ] フェーズ6: 安全 — 実行制御とルール
- [ ] フェーズ7: 拡張 — Agent Skillsの活用
- [ ] フェーズ8: 自動化 — 非対話実行
- [ ] フェーズ9: 外部連携 — Web検索とMCP
- [ ] フェーズ10: 応用 — 実践演習

**確認済み証跡テンプレ（各フェーズの完了時に記録）:**
- 日付: YYYY-MM-DD
- 実施内容: 例) `/status`確認、`codex --version`実行
- 結果: 例) 期待通り/要修正
- メモ: 例) つまずきポイントや次回の改善点

---

## フェーズ1: 導入 — 設定と準備

### 1.0 事前学習（ドキュメントを読む）

**公式ドキュメント:**
- https://developers.openai.com/codex/cli
- https://developers.openai.com/codex/config-basic
- https://developers.openai.com/codex/cli/reference

**読み方（順番と確認観点）:**
1. CLIの基本起動方法とインストール例を確認する
2. `config.toml`の主要オプション（model/approval/sandbox）を把握する
3. CLIフラグで一時上書きできることを理解する

### 1.1 セットアップと設定ファイルの理解

**この項で学習する概要:**
- `~/.codex/config.toml`がユーザー設定であること
- `.codex/config.toml`がプロジェクト設定（信頼済みのみ）であること
- `-c/--config`で一時上書きできること

**なぜやるのか:**
安全な初期設定と、プロジェクトごとの最適化を両立するため。

**学習内容:**
1. `codex`を起動し、ログイン状態を確認する
2. `codex --version`でバージョンを確認する
3. `codex login status`でログイン状態を確認する
2. `~/.codex/config.toml`の存在を確認する
3. `config.toml`で`model`/`approval_policy`/`sandbox_mode`の値を確認する

**実践課題:**
1. `approval_policy = "on-request"`と`sandbox_mode = "workspace-write"`を設定する
2. `model_reasoning_effort = "high"`を設定して効果を体感する
3. `-c web_search="cached"`の一時上書きを試す

**検証:**
1. `/status`で現在の設定を確認する
2. `-c`での上書きが当該セッションのみ反映されることを確認する
3. バージョンとログイン状態が想定通りであることを確認する

**推奨プロンプト例:**
1. 「`/status`で現在の設定を確認して」
2. 「このセッションで有効な設定を要約して」
3. 「`web_search`の現在のモードを教えて」

---

## フェーズ2: 基礎 — コンテキストの制御

### 2.0 事前学習（ドキュメントを読む）

**公式ドキュメント:**
- https://developers.openai.com/codex/guides/agents-md
- https://developers.openai.com/codex/cli/slash-commands

**読み方（順番と確認観点）:**
1. `AGENTS.md`の探索順序とオーバーライド規則を把握する
2. `AGENTS.override.md`が優先される理由を理解する
3. サイズ上限と`project_doc_fallback_filenames`の意味を確認する

### 2.1 AGENTS.mdの運用

**この項で学習する概要:**
- `AGENTS.md`が起動時に読み込まれること
- `AGENTS.override.md`が優先されること
- `/init`で雛形を作成できること

**なぜやるのか:**
プロジェクトの文脈を固定し、毎回の説明コストを下げるため。

**学習内容:**
1. `/init`で`AGENTS.md`を生成する
2. ルートとサブディレクトリで`AGENTS.md`/`AGENTS.override.md`を作り、優先順位を確認する
3. `project_doc_max_bytes`の上限で読み込みが止まることを確認する

**実践課題:**
1. `~/.codex/AGENTS.md`に全体方針（日本語応答など）を追記
2. プロジェクト直下の`AGENTS.md`に具体的な方針（変更理由の明示など）を追記
3. サブディレクトリの`AGENTS.override.md`で一部を上書き

**検証:**
1. 指示の優先順が回答に反映されることを確認する
2. `codex --ask-for-approval never "Summarize the current instructions."`で読み込み順を確認する

**推奨プロンプト例:**
1. 「`/init`で`AGENTS.md`を作って」
2. 「今読み込まれている指示を要約して」
3. 「このディレクトリ固有の指示は何？」

---

## フェーズ3: 実践 — 対話実行と承認フロー

### 3.0 事前学習（ドキュメントを読む）

**公式ドキュメント:**
- https://developers.openai.com/codex/security
- https://developers.openai.com/codex/cli/slash-commands

**読み方（順番と確認観点）:**
1. サンドボックスと承認ポリシーの役割を理解する
2. `--ask-for-approval`と`--sandbox`の意味を確認する
3. `/approvals`の使い方を把握する

### 3.1 承認ポリシーとサンドボックスの理解

**この項で学習する概要:**
- `read-only`/`workspace-write`/`danger-full-access`の違い
- `--full-auto`の実際の設定値は`/status`で必ず確認すること
- `/approvals`で切り替えられること

**なぜやるのか:**
安全性と作業スピードのバランスを取るため。

**学習内容:**
1. `codex --sandbox read-only --ask-for-approval on-request`で起動する
2. `/approvals`で`Auto`と`Read Only`を切り替える
3. `/status`で設定が反映されていることを確認する

**実践課題:**
1. Git管理下ディレクトリではAuto推奨、未管理ではread-only推奨である理由をまとめる
2. `--dangerously-bypass-approvals-and-sandbox`（`--yolo`）が危険である理由を整理する
3. `--sandbox workspace-write`に`network_access`を付与する設定を試す

**検証:**
1. 実行や編集時に期待通り承認が求められることを確認する
2. 設定変更が`/status`に反映されることを確認する

**推奨プロンプト例:**
1. 「`/approvals`でon-requestに切り替えて」
2. 「危険な設定を避ける理由を教えて」
3. 「`/status`で現在の承認ポリシーを表示して」

---

## フェーズ4: 管理 — セッションの保存と再開

### 4.0 事前学習（ドキュメントを読む）

**公式ドキュメント:**
- https://developers.openai.com/codex/cli/features
- https://developers.openai.com/codex/cli/reference
- https://developers.openai.com/codex/cli/slash-commands

**読み方（順番と確認観点）:**
1. `codex resume`の再開方法を確認する
2. `/resume`と`/fork`の用途を理解する
3. セッションIDの参照元を把握する

### 4.1 /resume と /fork の使い分け

**この項で学習する概要:**
- `codex resume --last/--all`で再開できること
- `/resume`で一覧から再開できること
- `/fork`で分岐できること

**なぜやるのか:**
長い作業を安全に中断・再開し、別案も試せるようにするため。

**学習内容:**
1. `codex resume --last`で直近セッションを再開する
2. `/resume`でセッション一覧を確認する
3. `/fork`で別案を試す

**実践課題:**
1. 別テーマの短い会話を2本作る
2. 1本目を`/fork`で分岐させ、異なる方針で依頼する
3. `/status`または`~/.codex/sessions/`でセッションIDを確認する

**検証:**
1. 再開後に文脈が引き継がれていることを確認する
2. 分岐後に元の履歴が保持されていることを確認する

**推奨プロンプト例:**
1. 「`/resume`でセッション一覧を開いて」
2. 「直前のセッションを再開して」
3. 「このセッションを`/fork`して別案を試したい」

---

## フェーズ5: 効率 — UIと操作の最適化

### 5.0 事前学習（ドキュメントを読む）

**公式ドキュメント:**
- https://developers.openai.com/codex/cli/slash-commands

**読み方（順番と確認観点）:**
1. 主要スラッシュコマンドの役割を把握する
2. `/model`でのモデル切替を理解する
3. `/diff`や`/review`の活用タイミングを整理する

### 5.1 スラッシュコマンドの習得

**この項で学習する概要:**
- `/diff`で変更差分を確認できること
- `/review`でレビュー依頼できること
- `/compact`で長い会話を圧縮できること

**なぜやるのか:**
作業速度を上げ、確認漏れを減らすため。

**学習内容:**
1. `/diff`で未追跡ファイルを含む差分を確認する
2. `/review`で指摘を受ける
3. `/compact`で会話を圧縮する

**実践課題:**
1. 小さな変更を依頼し、/diff → /review → /compact の順に実行
2. `/model`でモデルを切り替える
3. `/undo`で直前のターンを戻す
4. `/approvals`が見つからない場合はスラッシュコマンド一覧から確認する

**検証:**
1. `/diff`の内容が実際の差分と一致することを確認する
2. `/review`の指摘が変更内容と一致することを確認する

**推奨プロンプト例:**
1. 「`/diff`で変更を見せて」
2. 「`/review`でレビューして」
3. 「`/compact`で会話を要約して」

---

## フェーズ6: 安全 — 実行制御とルール

### 6.0 事前学習（ドキュメントを読む）

**公式ドキュメント:**
- https://developers.openai.com/codex/exec-policy

**読み方（順番と確認観点）:**
1. `.rules`の配置場所と読み込み条件を理解する
2. `allow`/`prompt`/`forbidden`の優先順位を把握する
3. `codex execpolicy check`でテストできることを確認する

### 6.1 実行ポリシー（exec-policy）の導入

**この項で学習する概要:**
- `.rules`でコマンド許可範囲を制御できること
- ルールは起動時に読み込まれること
- `match`/`not_match`で動作検証できること

**なぜやるのか:**
危険なコマンドの実行を抑止し、安全性を高めるため。

**学習内容:**
1. `~/.codex/rules/default.rules`を作成する
2. `prefix_rule`で`prompt`を指定する
3. `codex execpolicy check`でルール適用を確認する
4. `sandbox_workspace_write`で`network_access`を設定する

**実践課題:**
1. `git`や`gh`などのコマンドに対してルールを追加
2. `match`/`not_match`で想定通り判定されるか確認
3. ルール変更後に再起動が必要なことを確認する
4. `network_access`の有効化が必要な作業を列挙する（例: `git pull`）

**検証:**
1. ルールが期待どおり判定されることを確認する
2. `forbidden`が`prompt`より強いことを確認する

**推奨プロンプト例:**
1. 「exec-policyのルール例を作って」
2. 「`codex execpolicy check`でこのコマンドを検証して」
3. 「危険なコマンドを禁止するルールを提案して」

---

## フェーズ7: 拡張 — Agent Skillsの活用

### 7.0 事前学習（ドキュメントを読む）

**公式ドキュメント:**
- https://developers.openai.com/codex/skills
- https://developers.openai.com/codex/skills/create-skill
- https://developers.openai.com/codex/changelog

**読み方（順番と確認観点）:**
1. `SKILL.md`の必須項目（name/description）を理解する
2. スキルの配置場所（ユーザー/リポジトリ）を把握する
3. `$skill-creator`/`$skill-installer`の使い分けを確認する

### 7.1 スキルの作成と利用

**この項で学習する概要:**
- `$skill-creator`で雛形を作れること
- `~/.codex/skills`や`.codex/skills`に配置できること
- スキルは`$skill-name`で明示的に呼び出せること

**なぜやるのか:**
専門的なワークフローを短縮し、再利用性を高めるため。

**学習内容:**
1. `$skill-creator`を実行してスキルの雛形を作成する
2. `SKILL.md`に`name`/`description`が必要であることを確認する
3. `scripts/`や`references/`の役割を理解する

**実践課題:**
1. レビュー補助スキルを作成する
2. `$skill-installer`で外部スキルを導入する
3. スキル追加後にCodexを再起動して反映を確認する

**検証:**
1. `SKILL.md`の形式が要件を満たすことを確認する
2. `~/.codex/skills`と`.codex/skills`の違いを説明できること

**推奨プロンプト例:**
1. 「`$skill-creator`で簡単なスキルを作って」
2. 「このスキルの`SKILL.md`を作成して」
3. 「`$skill-installer`でスキルを追加して」

---

## フェーズ8: 自動化 — 非対話実行

### 8.0 事前学習（ドキュメントを読む）

**公式ドキュメント:**
- https://developers.openai.com/codex/cli/reference
- https://developers.openai.com/codex/cli/features

**読み方（順番と確認観点）:**
1. `codex exec`の用途を理解する
2. `codex exec resume`の使い方を把握する
3. `--cd`/`--add-dir`で作業範囲を指定できることを確認する

### 8.1 `codex exec`による自動化

**この項で学習する概要:**
- 非対話でタスクを投げられること
- セッション再開ができること
- 作業ディレクトリを制御できること

**なぜやるのか:**
CIや定型作業への組み込みを可能にするため。

**学習内容:**
1. `codex exec "<短い依頼>"`を実行する
2. `codex exec resume --last "<追記依頼>"`を実行する
3. `--cd`で作業ディレクトリを切り替える

**実践課題:**
1. 小さな修正を`codex exec`で実施する
2. 直後に`codex exec resume --last`で追加修正を依頼する
3. `--add-dir`で追加の書き込み許可を与える

**検証:**
1. 再開時に過去文脈が引き継がれていることを確認する
2. 作業ディレクトリが期待通りであることを確認する

**推奨プロンプト例:**
1. 「`codex exec`で簡単なタスクを実行して」
2. 「`codex exec resume --last`で追記して」
3. 「`--cd`で対象ディレクトリを指定して」

---

## フェーズ9: 外部連携 — Web検索とMCP

### 9.0 事前学習（ドキュメントを読む）

**公式ドキュメント:**
- https://developers.openai.com/codex/config-basic
- https://developers.openai.com/codex/mcp

**読み方（順番と確認観点）:**
1. `web_search`の`cached/live/disabled`を理解する
2. MCPの設定方法（CLIとconfig編集）を把握する
3. `enabled_tools`/`disabled_tools`の使い方を確認する

### 9.1 Web検索とMCPの導入

**この項で学習する概要:**
- Web検索はデフォルトで`cached`であること
- `--search`で単発の`live`検索ができること
- `codex mcp add`でMCPサーバーを追加できること
  - 2026-01-28以降はWeb検索が既定で有効（cached）になっていること

**なぜやるのか:**
最新情報や外部ツールの連携を安全に行うため。

**学習内容:**
1. `web_search = "cached"`の動作を確認する
2. `web_search = "live"`に切り替えた時の違いを確認する
3. `codex mcp add <server-name> -- <command>`でMCPサーバーを追加する

**実践課題:**
1. `/mcp`で接続中のサーバーを確認する
2. `enabled_tools`/`disabled_tools`で利用可能ツールを制限する
3. OAuthを使うMCPサーバーに対して`codex mcp login <server-name>`を試す
4. ローカルコマンド連携の最小例を試す（例: ローカルの`rg`をMCP経由で利用）

**検証:**
1. MCP一覧に追加したサーバーが表示されることを確認する
2. Web検索モードの切り替えが反映されることを確認する
3. MCP経由でローカルコマンドが実行できることを確認する

**推奨プロンプト例:**
1. 「`/mcp`で接続状態を確認して」
2. 「Web検索を`live`に切り替えて」
3. 「MCPサーバーを追加して」
4. 「MCP経由で`rg`を実行できるか試して」

### 9.2 MCPの具体例（ローカルツール連携）

**この項で学習する概要:**
- MCPでローカルコマンドを安全に公開し、CLIから利用できること
- `enabled_tools`で許可範囲を最小化できること

**なぜやるのか:**
普段のローカル操作（検索・集計など）をCodexの提案に組み込み、作業を一貫化するため。

**学習内容:**
1. MCPサーバー（ローカルツール連携）を追加する
2. `enabled_tools`で許可するツールを`rg`のみに絞る
3. `/mcp`で有効化状態を確認する

**設定例（config.toml）:**
```toml
[mcp_servers.local-tools]
command = "mcp-server-local-tools"
enabled_tools = ["rg"]
```

**CLIでの追加例:**
```bash
codex mcp add local-tools -- mcp-server-local-tools
```

**実践課題:**
1. 目的に合う検索条件を1つ決める（例: `TODO`の洗い出し）
2. MCP経由で`rg`を実行し、結果を要約させる
3. 結果に基づいて次の作業（修正/整理）を依頼する

**検証:**
1. 期待したファイル/行が検索に含まれることを確認する
2. 余計なツールが実行されないことを確認する

---

## フェーズ10: 応用 — 実践演習

### 10.0 事前学習（ドキュメントを読む）

**公式ドキュメント:**
- https://developers.openai.com/codex/cli/features
- https://developers.openai.com/codex/cli/slash-commands
- https://developers.openai.com/codex/security

**読み方（順番と確認観点）:**
1. `/review`と`/diff`の活用方法を確認する
2. 安全設定と承認フローを再確認する
3. 実務フローを想定して一連の流れを整理する

### 10.1 小さな変更を一気通貫で実施

**この項で学習する概要:**
- Codex CLIで計画→実装→検証→レビューを完結できること

**なぜやるのか:**
実際の開発フローに落とし込むため。

**学習内容:**
1. 変更内容を1段落で説明する
2. 変更方針と影響範囲を確認する
3. 実装→検証→レビューを順に進める

**実践課題:**
1. 小さな機能追加を依頼する
2. `/diff`で差分を確認する
3. `/review`でレビューを受ける
4. 修正が必要なら再実行する

**検証:**
1. 変更内容が期待通りであることを確認する
2. レビュー指摘が改善に反映されることを確認する

**推奨プロンプト例:**
1. 「この変更の計画を出して」
2. 「計画に沿って実装して」
3. 「`/review`でレビューして」

---

## 学習リソース

### 公式ドキュメント
- https://developers.openai.com/codex
- https://developers.openai.com/codex/cli
- https://developers.openai.com/codex/cli/features
- https://developers.openai.com/codex/cli/reference
- https://developers.openai.com/codex/cli/slash-commands
- https://developers.openai.com/codex/config-basic
- https://developers.openai.com/codex/rules
- https://developers.openai.com/codex/exec-policy
- https://developers.openai.com/codex/guides/agents-md
- https://developers.openai.com/codex/mcp
- https://developers.openai.com/codex/skills
- https://developers.openai.com/codex/security
- https://developers.openai.com/codex/changelog
