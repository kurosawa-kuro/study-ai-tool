# AI開発支援ツール比較：テックリード兼プログラマー向け

作成日: `2026-05-03`  
対象ツール: `Claude Code` / `Codex` / `Cursor` / `GitHub Copilot` / `Devin` / `DeepSeek`

---

## 0. このドキュメントの結論

質問者は、**リーダー業務とプログラミング業務の両方**を担当する。

そのため、AIツールは単なる「コーディング性能」ではなく、以下を横断して評価する。

- ヒアリング
- 設計
- スケジュール
- 見積もり
- レビュー
- ドキュメント
- 報告
- 詳細設計
- コーディング
- テストコード
- 動作検証

最終的な主力構成は次の通り。

| 用途 | 推奨ツール | 位置づけ |
| --- | --- | --- |
| 技術作業の主力 | `Claude Code` | 技術設計、既存コード調査、CLI実行、テスト、DB、Cloud、ML/MLOps |
| リーダー業務・文書化 | `Codex` | ヒアリング整理、設計分解、見積もり、報告、ドキュメント、実装委譲 |
| IDE内実装速度 | `Cursor` | React + pico.css、Python/Rustの細かい実装、局所修正 |
| GitHub PR運用 | `GitHub Copilot` | PRレビュー、PR要約、補完、組織標準ツール |
| チケット委譲 | `Devin` | 明確な完了条件があるタスク、移行、検証込み作業 |
| API / 社内AI基盤 | `DeepSeek` | 社内AI補助、SQL補助、RAG補助、独自エージェント基盤 |

実務上の結論は次。

```text
第一候補: Claude Code + Codex + Cursor
追加候補: GitHub Copilot
委譲候補: Devin
基盤候補: DeepSeek
```

---

## 1. 対象者と前提

このドキュメントは、一般的なAIコーディングツール比較ではない。

対象者は、**テックリード兼プログラマー**として、設計・調整・報告を行いながら、自分でも実装・テスト・検証を行う人である。

---

### 1.1 リーダー業務

| 業務 | 内容 | AIに期待する支援 |
| --- | --- | --- |
| ヒアリング | 要望、課題、制約、背景の確認 | 質問リスト、議事録、論点整理、未決事項整理 |
| 設計 | 基本設計、方式検討、技術選定 | 設計案比較、リスク整理、制約整理 |
| スケジュール | WBS、マイルストーン、依存関係整理 | タスク分解、順序整理、バッファ検討 |
| 見積もり | 体制、期間、リスク、調整工数の見積もり | 作業分解、リスク洗い出し、見積もり根拠作成 |
| レビュー | 設計レビュー、計画レビュー、成果物レビュー | 観点整理、チェックリスト、指摘案作成 |
| ドキュメント | 設計書、説明資料、運用メモ | 構成案、文章化、要約、比較表作成 |
| 報告 | 進捗、課題、リスク、完了報告 | 上長向け、顧客向け、開発者向けの文面作成 |

---

### 1.2 プログラミング業務

| 業務 | 内容 | AIに期待する支援 |
| --- | --- | --- |
| 詳細設計 | 実装単位の設計、DB設計、API設計 | 影響範囲調査、設計案比較、エッジケース整理 |
| 見積もり | 実装量、テスト量、調査量の見積もり | 修正ファイル、実装手順、テスト範囲の洗い出し |
| コーディング | Python/Rust/Reactなどの実装 | 実装、修正、リファクタ、補完 |
| 実装コード | 本体コード、設定、IaC、SQL | 複数ファイル修正、型エラー修正、SQL改善 |
| テストコード | unit test、integration test、回帰テスト | 不足テスト洗い出し、テスト追加、失敗原因調査 |
| 動作検証 | CLI、Web UI、DB、Cloud、ML実験の確認 | コマンド実行、ログ確認、再現手順、検証記録 |
| レビュー | コードレビュー、PRレビュー | 差分確認、指摘、改善案、セキュリティ観点 |
| ドキュメント | README、設計メモ、運用手順 | 変更内容、使い方、制約、運用手順の文章化 |
| 報告 | 実装結果、テスト結果、残課題の報告 | 変更概要、影響範囲、テスト結果、次アクション整理 |

---

### 1.3 技術スタック

| 領域 | 技術 |
| --- | --- |
| batch | `Python` / `Rust` |
| backend | `Python` / `Rust` |
| DB | `PostgreSQL` |
| Analytics / DWH | `BigQuery` / `DuckDB` |
| Search / Indexing / Retrieval | `Meilisearch` |
| frontend | `React` + `pico.css` |
| frontend rule | `Next.jsは禁止` |
| Cloud / IaC | `AWS` / `GCP` / `Terraform` |
| ML | `scikit-learn` / `LightGBM` |
| MLOps | `Vertex AI` |

補足:

- `Meilisearch` はDWHではない。
- `Meilisearch` は検索、索引、検索UI、RAG補助などの **Search / Indexing / Retrieval** 枠で扱う。
- `Next.js` は重いため、この前提では禁止する。
- frontendは `React + pico.css` を基本とする。

---

## 2. AIツール全体の位置づけ

| 分類 | ツール | 役割 |
| --- | --- | --- |
| 主力エージェント | `Claude Code` | 技術作業、CLI実行、既存コード調査、テスト、Cloud/DB/MLops |
| リーダー支援 | `Codex` | ヒアリング整理、設計、見積もり、報告、文書化 |
| IDE実装 | `Cursor` | IDE内の実装速度、React/Python/Rustの局所修正 |
| GitHub運用 | `GitHub Copilot` | PRレビュー、PR要約、補完、組織標準 |
| 委譲エージェント | `Devin` | チケット処理、バックログ処理、検証込み作業 |
| API基盤 | `DeepSeek` | 低コストAPI、社内AI基盤、SQL/RAG補助 |

---

## 3. 総合ランキング

| 総合順位 | ツール | 理由 |
| ---: | --- | --- |
| 1 | `Claude Code` | 技術設計、既存コード理解、Python/Rust、DB、Cloud、ML/MLOps、テスト、検証に強い |
| 2 | `Codex` | リーダー業務、設計整理、見積もり、報告、ドキュメント、実装委譲に強い |
| 3 | `Cursor` | 日々の実装速度、React + pico.css、Python/Rust実装に強い |
| 4 | `GitHub Copilot` | GitHub PRレビュー、補完、組織運用に強い |
| 5 | `Devin` | チケット単位の委譲、バックログ処理、ブラウザ検証込みの作業に強い |
| 6 | `DeepSeek` | 完成ツールではなく、API基盤、低コストモデルとして有用 |

---

### 3.1 順位が変わる条件

| 条件 | 順位変化 |
| --- | --- |
| 純粋なIDE内実装速度だけを見る | `Cursor` の順位が上がる |
| GitHub組織運用だけを見る | `GitHub Copilot` の順位が上がる |
| チケットを丸ごとAIに任せる | `Devin` の順位が上がる |
| 社内AI基盤を作る | `DeepSeek` の重要度が上がる |
| リーダー業務込みで見る | `Codex` の順位が上がる |
| CLI、DB、Cloud、MLops中心で見る | `Claude Code` の順位が上がる |

---

## 4. 業務別の使い分け

### 4.1 リーダー業務別おすすめ

| 業務 | 第一候補 | 次点 | コメント |
| --- | --- | --- | --- |
| ヒアリング | `Codex` | `Claude Code` | 質問リスト、議事録、論点整理、未決事項整理に向く |
| 要件整理 | `Codex` | `Claude Code` | 要望を機能要件、非機能要件、制約に分解しやすい |
| 設計 | `Claude Code` | `Codex` | コードや技術制約と結びつく設計は `Claude Code` が強い |
| スケジュール | `Codex` | `Devin` | WBS、マイルストーン、依存関係、リスク整理に向く |
| 計画見積もり | `Codex` | `Claude Code` | タスク分解と不確実性の整理に向く |
| 設計レビュー | `Claude Code` | `Codex` | 技術妥当性、影響範囲、リスク確認に向く |
| ドキュメント | `Codex` | `Claude Code` | 設計書、README、報告資料、比較表の作成に向く |
| 報告 | `Codex` | `Claude Code` | 上長向け、顧客向け、開発者向けに文体を変えやすい |

---

### 4.2 プログラミング業務別おすすめ

| 業務 | 第一候補 | 次点 | コメント |
| --- | --- | --- | --- |
| 詳細設計 | `Claude Code` | `Codex` | 既存コード、DB、Cloud、ML/MLOpsを踏まえた設計に向く |
| 実装見積もり | `Claude Code` | `Codex` | 影響範囲、修正ファイル、テスト量を見積もりやすい |
| コーディング | `Cursor` | `Claude Code` | IDE内の速度なら `Cursor`、CLI込みなら `Claude Code` |
| 実装コード | `Cursor` | `Claude Code` | React、Python、Rustの実装速度を重視するなら `Cursor` |
| テストコード | `Claude Code` | `Codex` | 既存コードを読んで不足テストを追加しやすい |
| 動作検証 | `Claude Code` | `Devin` | CLI、batch、DBは `Claude Code`、ブラウザ検証込みなら `Devin` |
| コードレビュー | `GitHub Copilot` | `Claude Code` | GitHub PR運用なら `Copilot`、深掘りなら `Claude Code` |
| 技術ドキュメント | `Claude Code` | `Codex` | コードを踏まえたREADME、設計メモ、運用手順に向く |
| 実装報告 | `Codex` | `Claude Code` | 変更内容、影響範囲、テスト結果の説明に向く |

---

### 4.3 見積もりの分離

見積もりは2種類に分ける。

| 種類 | 内容 | 第一候補 | 次点 |
| --- | --- | --- | --- |
| 計画見積もり | スケジュール、体制、リスク、バッファ、関係者調整 | `Codex` | `Claude Code` |
| 実装見積もり | 修正ファイル、実装量、テスト量、調査量、影響範囲 | `Claude Code` | `Codex` |

AIの見積もりは、タスク分解、影響範囲調査、リスク洗い出しの補助として使う。  
最終的な工数、スケジュール、バッファ、優先度は人間が判断する。

---

### 4.4 レビューの分離

レビューも2種類に分ける。

| 種類 | 内容 | 第一候補 | 次点 |
| --- | --- | --- | --- |
| 設計・計画レビュー | 要件、設計方針、依存関係、スケジュール、リスク | `Claude Code` | `Codex` |
| コードレビュー | 実装、テスト、SQL、Terraform、PR差分、保守性 | `GitHub Copilot` | `Claude Code` |

---

## 5. 技術スタック別の使い分け

| 技術 / 領域 | 第一候補 | 次点 | コメント |
| --- | --- | --- | --- |
| batch Python | `Claude Code` | `Codex` | CLIで実行、ログ確認、テスト修正まで回しやすい |
| batch Rust | `Claude Code` | `Cursor` | `cargo test`、型エラー、複数ファイル修正との相性が良い |
| backend Python / Rust | `Claude Code` | `Cursor` | 設計、修正、テストは `Claude Code`、実装速度は `Cursor` |
| PostgreSQL | `Claude Code` | `Codex` | SQL、migration、ORM、テストデータ作成に向く |
| BigQuery | `Claude Code` | `Codex` | SQLレビュー、クエリ分割、コスト意識のある修正に向く |
| DuckDB | `Claude Code` | `Cursor` | ローカル分析、batch処理、Python連携に向く |
| Meilisearch | `Cursor` | `Claude Code` | API連携やReact検索UI実装は `Cursor`、設計や同期処理は `Claude Code` |
| React + pico.css | `Cursor` | `Codex` | `Next.js禁止` ルールを明示して使う |
| AWS / GCP / Terraform | `Claude Code` | `GitHub Copilot` | 実装と検証は `Claude Code`、PRレビューは `Copilot` |
| scikit-learn / LightGBM | `Claude Code` | `Codex` | 実験コード、特徴量処理、評価、リファクタに向く |
| Vertex AI | `Claude Code` | `Codex` | pipeline、training job、endpoint、IAM整理に向く |
| PRレビュー | `GitHub Copilot` | `Claude Code` | GitHub中心なら `Copilot`、深掘りレビューは `Claude Code` |
| チケット委譲 | `Devin` | `Codex` | `Devin` は明確な完了条件があるタスク向け |

---

## 6. ツール別詳細

### 6.1 Claude Code

#### 位置づけ

このスタックでは **技術作業の第一候補**。

#### 向いていること

- `Python / Rust` のbatch処理
- backendの複数ファイル修正
- `PostgreSQL` migrationやSQLレビュー
- `BigQuery / DuckDB` クエリ整理
- `Terraform plan` の確認
- `scikit-learn / LightGBM` の実験コード整理
- `Vertex AI` pipelineやjob設定の調査
- 既存コードのリファクタ方針作成
- テスト実行、ログ確認、失敗原因調査

#### 向いていないこと

- 純粋なIDE補完速度だけを最優先する作業
- ブラウザ操作込みの長いE2E検証を丸投げする作業
- 仕様が曖昧なままのチケット完全委譲

#### 注意点

- CloudやDBに対する破壊的操作は必ず権限制限する
- `terraform apply`、本番DB更新、本番 `GCP / AWS` 操作は人間承認必須
- 実行権限を渡すほど便利になるが、同時に事故リスクも上がる

---

### 6.2 Codex

#### 位置づけ

このスタックでは **リーダー業務と実装委譲の第一候補**。

#### 向いていること

- ヒアリング項目の作成
- 議事録、決定事項、未決事項の整理
- 基本設計のたたき台
- 複数案比較
- WBS作成
- 見積もり補助
- 報告文書作成
- README、運用手順、設計メモ作成
- 実装タスクの分解と委譲
- コードレビューや変更案作成

#### 向いていないこと

- 常時IDE補完
- 本番環境への直接操作
- 曖昧な見積もりをそのまま確定値として扱うこと

#### 注意点

- 見積もりはAIの出力をそのまま採用しない
- CloudやDBの実操作は権限制限する
- 大きな変更はPR単位、タスク単位に分割する

---

### 6.3 Cursor

#### 位置づけ

このスタックでは **IDE内実装速度の第一候補**。

#### 向いていること

- `React + pico.css` の軽量UI実装
- `Next.js禁止` ルールを守ったフロントエンド修正
- `Python / Rust` の局所修正
- `Meilisearch` 連携UI
- 小さめのAPI追加
- SQLや設定ファイルの軽い修正
- 補完、複数ファイル編集、短い実装ループ

#### 向いていないこと

- 大きめの設計判断
- Cloud/IaCの安全確認
- DB破壊リスクを含むレビュー
- ML/MLOps全体設計

#### 注意点

- 大きめの設計判断や `Cloud / IaC` の安全確認は `Claude Code` や人間レビューと併用する
- Cursor Rulesで `Next.js禁止` を明記する
- 実装が速い分、設計・テスト・レビューを省略しない

---

### 6.4 GitHub Copilot

#### 位置づけ

このスタックでは **GitHub運用の補助**。

#### 向いていること

- PRレビュー
- PR要約
- 小さな補完
- GitHub中心のチーム標準化
- 組織ポリシー管理
- GitHub上でのレビュー運用

#### 向いていないこと

- 深い設計判断
- Terraform安全確認
- ML/MLOpsの広い文脈理解
- GitHub外の実行検証

#### 注意点

- 深い設計、Terraform安全確認、ML / MLOps の広い文脈理解では `Claude Code` や `Codex` を併用する
- 個人プランと組織プランでデータ管理やポリシーが異なるため、組織導入時は契約条件を確認する

---

### 6.5 Devin

#### 位置づけ

このスタックでは **チケット単位で任せる用途**。

#### 向いていること

- 小さな機能追加
- バグ再現と修正
- テスト追加
- 既存コードの棚卸し
- 移行作業
- ブラウザ検証込みのタスク
- 明確な完了条件があるバックログ処理

#### 向いていないこと

- 日常の細かい手元実装
- 本番Cloud/DB/IaC操作
- 曖昧な完了条件のタスク

#### 注意点

- `AWS / GCP / Terraform / DB / Vertex AI` の本番権限は渡さない
- 完了条件、禁止事項、テスト方法を明記する
- 日常の細かい手元実装では `Cursor` や `Claude Code` の方が軽い

---

### 6.6 DeepSeek

#### 位置づけ

このスタックでは **完成型AIコーディングツールではなく、低コストなモデル / API基盤**。

#### 向いていること

- 社内CLIエージェント
- SQL補助ツール
- `Meilisearch / RAG` 向け補助
- batchコード生成補助
- コスト重視のAI機能組み込み
- 独自UIや社内ツールへの組み込み

#### 向いていないこと

- IDE体験そのもの
- PRレビュー運用
- GitHub連携
- 実行ループ
- Cloud操作

#### 注意点

- セキュリティ、監査、ログ、権限制御を自前で設計する必要がある
- 完成ツール料金ではなく、モデルAPI単価として評価する

---

## 7. 実務ワークフロー

### 7.1 リーダー業務フロー

```text
ヒアリング準備
  ↓
ヒアリング実施
  ↓
議事録・論点整理
  ↓
要件整理
  ↓
設計案作成
  ↓
見積もり・スケジュール
  ↓
レビュー
  ↓
報告
```

| フェーズ | 使うツール | 具体的な使い方 |
| --- | --- | --- |
| ヒアリング準備 | `Codex` | 質問リスト、確認事項、想定リスクを作る |
| ヒアリング後 | `Codex` | 議事録、決定事項、未決事項、TODOに整理する |
| 要件整理 | `Codex` | 機能要件、非機能要件、制約、前提に分ける |
| 技術確認 | `Claude Code` | 既存コード、DB、Cloud、ML/MLOps構成を確認する |
| 設計 | `Claude Code` + `Codex` | 技術的妥当性を確認し、説明文書に落とす |
| 見積もり | `Codex` + `Claude Code` | WBS化し、実装影響範囲を確認する |
| レビュー | `Claude Code` + `GitHub Copilot` | 設計レビューとPRレビューを分担する |
| 報告 | `Codex` | 上長向け、顧客向け、開発者向けに文体を変える |

---

### 7.2 プログラミング業務フロー

```text
詳細設計
  ↓
実装見積もり
  ↓
コーディング
  ↓
テストコード追加
  ↓
動作検証
  ↓
レビュー
  ↓
ドキュメント・報告
```

| フェーズ | 使うツール | 具体的な使い方 |
| --- | --- | --- |
| 詳細設計 | `Claude Code` | 既存コード、DB、Cloud、ML/MLOps構成を確認する |
| 実装見積もり | `Claude Code` + `Codex` | 修正ファイル、影響範囲、テスト範囲を洗い出す |
| コーディング | `Cursor` + `Claude Code` | IDE実装は `Cursor`、CLI検証は `Claude Code` |
| テストコード | `Claude Code` | 不足テストを洗い出し、失敗を修正する |
| 動作検証 | `Claude Code` / `Devin` | CLI中心なら `Claude Code`、ブラウザ込みなら `Devin` |
| レビュー | `GitHub Copilot` + `Claude Code` | PRレビューと深掘りレビューを分担する |
| ドキュメント | `Codex` + `Claude Code` | 報告文は `Codex`、技術正確性は `Claude Code` |

---

### 7.3 PRレビュー・報告フロー

```text
PR作成
  ↓
AI要約
  ↓
AIレビュー
  ↓
人間レビュー
  ↓
修正
  ↓
テスト再実行
  ↓
報告
```

| フェーズ | 使うツール | 具体的な使い方 |
| --- | --- | --- |
| PR要約 | `GitHub Copilot` | 差分要約、レビュー観点整理 |
| 深掘りレビュー | `Claude Code` | SQL、Terraform、ML/MLOps、DB変更の確認 |
| 報告文作成 | `Codex` | 変更内容、影響範囲、テスト結果、残リスクの整理 |

---

### 7.4 チケット委譲フロー

```text
チケット定義
  ↓
完了条件明記
  ↓
禁止操作明記
  ↓
Devinへ委譲
  ↓
PR確認
  ↓
Claude Codeで深掘りレビュー
  ↓
人間承認
```

| フェーズ | 使うツール | 具体的な使い方 |
| --- | --- | --- |
| チケット分解 | `Codex` | 背景、目的、完了条件、リスクを整理 |
| 作業委譲 | `Devin` | 明確なタスクを渡す |
| 技術レビュー | `Claude Code` | 差分、テスト、影響範囲を確認 |
| PR運用 | `GitHub Copilot` | PR要約、レビュー支援 |

---

## 8. AI設定ファイル対応表

### 8.1 結論

このリポジトリでは、次のように整理するのが最も安全で運用しやすい。

| 用途 | 推奨する主ファイル / 機能 | 理由 |
| --- | --- | --- |
| プロジェクト全体の常時ルール | `AGENTS.md` + `CLAUDE.md` | Codex / Devin / GitHub Copilot が `AGENTS.md` を扱いやすく、Claude Code は `CLAUDE.md` が標準 |
| Claude Code 用の常時ルール | `CLAUDE.md` | Claude Code の中心。ビルド、テスト、禁止事項、アーキテクチャ概要を書く |
| Cursor 用の常時・条件付きルール | `.cursor/rules/*.mdc` | Cursor では Rules が主力。`Next.js禁止`、React + pico.css、Python/Rust などを明示する |
| GitHub Copilot 用のリポジトリ指示 | `.github/copilot-instructions.md` | Copilot Chat、Copilot code review、Copilot cloud agent に効かせやすい |
| GitHub Copilot 用のパス別指示 | `.github/instructions/*.instructions.md` | Python、Rust、Terraform、React、SQL などファイル種別ごとの指示に向く |
| Devin 用のリポジトリ指示 | `AGENTS.md` | Devin は `AGENTS.md` を coding 前に読む |
| Devin 用の組織・横断知識 | `Knowledge` | 組織共通のコーディング規約、デプロイ手順、PR命名規則などに向く |
| 繰り返しワークフロー | `.agents/skills/<skill-name>/SKILL.md` | Codex / Devin で明示的に使いやすい |
| PRレビュー規約 | `.github/copilot-instructions.md` + `REVIEW.md` | Copilot は custom instructions、Devin Review は `AGENTS.md` と `REVIEW.md` を読む |

---

### 8.2 Claude Code 概念別の対応表

| Claude Code の概念 | 役割 | Codex 相当 | Cursor 相当 | GitHub Copilot 相当 | Devin 相当 |
| --- | --- | --- | --- | --- | --- |
| `CLAUDE.md` | 常時読み込ませるプロジェクト規約、build/test、禁止事項、設計方針 | `AGENTS.md` / `AGENTS.override.md` | `AGENTS.md` または `.cursor/rules/*.mdc` | `.github/copilot-instructions.md`、`AGENTS.md`、`/CLAUDE.md` | `AGENTS.md`、Knowledge |
| `.claude/rules/` | 常時またはファイルパス別の細かいルール | ディレクトリ別 `AGENTS.md`、linters / hooks | `.cursor/rules/*.mdc` | `.github/instructions/*.instructions.md` | `AGENTS.md`、Knowledge、`REVIEW.md` |
| Subagents / custom agents | 専門役割を持つ分離コンテキストのエージェント | Subagents / custom agents | Subagents | Copilot custom agents、Copilot cloud agent、Copilot CLI custom agents | Agent Mode、Agent selector、Managed Devins |
| Skills | 必要時に読み込む再利用可能な手順・知識・ワークフロー | `.agents/skills/<name>/SKILL.md` | Agent Skills | Copilot agent skills | `.agents/skills/<name>/SKILL.md` |
| Slash commands / commands | 手動で起動する定型プロンプト | Slash commands / skills | Commands | Copilot CLI slash commands / prompt files | Custom slash commands / Playbooks |
| Hooks | 編集後・停止時などに決定的なスクリプトを実行 | Hooks | Hooks | Copilot CLI / cloud agent hooks | Skills / Commands / CI で代替 |
| MCP | 外部ツール・DB・GitHub・Linear・Docs への接続 | MCP | MCP | MCP servers | Devin MCP / DeepWiki MCP |
| Review rules | レビュー方針、検出したいバグパターン | Skill または `AGENTS.md` | Rules / Bugbot 対応ルール | `.github/copilot-instructions.md`、code review custom instructions | `REVIEW.md`、Devin Review |

---

### 8.3 ツール別の対応表

| ツール | `CLAUDE.md` 相当 | `Rules` 相当 | `Agents` 相当 | `Skills` 相当 | Commands 相当 |
| --- | --- | --- | --- | --- | --- |
| Claude Code | `CLAUDE.md` | `.claude/rules/` | Subagents | `.claude/skills/<name>/SKILL.md` | Slash commands / skills |
| Codex | `AGENTS.md`、`AGENTS.override.md` | ディレクトリ別 `AGENTS.md`、hooks / linters | Subagents / custom agents | `.agents/skills/<name>/SKILL.md` | Slash commands |
| Cursor | `AGENTS.md`、`.cursor/rules` | `.cursor/rules/*.mdc` | Subagents / Cloud Agent | Agent Skills | `.cursor/commands` |
| GitHub Copilot | `.github/copilot-instructions.md`、`AGENTS.md`、`/CLAUDE.md` | `.github/instructions/*.instructions.md` | Copilot cloud agent、custom agents、CLI custom agents | Copilot agent skills | Copilot CLI commands / prompt files |
| Devin | `AGENTS.md`、Knowledge | Knowledge、`AGENTS.md`、`REVIEW.md` | Agent Mode、Agent selector、Managed Devins | `.agents/skills/<name>/SKILL.md` | Custom slash commands、Playbooks |

---

## 9. 推奨リポジトリ構成

### 9.1 最終推奨構成

```text
repo-root/
  AGENTS.md
  CLAUDE.md
  REVIEW.md

  docs/
    ai-tools/
      README.md
      01-summary.md
      02-persona-and-stack.md
      03-tool-selection.md
      04-workflow.md
      05-config-mapping.md
      06-repo-structure.md
      07-ai-rules.md
      08-safety.md
      09-templates.md
      10-pricing-and-sources.md

  .github/
    copilot-instructions.md
    instructions/
      python.instructions.md
      rust.instructions.md
      react-pico.instructions.md
      terraform.instructions.md
      sql.instructions.md
      mlops-vertex-ai.instructions.md

  .cursor/
    rules/
      project.mdc
      python.mdc
      rust.mdc
      react-pico-no-nextjs.mdc
      terraform.mdc
      sql-bigquery-duckdb.mdc
      mlops-vertex-ai.mdc
    commands/
      code-review.md
      implementation-report.md
      terraform-plan-review.md

  .agents/
    skills/
      test-before-pr/
        SKILL.md
      terraform-plan-review/
        SKILL.md
      bigquery-cost-review/
        SKILL.md
      rust-cargo-check/
        SKILL.md
      python-pytest-review/
        SKILL.md
      vertex-ai-pipeline-review/
        SKILL.md
      meilisearch-sync-review/
        SKILL.md
```

---

### 9.2 最初に作るべきファイル順

1. `AGENTS.md`
2. `CLAUDE.md`
3. `.github/copilot-instructions.md`
4. `.cursor/rules/project.mdc`
5. `.cursor/rules/react-pico-no-nextjs.mdc`
6. `.github/instructions/terraform.instructions.md`
7. `.github/instructions/sql.instructions.md`
8. `REVIEW.md`
9. `.agents/skills/test-before-pr/SKILL.md`
10. `.agents/skills/terraform-plan-review/SKILL.md`

---

### 9.3 使い分けルール

| 書きたい内容 | 書く場所 | 理由 |
| --- | --- | --- |
| 全ツール共通の技術スタック制約 | `AGENTS.md` | ツール横断の共通入口にするため |
| Claude Code に常に守らせること | `CLAUDE.md` | Claude Code が毎セッション読む前提で使えるため |
| Cursor に常に守らせること | `.cursor/rules/*.mdc` | Cursor のIDE内提案に効かせるため |
| Copilot PRレビューやChatに効かせること | `.github/copilot-instructions.md` | GitHub Copilot の標準的なカスタム指示だから |
| Python/Rust/React/Terraform/SQL 別の細則 | `.github/instructions/*.instructions.md`、`.cursor/rules/*.mdc` | パス別・言語別に分けるとノイズが少ないため |
| 長い設計資料や手順書 | Skills / Knowledge | 常時読み込みに入れるとノイズになるため |
| 何度も使う作業手順 | `.agents/skills/*/SKILL.md` | 手順化して再利用しやすいため |
| レビュー専用ルール | `REVIEW.md`、Copilot instructions | 実装ルールとレビュー観点を分離するため |
| チケット委譲の型 | Devin Playbooks / custom slash commands | 毎回同じ依頼テンプレートを使えるため |

---

## 10. 共通AIルール

### 10.1 AGENTS.md

```md
# AGENTS.md

## Project constraints

- Backend and batch code must be Python or Rust.
- PostgreSQL is the primary OLTP database.
- BigQuery and DuckDB are used for analytics.
- Meilisearch is search/indexing, not DWH.
- Frontend must use React + pico.css.
- Do not use Next.js.
- Cloud/IaC is AWS, GCP, and Terraform.
- ML is scikit-learn and LightGBM.
- MLOps is Vertex AI.

## Safety

- Do not run destructive Terraform, cloud, DB, or production commands without explicit approval.
- Prefer `terraform plan` review before any apply-like operation.
- Do not change IAM, service accounts, buckets, datasets, or production resources without approval.

## Testing

- For Rust, run `cargo check` and relevant `cargo test`.
- For Python, run the project pytest command.
- Add or update tests when changing backend, batch, SQL, or ML pipeline code.
```

---

### 10.2 CLAUDE.md

```md
# CLAUDE.md

Follow `AGENTS.md` as the source of truth.

## Claude Code workflow

- First inspect existing code and tests.
- Prefer small diffs.
- Before editing Terraform, SQL, or MLOps files, explain the risk.
- Never run destructive production commands without explicit approval.
- Use CLI validation when possible.

## Priority

1. Preserve existing architecture.
2. Avoid introducing unnecessary frameworks.
3. Do not propose Next.js.
4. Prefer Python/Rust, PostgreSQL, BigQuery, DuckDB, React + pico.css, Terraform, Vertex AI.
```

---

### 10.3 Cursor Rule: React + pico.css / No Next.js

```md
---
description: React + pico.css frontend rule. Never use Next.js.
---

- Use React and pico.css.
- Do not introduce Next.js.
- Do not propose SSR, App Router, or Next-specific routing.
- Prefer plain React components and minimal dependencies.
- Keep components small and readable.
```

---

### 10.4 GitHub Copilot Instructions

```md
# GitHub Copilot instructions

This repository uses Python/Rust for backend and batch processing.
Frontend is React + pico.css. Next.js is prohibited.

Before suggesting code changes:

- Check existing patterns.
- Preserve current architecture.
- Add or update tests for backend, batch, SQL, and ML pipeline changes.
- For Terraform, prefer plan review and never assume apply is allowed.
- For PR review, flag risky changes to IAM, production DB, GCP/AWS resources, and Vertex AI jobs.
```

---

### 10.5 REVIEW.md

```md
# REVIEW.md

## Review guidelines

- Flag untested backend or batch changes.
- Flag SQL changes without migration or rollback consideration.
- Flag BigQuery queries that may scan excessive data.
- Flag Terraform changes touching IAM, service accounts, datasets, buckets, or production resources.
- Flag frontend changes that introduce Next.js.
- Flag ML/MLOps changes without evaluation or reproducibility notes.
- Confirm new code follows Python/Rust conventions used in this repo.
```

---

## 11. 安全ルール

AIエージェントにCloudやDB操作を任せる場合、以下のルールを必須にする。

---

### 11.1 禁止する操作

```text
terraform apply
terraform destroy
本番DBへの直接UPDATE / DELETE / TRUNCATE
IAM権限の追加・削除
本番bucket / dataset / tableの削除
Vertex AI endpoint / model / jobの削除
本番環境のsecret更新
```

---

### 11.2 人間承認が必要な操作

```text
Terraform state変更
migration適用
本番DB schema変更
BigQuery dataset / table変更
GCP service account変更
AWS IAM policy変更
Vertex AI endpoint変更
本番環境へのdeploy
```

---

### 11.3 AIに許可しやすい操作

```text
terraform planのレビュー
SQLの静的レビュー
migration案の作成
ローカルテスト実行
開発環境での検証
README更新
PR説明文の作成
影響範囲調査
```

---

## 12. タスク依頼テンプレート

### 12.1 ヒアリング準備テンプレート

```md
以下の案件について、ヒアリング項目を作成してください。

## 案件概要
- 目的:
- 現状課題:
- 関係者:
- 期限:

## 技術スタック
- backend: Python / Rust
- DB: PostgreSQL
- DWH: BigQuery / DuckDB
- frontend: React + pico.css
- Cloud: AWS / GCP / Terraform
- ML/MLOps: scikit-learn / LightGBM / Vertex AI

## 出力してほしいもの
- 確認すべき質問
- 未確定になりやすい論点
- 技術リスク
- 見積もりに影響する要素
- 次回までのTODO
```

---

### 12.2 設計レビュー依頼テンプレート

```md
以下の設計案をレビューしてください。

## 前提
- Next.jsは禁止
- frontendはReact + pico.css
- backend/batchはPythonまたはRust
- DBはPostgreSQL
- 分析はBigQuery / DuckDB
- MLOpsはVertex AI

## レビュー観点
- 要件を満たしているか
- 過剰設計になっていないか
- 依存関係が増えすぎていないか
- DB / Cloud / Terraformのリスクはないか
- テストしやすい構成か
- 運用しやすいか
- 見積もりに影響する不確実性は何か
```

---

### 12.3 実装依頼テンプレート

```md
以下の実装を行ってください。

## 目的

## 変更対象

## 制約
- Python / Rust を優先
- frontendはReact + pico.css
- Next.jsは禁止
- 破壊的なDB操作は禁止
- terraform applyは禁止

## 完了条件
- 実装が完了している
- テストコードが追加または更新されている
- 既存テストが通る
- 変更内容がREADMEまたは設計メモに反映されている
- 変更内容、影響範囲、テスト結果を報告している
```

---

### 12.4 PRレビュー依頼テンプレート

```md
このPRをレビューしてください。

## レビュー観点
- 要件との整合性
- 実装の保守性
- テスト不足
- SQL / DB migrationの安全性
- Terraform / Cloud / IAMの安全性
- ML / MLOpsへの影響
- React + pico.cssのルール違反
- Next.jsや不要な依存追加がないか
- ドキュメント更新漏れ

## 出力形式
- 重大な指摘
- 修正推奨
- 任意改善
- 良い点
- マージ前に確認すべきこと
```

---

### 12.5 報告文作成テンプレート

```md
以下の作業結果を、報告文に整理してください。

## 対象読者
- 上長向け / 顧客向け / 開発者向け

## 作業内容

## 変更内容

## 影響範囲

## テスト結果

## 残課題

## 次アクション

## 出力形式
- 簡潔版
- 詳細版
- リスクと対策
```

---

### 12.6 Devinチケット委譲テンプレート

```md
以下のチケットを実装してください。

## 背景

## 目的

## 変更対象

## 完了条件

- 実装が完了している
- テストが追加または更新されている
- 既存テストが通る
- PRが作成されている
- 変更内容、影響範囲、テスト結果がPRに記載されている

## 禁止事項

- terraform applyは禁止
- 本番DB更新は禁止
- IAM変更は禁止
- 本番GCP/AWSリソース変更は禁止
- Next.js導入は禁止

## レビュー時に確認したいこと

- 変更の妥当性
- テスト不足
- 破壊的変更の有無
- ドキュメント更新漏れ
```

---

## 13. 導入パターン

### 13.1 最小構成

| 用途 | ツール |
| --- | --- |
| 技術作業 | `Claude Code` |
| リーダー業務・報告 | `Codex` |

特徴:

- 低コストで始めやすい
- 設計、実装、テスト、報告まで広く対応
- IDE補完の快適さはやや不足する

---

### 13.2 推奨構成

| 用途 | ツール |
| --- | --- |
| 技術作業 | `Claude Code` |
| リーダー業務・報告 | `Codex` |
| IDE実装速度 | `Cursor` |

特徴:

- 質問者の業務範囲に最も合う
- リーダー業務と実装業務を両方支援できる
- React + pico.css、Python/Rust実装が速くなる

---

### 13.3 チーム運用構成

| 用途 | ツール |
| --- | --- |
| 技術作業 | `Claude Code` |
| リーダー業務・報告 | `Codex` |
| IDE実装速度 | `Cursor` |
| PRレビュー・組織管理 | `GitHub Copilot` |
| チケット委譲 | `Devin` |

特徴:

- チームでGitHub運用する場合に強い
- バックログ処理や移行作業を委譲しやすい
- ツール数が増えるため、権限管理と費用管理が必要

---

### 13.4 社内AI基盤構成

| 用途 | ツール |
| --- | --- |
| 開発支援 | `Claude Code` / `Codex` / `Cursor` |
| 社内API基盤 | `DeepSeek` |
| 検索・RAG | `Meilisearch` |

特徴:

- 社内CLI、SQL補助、検索補助、RAG補助を自作しやすい
- 低コストにスケールしやすい
- セキュリティ、監査、権限制御を自前で作る必要がある

---

## 14. 価格とプラン

価格は変動しやすいため、導入前に必ず公式ページを確認する。  
この章は補助情報であり、ツール選定の主判断は **業務適合性・安全性・運用しやすさ** を優先する。

| ツール | 価格 / プラン概要 | コメント |
| --- | --- | --- |
| GitHub Copilot | Free、Pro、Pro+、Business、Enterpriseなど | GitHub PRレビュー、Copilot CLI、cloud agent、組織運用を重視する場合に検討 |
| Cursor | Hobby、Pro、Pro+、Ultra、Teams、Enterpriseなど | IDE実装速度を重視する場合に検討 |
| Codex | ChatGPT Plus / Pro / Business / Enterprise 等で利用。Business Codex は利用量確認が重要 | リーダー業務と実装委譲の両方に使いやすい |
| Claude Code | Claude Pro、Max、Team、Enterpriseなど | 技術作業の主力。利用量が多いならMax系も検討 |
| Devin | Free、Pro、Max、Teams、Enterpriseなど | チケット委譲や検証込み作業向け |
| DeepSeek | API従量課金 | 低コストAPI基盤として検討 |

---

## 15. 公式ソース

導入前に必ず最新情報を確認する。

- GitHub Copilot Plans: <https://docs.github.com/en/copilot/get-started/plans>
- GitHub Copilot Pricing: <https://github.com/features/copilot/plans>
- Cursor Pricing: <https://cursor.com/en-US/pricing>
- Claude Code overview: <https://code.claude.com/docs/en/overview>
- Claude plan selection: <https://support.claude.com/en/articles/11049762-choosing-a-claude-plan>
- OpenAI Codex announcement: <https://openai.com/index/introducing-codex/>
- OpenAI Codex with ChatGPT plans: <https://help.openai.com/en/articles/11369540-using-codex-with-your-chatgpt-plan>
- OpenAI Codex rate card: <https://help.openai.com/en/articles/20001106-codex-rate-card>
- OpenAI Business pricing: <https://openai.com/business/chatgpt-pricing/>
- Devin overview: <https://docs.devin.ai/>
- Devin pricing: <https://devin.ai/pricing>
- DeepSeek product site: <https://www.deepseek.com/en/>
- DeepSeek API pricing: <https://api-docs.deepseek.com/quick_start/pricing>
- Meilisearch official site: <https://www.meilisearch.com/>

---

## 16. 変更履歴

| 日付 | 内容 |
| --- | --- |
| 2026-05-03 | 初版作成。AI開発支援ツール比較を、テックリード兼プログラマー向けに再構成 |
| 2026-05-03 | `CLAUDE.md / AGENTS.md / Rules / Skills` 対応表を統合 |
| 2026-05-03 | 構成を「判断 → 前提 → 比較 → 運用 → 設定 → 安全 → テンプレート → 価格」にリファクタリング |

---

## 17. 最終判断

このドキュメントの最重要方針は次。

```text
AGENTS.md を共通ルールの source of truth にする。
CLAUDE.md / Cursor Rules / Copilot Instructions / Devin Skills は、AGENTS.md を参照しつつツール固有の補足に限定する。
```

質問者の業務範囲では、最終的に以下の構成が最も破綻しにくい。

```text
Claude Code: 技術作業の主力
Codex: リーダー業務と文書化
Cursor: IDE実装速度
GitHub Copilot: PRレビューとGitHub運用
Devin: 明確なチケット委譲
DeepSeek: 低コストAPI基盤
```
