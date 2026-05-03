# Cursor

作成日: `2026-05-03`

## 1. 概要

Cursor は AI 機能を統合した VSCode ベースの IDE です。
コード補完・インライン編集・チャット・複数ファイル編集を IDE 内で完結できます。

このスタックでは **IDE 内実装速度の第一候補** として位置づけます。

---

## 2. 主な特徴

| 特徴 | 内容 |
| --- | --- |
| インライン補完 | Tab キーで次のコードを予測・補完する |
| インライン編集 | `Ctrl+K` でカーソル位置のコードを直接書き換える |
| チャット | `Ctrl+L` でサイドパネルのチャットを開く |
| Composer | `Ctrl+I` で複数ファイルにまたがる変更を一括指示する |
| Cursor Rules | プロジェクト固有のルール・禁止事項を AI に伝える |
| Context 指定 | `@file`、`@folder`、`@web` でコンテキストを指定できる |

### 得意な業務

- `React + pico.css` の軽量 UI 実装
- `Next.js 禁止` ルールを守ったフロントエンド修正
- `Python / Rust` の局所修正
- `Meilisearch` 連携 UI
- 小さめの API 追加
- SQL や設定ファイルの軽い修正
- 補完・複数ファイル編集・短い実装ループ

---

## 3. 環境構築

### インストール

1. [cursor.sh](https://cursor.sh) からインストーラーをダウンロード
2. インストール後に起動してサインアップ（GitHub アカウント連携可）

```bash
# Linux の場合 AppImage をダウンロード後
chmod +x cursor-*.AppImage
./cursor-*.AppImage
```

### モデル設定

`Cursor Settings > Models` でモデルを選択します。

| モデル | 特徴 |
| --- | --- |
| claude-3.5-sonnet | バランスが良く日常の実装に向く |
| claude-3.7-sonnet | 複雑な設計・推論が必要な場面に向く |
| gpt-4o | OpenAI モデルを使いたい場合 |

### VSCode 拡張の引き継ぎ

`Ctrl+Shift+P` → `Import VS Code Extensions` で既存の VSCode 拡張を取り込めます。

---

## 4. 起動方法

```bash
# ターミナルから特定フォルダを開く
cursor /path/to/project

# カレントディレクトリを開く
cursor .
```

---

## 5. Cursor Rules（プロジェクト指示ファイル）

`.cursor/rules/` ディレクトリにルールファイルを置くことで、AI の動作をカスタマイズできます。

### ルールファイルの種類

| 種類 | 説明 |
| --- | --- |
| `Always` | 常に適用されるルール |
| `Auto Attached` | 特定のファイルパターンにマッチしたときに適用 |
| `Agent Requested` | AI が必要と判断したときに適用 |
| `Manual` | `@rulename` で明示的に呼び出したときのみ適用 |

### ルールファイル例（`.cursor/rules/project.mdc`）

```markdown
---
description: プロジェクト共通ルール
globs:
alwaysApply: true
---

# プロジェクト共通ルール

## 禁止事項
- Next.js は使用禁止。React + pico.css を使うこと
- `any` 型の使用禁止（TypeScript）
- console.log を本番コードに残さないこと

## 技術スタック
- frontend: React + pico.css（Next.js 禁止）
- backend: Python 3.11 / Rust 1.75
- DB: PostgreSQL 15

## コーディング規則
- Python: black でフォーマット、ruff でリント
- Rust: rustfmt でフォーマット
- テストは必ず追加すること
```

### フロントエンド専用ルール例（`.cursor/rules/frontend.mdc`）

```markdown
---
description: フロントエンドルール
globs: ["**/*.tsx", "**/*.jsx", "src/frontend/**"]
alwaysApply: false
---

# フロントエンドルール

- React + pico.css を使うこと
- Next.js 禁止
- CSS フレームワークは pico.css のみ使用可
- コンポーネントは関数コンポーネントで書くこと
- useState / useEffect の過剰な使用は避けること
```

---

## 6. 主要ショートカット

| ショートカット | 機能 |
| --- | --- |
| `Tab` | インライン補完を受け入れる |
| `Ctrl+K` | インライン編集（選択範囲 or カーソル位置） |
| `Ctrl+L` | チャットパネルを開く |
| `Ctrl+I` | Composer（複数ファイル編集） |
| `Ctrl+Shift+P` | コマンドパレット |
| `@file` | チャットで特定ファイルをコンテキストに追加 |
| `@folder` | チャットでフォルダをコンテキストに追加 |
| `@web` | チャットで Web 検索結果をコンテキストに追加 |

---

## 7. Context の使い方

```
# ファイルを指定
@src/components/Header.tsx このコンポーネントをレビューしてください

# フォルダを指定
@src/frontend/ React コンポーネントの一覧を教えてください

# ドキュメントを参照
@docs pico.css のグリッドシステムの使い方を教えてください
```

---

## 8. .cursorrules（旧形式）

旧形式の `.cursorrules` ファイルもサポートされています。

```text
# .cursorrules

あなたは React + pico.css の専門家です。
Next.js は絶対に使わないでください。
Python コードは black でフォーマットし、型ヒントを必ず付けてください。
テストは pytest で書いてください。
```

---

## 9. 運用のポイント

| ポイント | 内容 |
| --- | --- |
| Cursor Rules を必ず設定する | `Next.js 禁止` などのルールを明記する |
| 設計判断は別ツールと併用 | 大きな設計判断は `Claude Code` や人間レビューと組み合わせる |
| テスト・レビューを省略しない | 実装速度が上がる分、品質担保の手順を維持する |
| Composer は差分確認してから承認 | 複数ファイル変更は必ず内容を確認する |
| `.cursor/rules/` で用途別に管理 | frontend / backend / test など用途別にルールを分けると管理しやすい |

---

## 10. 他ツールとの使い分け

| 業務 | Cursor | 他ツール |
| --- | --- | --- |
| IDE 内の素早い実装 | **第一候補** | Claude Code（CLI 実行込み） |
| 技術設計・既存コード調査 | 補助 | **Claude Code が得意** |
| リーダー業務・ドキュメント | 補助 | **Codex が得意** |
| PR レビュー | 補助 | **GitHub Copilot が得意** |
| チケット委譲 | 補助 | **Devin が得意** |
