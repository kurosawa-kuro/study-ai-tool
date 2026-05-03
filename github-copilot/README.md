# GitHub Copilot

作成日: `2026-05-03`

## 1. 概要

GitHub Copilot は GitHub（Microsoft）が提供する AI コーディング支援サービスです。
IDE 補完・PR レビュー・チャット・CLI など GitHub エコシステムと深く統合されています。

このスタックでは **GitHub 運用の補助・PRレビュー・組織標準ツール** として位置づけます。

---

## 2. 主な特徴

| 特徴 | 内容 |
| --- | --- |
| IDE 補完 | VSCode / JetBrains などで次のコードを補完する |
| PR レビュー | GitHub 上の PR に対して AI がレビューコメントを付ける |
| PR 要約 | PR の変更内容を自動で要約する |
| Copilot Chat | IDE 内でコードについてチャットで質問できる |
| Copilot CLI | ターミナルでシェルコマンドを自然言語で生成できる |
| Agent モード | 複数ファイルにまたがる実装・修正を自律的に行う |
| Workspace | リポジトリ全体を参照してチャットできる |

### 得意な業務

- GitHub PR のレビュー・要約
- 小さなコード補完
- GitHub 中心のチーム標準化・組織ポリシー管理
- IDE 内でのコード質問・説明
- シェルコマンドの生成（CLI）
- コミットメッセージの生成

---

## 3. 環境構築

### プラン

| プラン | 対象 | 主な機能 |
| --- | --- | --- |
| Copilot Free | 個人（無料枠あり） | 補完・チャット（月間制限あり） |
| Copilot Pro | 個人（$10/月） | 補完・チャット・PR レビュー・CLI |
| Copilot Business | 組織（$19/ユーザー/月） | 上記＋組織管理・ポリシー設定 |
| Copilot Enterprise | 大規模組織 | 上記＋ナレッジベース・Fine-tuning |

### VSCode 拡張インストール

```bash
# VSCode の拡張マーケットプレースから
# 拡張 ID: GitHub.copilot
code --install-extension GitHub.copilot
code --install-extension GitHub.copilot-chat
```

または VSCode 内で `Ctrl+Shift+X` → `GitHub Copilot` を検索してインストール。

### 認証

1. VSCode 右下の Copilot アイコンをクリック
2. `Sign in to GitHub` を選択
3. ブラウザで GitHub 認証を完了

---

## 4. GitHub Copilot CLI のインストール

Copilot CLI はターミナルで自然言語からシェルコマンドを生成できるツールです。

### 前提

- Node.js 18 以上
- `gh`（GitHub CLI）がインストールされていること

### gh のインストール

```bash
# Ubuntu / Debian
sudo apt update
sudo apt install gh

# macOS (Homebrew)
brew install gh

# バージョン確認
gh --version
```

### gh 認証

```bash
gh auth login
# → GitHub.com を選択
# → HTTPS を選択
# → ブラウザ認証または PAT を選択
```

### Copilot CLI 拡張のインストール

```bash
gh extension install github/gh-copilot
```

### 動作確認

```bash
gh copilot --version
gh copilot suggest "カレントディレクトリ以下の .log ファイルを全部削除したい"
```

---

## 5. Copilot CLI の使い方

### 基本コマンド

| コマンド | 説明 |
| --- | --- |
| `gh copilot suggest` | シェルコマンドを提案する |
| `gh copilot explain` | コマンドの説明をする |

### suggest の使い方

```bash
# シェルコマンドを提案
gh copilot suggest "30日以上前に変更されたファイルを探したい"

# git コマンドを提案（-t git を指定）
gh copilot suggest -t git "過去1週間のコミット履歴を一行で見たい"

# gh コマンドを提案（-t gh を指定）
gh copilot suggest -t gh "自分がアサインされた open の PR 一覧を見たい"
```

### explain の使い方

```bash
# コマンドの説明
gh copilot explain "find . -name '*.log' -mtime +30 -delete"
```

### エイリアス設定（推奨）

```bash
# .bashrc または .zshrc に追加
eval "$(gh copilot alias -- bash)"   # bash の場合
eval "$(gh copilot alias -- zsh)"    # zsh の場合

# エイリアス設定後は短縮形で使える
ghcs "git でブランチを切りたい"       # suggest
ghce "ls -la"                         # explain
```

---

## 6. copilot-instructions.md（カスタム指示）

`.github/copilot-instructions.md` に置くことで、リポジトリ全体の Copilot の動作をカスタマイズできます。

### 役割

- プロジェクト固有のコーディング規約を AI に伝える
- 使用技術・禁止事項・命名規則を記述する
- チームの標準的な実装パターンを伝える

### テンプレート（`.github/copilot-instructions.md`）

```markdown
# Copilot Instructions

## 技術スタック
- backend: Python 3.11 / Rust 1.75
- DB: PostgreSQL 15
- frontend: React + pico.css（Next.js 禁止）
- IaC: Terraform 1.6

## コーディング規約
- Python: black でフォーマット、ruff でリント、mypy で型チェック
- Rust: rustfmt でフォーマット、clippy でリント
- TypeScript: strict モード必須
- テストは必ず追加すること

## 禁止事項
- Next.js は使用禁止。React + pico.css を使うこと
- any 型の使用禁止（TypeScript / Python）
- console.log を本番コードに残さないこと
- terraform apply は人間承認必須

## 命名規則
- Python: snake_case
- Rust: snake_case（関数・変数）、PascalCase（型・構造体）
- React コンポーネント: PascalCase
- CSS クラス: kebab-case

## PR・コミット規則
- コミットメッセージ: feat / fix / docs / test / refactor / chore
- PR は小さく、1機能・1修正単位で作成する
```

---

## 7. Prompt Files（`.github/prompts/`）

再利用可能なプロンプトを `.github/prompts/` に置いて共有できます（VSCode のみ）。

```markdown
<!-- .github/prompts/pr-review.prompt.md -->
---
mode: ask
---

このPRをレビューしてください。以下の観点で確認してください:
1. セキュリティの問題がないか（SQL インジェクション、XSS など）
2. エラーハンドリングが適切か
3. テストが十分か
4. コーディング規約に準拠しているか
5. パフォーマンス上の問題がないか
```

---

## 8. Skills（VSCode Copilot Chat 拡張）

Copilot Chat に独自のスキルを追加できます（`.github/` 配下の設定ファイルで管理）。

### 組み込みスラッシュコマンド

| コマンド | 説明 |
| --- | --- |
| `/explain` | 選択したコードの説明 |
| `/fix` | バグの修正提案 |
| `/tests` | テストコードの生成 |
| `/doc` | ドキュメントの生成 |
| `/optimize` | パフォーマンス改善の提案 |

### `@workspace` コンテキスト

```
# リポジトリ全体を参照してチャット
@workspace このプロジェクトの認証処理はどこにありますか？

# 特定ファイルを参照
#file:src/auth.py この関数の挙動を説明してください
```

---

## 9. Agent モード（Copilot Edits）

VSCode の Copilot Edits（`Ctrl+Shift+I`）で複数ファイルにまたがる修正を行えます。

```
# 使用例
Ctrl+Shift+I を押して Copilot Edits を開く

↓

「ユーザー認証機能にリフレッシュトークンを追加してください。
影響するファイルはすべて修正してください。」

↓

変更差分を確認してから Accept / Discard を選択
```

---

## 10. PR レビューの設定

GitHub リポジトリの設定で Copilot の PR レビューを有効化します。

1. リポジトリの `Settings > Code and automation > Copilot`
2. `Copilot pull request summaries` を有効化
3. `Copilot code review` を有効化

### PR レビューのリクエスト

PR 作成後、`Reviewers` に `Copilot` を追加するとレビューが自動実行されます。

---

## 11. 組織管理（Business / Enterprise）

| 設定 | 内容 |
| --- | --- |
| ポリシー管理 | 使用できるモデル・機能を組織で制限できる |
| データ除外 | 特定ファイル・リポジトリを AI に送らないよう設定できる |
| 監査ログ | 利用状況のログを確認できる |
| ナレッジベース | 組織固有のドキュメントを Copilot に学習させられる（Enterprise） |

---

## 12. 運用のポイント

| ポイント | 内容 |
| --- | --- |
| copilot-instructions.md を必ず用意する | プロジェクトルール・禁止事項を記述する |
| 深い設計判断は別ツールと併用 | Terraform、ML/MLOps の広い文脈は `Claude Code` を使う |
| 個人・組織プランのデータポリシーを確認する | 契約条件によってデータの扱いが異なる |
| PR レビューは確認してから Merge する | AI のレビューをそのまま採用しない |
| CLI は alias を設定する | `ghcs` / `ghce` で素早く呼び出せるようにする |

---

## 13. 他ツールとの使い分け

| 業務 | GitHub Copilot | 他ツール |
| --- | --- | --- |
| GitHub PR レビュー・要約 | **第一候補** | Claude Code（深掘り） |
| IDE 補完 | **得意** | Cursor（AI チャット込み） |
| シェルコマンド生成 | **CLI が得意** | Claude Code（複雑な操作） |
| 技術設計・コード調査 | 補助 | **Claude Code が得意** |
| リーダー業務 | 補助 | **Codex が得意** |
| チケット委譲 | 補助 | **Devin が得意** |
