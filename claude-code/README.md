# Claude Code

作成日: `2026-05-03`

## 1. 概要

Claude Code は Anthropic が提供するターミナルベースの AIコーディングエージェントです。
コードの読み書き、CLI 実行、テスト実行、DB 操作、Cloud 操作まで一貫して行えます。

このスタックでは **技術作業の第一候補** として位置づけます。

---

## 2. 主な特徴

| 特徴 | 内容 |
| --- | --- |
| CLI ネイティブ | ターミナル上で動作し、コマンド実行・ログ確認まで完結する |
| 複数ファイル修正 | Python / Rust の複数ファイルにまたがる修正が得意 |
| コード理解 | 既存コードベースを読んで設計・修正・テストを提案できる |
| DB 対応 | PostgreSQL migration、SQL レビュー、テストデータ作成 |
| Cloud 対応 | Terraform plan 確認、AWS / GCP 操作支援 |
| ML / MLOps 対応 | scikit-learn / LightGBM / Vertex AI pipeline 調査・修正 |

### 得意な業務

- `Python / Rust` の batch 処理・backend 実装
- `PostgreSQL` migration・SQL レビュー
- `BigQuery / DuckDB` クエリ整理
- `Terraform plan` 確認
- `scikit-learn / LightGBM` 実験コード整理
- `Vertex AI` pipeline・job 設定調査
- 既存コードのリファクタ方針作成
- テスト実行、ログ確認、失敗原因調査

---

## 3. 環境構築

### 前提

- Node.js 18 以上
- Anthropic API キー（または Claude Pro / Claude Max サブスクリプション）

### インストール

```bash
npm install -g @anthropic-ai/claude-code
```

### 認証

```bash
# Anthropic API キーを使う場合
export ANTHROPIC_API_KEY=sk-ant-...

# または claude login でブラウザ認証
claude login
```

### 動作確認

```bash
claude --version
claude "hello"
```

---

## 4. 起動方法

```bash
# プロジェクトディレクトリに移動
cd /path/to/project

# 対話モードで起動
claude

# 1 回だけ実行
claude "テストが失敗している原因を調べてください"

# ファイルを指定して起動
claude --add-dir src/ "src 以下のコードをレビューしてください"
```

---

## 5. CLAUDE.md（プロジェクト指示ファイル）

プロジェクトルートに `CLAUDE.md` を置くと、Claude Code が起動時に自動で読み込みます。

### 役割

- プロジェクト固有のルール・制約・禁止事項を記述する
- 使用技術スタック・ディレクトリ構成・命名規則を伝える
- 実行してよいコマンド・してはいけない操作を明記する

### テンプレート

```markdown
# CLAUDE.md

## プロジェクト概要
- 用途: ...
- 主言語: Python / Rust
- DB: PostgreSQL

## 禁止事項
- `terraform apply` は人間承認必須
- 本番 DB への直接 UPDATE / DELETE 禁止
- `main` ブランチへの直接 push 禁止

## 技術スタック
- backend: Python 3.11 / Rust 1.75
- DB: PostgreSQL 15
- IaC: Terraform 1.6

## ディレクトリ構成
- src/: メインコード
- tests/: テストコード
- infra/: Terraform

## よく使うコマンド
- テスト実行: `pytest tests/`
- Rust ビルド: `cargo build`
- DB migration: `alembic upgrade head`
```

## 5-1. Rules（最初に押さえるべき基本機能）

Claude Code の品質は Rule 設計で大きく変わります。
最初に「どの指示が優先されるか」を固定してください。

### Rule の優先順位

| レイヤー | 例 | 使い方 |
| --- | --- | --- |
| セッション指示 | 対話で渡す依頼文 | その作業だけの一時指示 |
| リポジトリ指示 | `CLAUDE.md` | プロジェクト全体の標準ルール |
| 個人設定 | `~/.claude.json` | 全プロジェクト共通の個人設定 |

### 最小 Rule セット（必須）

- 破壊的操作の禁止: `terraform apply`、本番 DB write、`main` 直 push
- テスト実行の必須化: 変更後は必ずテストを実行
- 変更単位の制約: 1 PR 1 目的、巨大差分を避ける
- セキュリティ制約: 秘密情報をログ出力しない

### Rule 追記例

```markdown
## 品質ルール
- 変更後は `pytest tests/` を実行し、失敗時は原因と対策を記録する
- 既存 API の互換性を壊す変更は事前に明示する

## セキュリティルール
- `.env`、鍵ファイル、トークンを出力しない
- 本番環境に対する write 操作は人間承認必須
```

## 5-2. Skills（再利用できる手順の基本機能）

Skills は「よく使う作業手順」を標準化して再利用する仕組みです。
毎回同じ説明をしなくてよくなるため、精度と速度が安定します。

### Skill 化すべき代表パターン

- Python バグ修正フロー（再現 → 修正 → テスト）
- Rust 型エラー修正フロー（ビルド → clippy → テスト）
- SQL レビュー（実行計画確認、index 提案、安全性確認）
- PR レビュー（破壊的変更、回帰、テスト不足の確認）

### Skill テンプレート（例）

```markdown
# Skill: Python バグ修正

## 入力
- 失敗ログ
- 該当ファイル

## 手順
1. 再現コマンドを実行
2. 原因箇所を最小範囲で特定
3. 修正
4. テスト追加
5. 回帰テスト実行

## 完了条件
- 失敗テストが通る
- 既存テストを壊していない
```

## 5-3. Agents（役割分担の基本機能）

Agent は「どの種類の作業を、どの方針で進めるか」を分担する設計です。
単一の汎用指示より、役割を分けたほうが品質が安定します。

### 最小 Agent 構成（推奨）

| Agent | 役割 | 禁止事項 |
| --- | --- | --- |
| 実装 Agent | コード修正・テスト追加 | 本番環境への直接変更 |
| レビュー Agent | 差分監査・回帰リスク洗い出し | 実装修正の直接実行 |
| ドキュメント Agent | README、変更記録、運用手順更新 | コードの仕様変更 |

### Agent 指示の書き方

- 目的: 何を達成するか
- 入力: 参照するファイルとログ
- 完了条件: テスト結果、差分条件、報告形式
- 禁止事項: 本番操作、機密情報出力、巨大差分

### Agent 指示テンプレート

```markdown
## Agent: レビュー

目的:
- 変更差分のバグ・回帰・セキュリティリスクを特定する

完了条件:
- 重大/中/軽微で指摘を分類
- 再現手順または根拠行を提示
- テスト不足を列挙
```

---

## 6. Memory システム

Claude Code はセッションをまたいで記憶を保持する Memory ファイルシステムを持ちます。

| スコープ | パス | 用途 |
| --- | --- | --- |
| ユーザーメモリ | `~/.claude/memory/` | 全プロジェクト共通の設定・好み |
| セッションメモリ | セッション内のみ | 作業中の一時メモ |
| リポジトリメモリ | `.claude/memory/` | リポジトリ固有の知識 |

---

## 7. スラッシュコマンド

| コマンド | 説明 |
| --- | --- |
| `/help` | コマンド一覧を表示 |
| `/clear` | 会話履歴をリセット |
| `/compact` | 会話を要約してコンテキストを節約 |
| `/model` | 使用モデルを切り替え |
| `/permissions` | 許可設定を確認・変更 |
| `/cost` | このセッションの API コスト確認 |
| `/exit` | 終了 |

---

## 8. MCP ツール連携

Claude Code は MCP（Model Context Protocol）を通じて外部ツールと連携できます。

```bash
# MCP サーバーを追加（例: filesystem）
claude mcp add filesystem npx @modelcontextprotocol/server-filesystem /path/to/dir

# MCP 設定確認
claude mcp list
```

`~/.claude.json` または `.claude/settings.json` に MCP 設定を記述します。

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/home/user/project"]
    }
  }
}
```

---

## 9. 権限・安全設定

### 承認モード

| モード | 説明 |
| --- | --- |
| `default` | 危険な操作は都度確認する（推奨） |
| `--dangerously-skip-permissions` | 確認なしで全操作を実行（本番禁止） |

### 推奨設定

```markdown
# CLAUDE.md に記載する禁止事項の例
- terraform apply は `plan` のみ実行し、apply は人間が承認する
- 本番 DB (host: prod-db) への write 操作禁止
- `.env` ファイルの内容をログ出力しない
```

---

## 10. 運用のポイント

| ポイント | 内容 |
| --- | --- |
| CLAUDE.md を必ず用意する | プロジェクトのルール・禁止事項を事前に記述する |
| 作業単位を明確にする | 「このファイルを修正して」より「このテストを通してください」が安全 |
| Cloud / DB 操作は権限制限 | 本番環境への write 権限は渡さない |
| 実行前に plan を確認 | Terraform / DB migration は実行前に差分を確認する |
| コンテキストを節約する | `/compact` で要約し、長期セッションでのコスト肥大を防ぐ |

---

## 11. 他ツールとの使い分け

| 業務 | Claude Code | 他ツール |
| --- | --- | --- |
| 技術設計・既存コード調査 | **第一候補** | Codex（文書化補助） |
| IDE 内の素早い実装 | 補助 | **Cursor が得意** |
| PR レビュー | 深掘りレビュー | **GitHub Copilot が得意** |
| リーダー業務・ドキュメント | 補助 | **Codex が得意** |
| チケット委譲 | 補助 | **Devin が得意** |

