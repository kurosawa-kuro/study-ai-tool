# AI開発支援ツール比較：テックリード兼プログラマー向け

作成日: `2026-05-03`  
対象ツール: `Claude Code` / `Codex` / `Cursor` / `GitHub Copilot` / `Devin` / `DeepSeek`

## 各ツールの詳細ドキュメント

| ツール | ドキュメント | 位置づけ |
| --- | --- | --- |
| Claude Code | [claude-code/README.md](claude-code/README.md) | 技術作業の主力エージェント |
| Codex CLI | [codex/README.md](codex/README.md) | リーダー業務・実装委譲 |
| Cursor | [cursor/README.md](cursor/README.md) | IDE 内実装速度 |
| GitHub Copilot | [github-copilot/README.md](github-copilot/README.md) | PR レビュー・組織標準・CLI |
| Devin | [devin/README.md](devin/README.md) | チケット単位の委譲 |
| DeepSeek | [deep-seek/README.md](deep-seek/README.md) | 低コスト API 基盤 |

---

このドキュメントは、一般的なAIコーディングツール比較ではなく、**リーダー業務とプログラミング業務の両方を担当する人** 向けの実務選定メモです。

対象者は、ヒアリング、設計、スケジュール、見積もり、レビュー、ドキュメント、報告を行いながら、Python / Rust / PostgreSQL / BigQuery / DuckDB / React / Terraform / Vertex AI などを使って実装も行う前提です。

---

## 1. 先に結論

### 最終おすすめ構成

| 用途 | 推奨ツール | 位置づけ |
| --- | --- | --- |
| 主力エージェント | `Claude Code` | 技術設計、既存コード調査、CLI実行、テスト、DB、Cloud、ML/MLOpsの主力 |
| リーダー業務支援 | `Codex` | ヒアリング整理、設計分解、見積もり、報告、ドキュメント、実装委譲 |
| IDE実装速度 | `Cursor` | React + pico.css、Python/Rustの細かい実装、局所修正 |
| GitHubレビュー運用 | `GitHub Copilot` | PRレビュー、PR要約、補完、組織標準ツール |
| チケット単位の委譲 | `Devin` | 明確な完了条件があるタスク、移行、検証込み作業 |
| 低コストAPI基盤 | `DeepSeek` | 社内AI補助、SQL補助、RAG補助、独自エージェント基盤 |

### 実務上の結論

質問者は **リーダー兼プログラマー** なので、単に「コードを書くのが速いツール」ではなく、以下の組み合わせが最も実用的です。

```text
Claude Code + Codex + Cursor
```

- `Claude Code`: 技術的に深い作業を担当する主力
- `Codex`: リーダー業務、設計整理、報告、ドキュメント、委譲を担当
- `Cursor`: IDE内の実装速度を上げる担当

必要に応じて、GitHub運用に `GitHub Copilot`、チケット委譲に `Devin`、低コストな社内AI基盤に `DeepSeek` を追加します。

---

## 2. 想定する対象者

このドキュメントの対象者は、以下の2種類の業務を両方行う人です。

### 2.1 リーダー業務

| 業務 | 内容 | AIに期待する支援 |
| --- | --- | --- |
| ヒアリング | 要望、課題、制約、背景の確認 | 質問リスト、議事録、論点整理、未決事項整理 |
| 設計 | 基本設計、方式検討、技術選定 | 設計案比較、リスク整理、制約整理 |
| スケジュール | WBS、マイルストーン、依存関係整理 | タスク分解、順序整理、バッファ検討 |
| 見積もり | 体制、期間、リスク、調整工数の見積もり | 作業分解、リスク洗い出し、見積もり根拠作成 |
| レビュー | 設計レビュー、計画レビュー、成果物レビュー | 観点整理、チェックリスト、指摘案作成 |
| ドキュメント | 設計書、説明資料、運用メモ | 構成案、文章化、要約、比較表作成 |
| 報告 | 進捗、課題、リスク、完了報告 | 上長向け、顧客向け、開発者向けの文面作成 |

### 2.2 プログラミング業務

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

## 3. 質問者の技術スタック

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

### 補足

`Meilisearch` はDWHではなく、検索、索引、検索UI、RAG補助などの **Search / Indexing / Retrieval** 枠で扱います。

---

## 4. 評価軸

### 4.1 リーダー業務の評価軸

| 評価軸 | 重み | 理由 |
| --- | ---: | --- |
| ヒアリング整理 | 15% | 要望、背景、制約を構造化できるか |
| 設計分解 | 20% | 基本設計、方式検討、比較表作成に使えるか |
| スケジュール / WBS | 15% | 作業分解、依存関係、順序整理ができるか |
| 見積もり補助 | 15% | 不確実性、調査量、レビュー待ちを考慮できるか |
| レビュー観点整理 | 15% | 設計、成果物、リスクをレビューできるか |
| ドキュメント / 報告 | 20% | 説明責任を果たす文章を作れるか |

### 4.2 プログラミング業務の評価軸

| 評価軸 | 重み | 理由 |
| --- | ---: | --- |
| CLI実行、テスト、ログ確認 | 25% | batch、backend、Terraform、ML実験で重要 |
| Python / Rust 複数ファイル修正 | 20% | 主言語が `Python / Rust` のため |
| SQL / DB / DWH理解 | 15% | `PostgreSQL`、`BigQuery`、`DuckDB` を多用するため |
| Cloud / Terraform安全性 | 15% | `AWS / GCP / IaC` は破壊的変更のリスクが高いため |
| フロントエンド軽量実装 | 10% | `React + pico.css`、`Next.js禁止` という制約があるため |
| ML / MLOps対応 | 10% | `scikit-learn`、`LightGBM`、`Vertex AI` を使うため |
| PRレビュー、組織運用 | 5% | GitHub運用がある場合に重要 |

---

## 5. 総合順位

| 総合順位 | ツール | 理由 |
| ---: | --- | --- |
| 1 | `Claude Code` | 技術設計、既存コード理解、Python/Rust、DB、Cloud、ML/MLOps、テスト、検証に強い |
| 2 | `Codex` | リーダー業務、設計整理、見積もり、報告、ドキュメント、実装委譲に強い |
| 3 | `Cursor` | 日々の実装速度、React + pico.css、Python/Rust実装に強い |
| 4 | `GitHub Copilot` | GitHub PRレビュー、補完、組織運用に強い |
| 5 | `Devin` | チケット単位の委譲、バックログ処理、ブラウザ検証込みの作業に強い |
| 6 | `DeepSeek` | 完成ツールではなく、API基盤、低コストモデルとして有用 |

### 順位の考え方

- 純粋なIDE内実装速度だけなら `Cursor` の順位は上がる
- GitHub組織運用だけなら `GitHub Copilot` の順位は上がる
- チケットを丸ごと任せる運用なら `Devin` の順位は上がる
- ただし、質問者はリーダー業務と実装業務を両方行うため、総合では `Claude Code + Codex` を中心にするのが自然

---

## 6. リーダー業務別のおすすめ

| 業務 | 第一候補 | 次点 | コメント |
| --- | --- | --- | --- |
| ヒアリング | `Codex` | `Claude Code` | 質問リスト、議事録、論点整理、未決事項整理に向く |
| 要件整理 | `Codex` | `Claude Code` | 要望を機能要件、非機能要件、制約に分解しやすい |
| 設計 | `Claude Code` | `Codex` | コードや技術制約と結びつく設計は `Claude Code` が強い |
| スケジュール | `Codex` | `Devin` | WBS、マイルストーン、依存関係、リスク整理に向く |
| 見積もり | `Codex` | `Claude Code` | タスク分解と不確実性の整理に向く。ただし最終判断は人間 |
| レビュー | `Claude Code` | `GitHub Copilot` | 設計レビューは `Claude Code`、PRレビューは `Copilot` |
| ドキュメント | `Codex` | `Claude Code` | 設計書、README、報告資料、比較表の作成に向く |
| 報告 | `Codex` | `Claude Code` | 上長向け、顧客向け、開発者向けに文体を変えやすい |

---

## 7. プログラミング業務別のおすすめ

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

## 8. 技術スタック別のおすすめ

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

## 9. ツールごとの位置づけ

### 9.1 Claude Code

このスタックでは **技術作業の第一候補** です。

向いていること:

- `Python / Rust` のbatch処理
- backendの複数ファイル修正
- `PostgreSQL` migrationやSQLレビュー
- `BigQuery / DuckDB` クエリ整理
- `Terraform plan` の確認
- `scikit-learn / LightGBM` の実験コード整理
- `Vertex AI` pipelineやjob設定の調査
- 既存コードのリファクタ方針作成
- テスト実行、ログ確認、失敗原因調査

注意点:

- CloudやDBに対する破壊的操作は必ず権限制限する
- `terraform apply`、本番DB更新、本番 `GCP / AWS` 操作は人間承認必須
- 実行権限を渡すほど便利になるが、同時に事故リスクも上がる

### 9.2 Codex

このスタックでは **リーダー業務と実装委譲の第一候補** です。

向いていること:

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

注意点:

- 見積もりはAIの出力をそのまま採用しない
- CloudやDBの実操作は権限制限する
- 大きな変更はPR単位、タスク単位に分割する

### 9.3 Cursor

このスタックでは **IDE内実装速度の第一候補** です。

向いていること:

- `React + pico.css` の軽量UI実装
- `Next.js禁止` ルールを守ったフロントエンド修正
- `Python / Rust` の局所修正
- `Meilisearch` 連携UI
- 小さめのAPI追加
- SQLや設定ファイルの軽い修正
- 補完、複数ファイル編集、短い実装ループ

注意点:

- 大きめの設計判断や `Cloud / IaC` の安全確認は `Claude Code` や人間レビューと併用する
- Cursor Rulesで `Next.js禁止` を明記する
- 実装が速い分、設計・テスト・レビューを省略しない

### 9.4 GitHub Copilot

このスタックでは **GitHub運用の補助** として扱います。

向いていること:

- PRレビュー
- PR要約
- 小さな補完
- GitHub中心のチーム標準化
- 組織ポリシー管理
- GitHub上でのレビュー運用

注意点:

- 深い設計、Terraform安全確認、ML / MLOps の広い文脈理解では `Claude Code` や `Codex` を併用する
- 個人プランと組織プランでデータ管理やポリシーが異なるため、組織導入時は契約条件を確認する

### 9.5 Devin

このスタックでは **チケット単位で任せる用途** に向きます。

向いていること:

- 小さな機能追加
- バグ再現と修正
- テスト追加
- 既存コードの棚卸し
- 移行作業
- ブラウザ検証込みのタスク
- 明確な完了条件があるバックログ処理

注意点:

- `AWS / GCP / Terraform / DB / Vertex AI` の本番権限は渡さない
- 完了条件、禁止事項、テスト方法を明記する
- 日常の細かい手元実装では `Cursor` や `Claude Code` の方が軽い

### 9.6 DeepSeek

このスタックでは **完成型AIコーディングツールではなく、低コストなモデル / API基盤** として評価します。

向いていること:

- 社内CLIエージェント
- SQL補助ツール
- `Meilisearch / RAG` 向け補助
- batchコード生成補助
- コスト重視のAI機能組み込み
- 独自UIや社内ツールへの組み込み

注意点:

- IDE体験、PRレビュー、実行ループ、Cloud操作は自前実装が必要
- セキュリティ、監査、ログ、権限制御も自前で設計する必要がある

---

## 10. 推奨業務フロー

### 10.1 リーダー業務の流れ

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

### 10.2 プログラミング業務の流れ

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

## 11. 見積もり方針

質問者の業務では、見積もりは2種類に分けます。

### 11.1 計画見積もり

リーダー業務としての見積もりです。

| 見積もり対象 | 主な観点 |
| --- | --- |
| スケジュール | 期間、マイルストーン、依存関係、レビュー待ち |
| 体制 | 担当者、役割、スキル、並列化可否 |
| リスク | 要件不明点、技術調査、権限申請、本番制約 |
| バッファ | 不確実性、手戻り、レビュー遅延、障害対応 |

使うツール:

- 第一候補: `Codex`
- 次点: `Claude Code`

### 11.2 実装見積もり

プログラミング業務としての見積もりです。

| 見積もり対象 | 主な観点 |
| --- | --- |
| 実装量 | 修正ファイル、API、SQL、設定、テスト |
| 調査量 | 既存コード調査、仕様確認、ライブラリ確認 |
| テスト量 | unit、integration、回帰、手動確認 |
| 影響範囲 | DB、Cloud、ML pipeline、frontend、batch |

使うツール:

- 第一候補: `Claude Code`
- 次点: `Codex`

### 11.3 見積もりの注意

AIの見積もりは、タスク分解、影響範囲調査、リスク洗い出しの補助として使います。最終的な工数、スケジュール、バッファ、優先度は人間が判断します。

特に以下は人間が確認します。

- 要件の曖昧さ
- 関係者調整の時間
- レビュー待ち時間
- 環境構築や権限申請の時間
- 本番反映の制約
- 障害時の切り戻し
- `Terraform / Cloud / DB / Vertex AI` の安全性

---

## 12. レビュー方針

レビューも2種類に分けます。

### 12.1 設計・計画レビュー

| 観点 | チェック内容 | 主なツール |
| --- | --- | --- |
| 要件 | 背景、目的、制約、非機能要件が明確か | `Codex` |
| 設計 | 方針、代替案、依存関係、影響範囲が整理されているか | `Claude Code` |
| 見積もり | 作業分解、リスク、バッファが妥当か | `Codex` |
| Cloud / DB | 破壊的変更や権限変更がないか | `Claude Code` |
| 報告 | 説明責任を果たせる文書になっているか | `Codex` |

### 12.2 コードレビュー

| 観点 | チェック内容 | 主なツール |
| --- | --- | --- |
| 実装 | 要件を満たしているか、過不足がないか | `Claude Code` |
| テスト | テストが不足していないか、失敗時に検知できるか | `Claude Code` |
| SQL / DB | migration、index、性能、データ破壊リスク | `Claude Code` |
| Cloud / Terraform | plan確認、IAM、resource破壊、環境差分 | `Claude Code` |
| PR運用 | PR要約、差分レビュー、コメント | `GitHub Copilot` |
| 小さな修正 | IDE上の局所指摘 | `Cursor` |

---

## 13. AIツールに守らせるプロジェクトルール

以下は、`CLAUDE.md`、Cursor Rules、Codex用プロジェクト指示、Devin向けタスクテンプレートに展開するルール例です。

```md
## Project AI Rules

### Role
- The user acts as both a technical leader and a programmer.
- Support both leadership tasks and implementation tasks.
- Always separate planning estimates from implementation estimates.

### Language
- Backend and batch code should be written in Python or Rust.
- Do not introduce Go, Java, Ruby, PHP, or Node backend unless explicitly requested.

### Frontend
- Use React and pico.css.
- Do not use Next.js.
- Do not propose SSR, App Router, or Next.js-specific architecture.
- Prefer small components, plain React, and minimal dependencies.

### Database
- PostgreSQL is the primary OLTP database.
- BigQuery and DuckDB are used for analytics.
- Meilisearch is used for search/indexing/retrieval, not as a DWH.

### Cloud / IaC
- Use AWS, GCP, and Terraform.
- Never run destructive Terraform or cloud commands without explicit confirmation.
- Prefer `terraform plan` review before any apply-like operation.
- Do not change IAM, service accounts, buckets, datasets, or production resources without approval.

### ML
- Use scikit-learn and LightGBM for classical ML.
- Use Vertex AI for MLOps.
- Prefer reproducible scripts and small pipelines over notebook-only workflows.

### Testing
- Add or update tests when changing backend, batch, SQL, or ML pipeline code.
- For Rust, run cargo check/test where applicable.
- For Python, run the project's configured test command.

### Documentation and reporting
- When implementation is completed, summarize:
  - What changed
  - Why it changed
  - Impact scope
  - Tests performed
  - Remaining risks
  - Next actions
```

---

## 14. タスク依頼テンプレート

### 14.1 ヒアリング準備テンプレート

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

### 14.2 設計レビュー依頼テンプレート

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

### 14.3 実装依頼テンプレート

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

### 14.4 PRレビュー依頼テンプレート

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

### 14.5 報告文作成テンプレート

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

## 15. Cloud / DB / Terraform / Vertex AI の安全ルール

AIエージェントにCloudやDB操作を任せる場合、以下のルールを必須にします。

### 15.1 禁止する操作

```text
terraform apply
terraform destroy
本番DBへの直接UPDATE / DELETE / TRUNCATE
IAM権限の追加・削除
本番bucket / dataset / tableの削除
Vertex AI endpoint / model / jobの削除
本番環境のsecret更新
```

### 15.2 人間承認が必要な操作

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

### 15.3 AIに許可しやすい操作

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

## 16. 導入パターン

### 16.1 最小構成

| 用途 | ツール |
| --- | --- |
| 技術作業 | `Claude Code` |
| リーダー業務・報告 | `Codex` |

特徴:

- 低コストで始めやすい
- 設計、実装、テスト、報告まで広く対応
- IDE補完の快適さはやや不足する

### 16.2 推奨構成

| 用途 | ツール |
| --- | --- |
| 技術作業 | `Claude Code` |
| リーダー業務・報告 | `Codex` |
| IDE実装速度 | `Cursor` |

特徴:

- 質問者の業務範囲に最も合う
- リーダー業務と実装業務を両方支援できる
- React + pico.css、Python/Rust実装が速くなる

### 16.3 チーム運用構成

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

### 16.4 社内AI基盤構成

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

## 17. 価格とプランの整理

価格は変動しやすいため、導入前に必ず公式ページを確認してください。以下は `2026-05-03` 時点の公式ページ確認ベースのメモです。

| ツール | 価格 / プラン概要 | コメント |
| --- | --- | --- |
| GitHub Copilot | Free、Pro `$10/月`、Pro+ `$39/月` など。Premium requests の概念あり | GitHub PRレビュー、Copilot CLI、cloud agent、組織運用を重視する場合に検討 |
| Cursor | Hobby Free、Pro `$20/月`、Pro+ `$60/月`、Ultra `$200/月`、Teams `$40/user/月` | IDE実装速度を重視する場合に検討 |
| Codex | ChatGPT Plus / Pro / Business / Enterprise 等で利用。Business Codex は固定席料金なしの従量課金。Codex rate card は token-based credits | リーダー業務と実装委譲の両方に使いやすい。使用量管理が重要 |
| Claude Code | Claude Pro `$20/月`、Max 5x `$100/月`、Max 20x `$200/月` など | 技術作業の主力。利用量が多いならMax系も検討 |
| Devin | Free、Pro `$20/月`、Max `$200/月`、Teams `$80/月`、Enterprise custom | チケット委譲や検証込み作業向け。旧ACU前提の価格表は最新確認が必要 |
| DeepSeek | API従量課金。例: `deepseek-v4-flash` は入力 `$0.14/1M tokens`、出力 `$0.28/1M tokens` | 低コストAPI基盤として検討。割引やモデル名の変更に注意 |

---

## 18. このドキュメントで修正した主なポイント

| 修正内容 | 理由 |
| --- | --- |
| タイトルを「AIコーディングツール比較」から「AI開発支援ツール比較」に変更 | 質問者はリーダー業務も担当するため |
| 対象者を「テックリード兼プログラマー」と明記 | 実装速度だけでなく、設計、見積もり、報告も評価するため |
| リーダー業務とプログラミング業務を分離 | 見積もり、レビュー、ドキュメント、報告の意味が異なるため |
| `Meilisearch` をDWHではなくSearch枠に移動 | BigQuery / DuckDB と役割が異なるため |
| `Claude Code + Codex + Cursor` を中心構成に変更 | 技術作業、リーダー業務、IDE実装速度を分担できるため |
| Cloud / DB / Terraform / Vertex AI の安全ルールを追加 | AIエージェントに権限を渡す際の事故防止のため |
| テンプレートを追加 | 実際のヒアリング、設計、レビュー、実装、報告で再利用できるようにするため |
| 価格表を現行公式情報ベースに更新 | 元文書の一部価格表が変わりやすいため |

---

## 19. 最終判断

質問者の業務は、単なる実装ではなく、以下を横断します。

```text
ヒアリング
設計
スケジュール
見積もり
レビュー
ドキュメント
報告
詳細設計
コーディング
テストコード
動作検証
```

そのため、ツール選定は「コーディング性能ランキング」ではなく、**リーダー業務と実装業務を両方支える構成** として考えるべきです。

最終的には以下を推奨します。

```text
第一候補: Claude Code + Codex + Cursor
追加候補: GitHub Copilot
委譲候補: Devin
基盤候補: DeepSeek
```

特にこの技術スタックでは、`Claude Code` を技術作業の主力、`Codex` をリーダー業務と文書化の主力、`Cursor` を実装速度の主力に分けるのが最も現実的です。

---

## 20. 公式ソース

導入前に必ず最新情報を確認してください。

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

## 21. Claude Code 設定要素と他AI開発支援ツールの対応表

作成日: `2026-05-03`

対象概念:

- `CLAUDE.md`
- `Rules`
- `Agents / Subagents`
- `Skills`
- 補助概念として `Commands / Hooks / MCP / Review rules`

### 21.1 結論

このリポジトリでは、次のように整理するのが最も安全で運用しやすい。

| 用途 | 推奨する主ファイル / 機能 | 理由 |
| --- | --- | --- |
| プロジェクト全体の常時ルール | `AGENTS.md` + `CLAUDE.md` | Codex / Devin / GitHub Copilot が `AGENTS.md` を扱いやすく、Claude Code は `CLAUDE.md` が標準。GitHub Copilot cloud agent は `/CLAUDE.md` も読める。 |
| Claude Code 用の常時ルール | `CLAUDE.md` | Claude Code の中心。ビルド、テスト、禁止事項、アーキテクチャ概要を書く。 |
| Cursor 用の常時・条件付きルール | `.cursor/rules/*.mdc` | Cursor では Rules が主力。`Next.js禁止`、React + pico.css、Python/Rust などを明示する。 |
| GitHub Copilot 用のリポジトリ指示 | `.github/copilot-instructions.md` | Copilot Chat、Copilot code review、Copilot cloud agent に効かせやすい。 |
| GitHub Copilot 用のパス別指示 | `.github/instructions/*.instructions.md` | Python、Rust、Terraform、React、SQL などファイル種別ごとの指示に向く。 |
| Devin 用のリポジトリ指示 | `AGENTS.md` | Devin は `AGENTS.md` を coding 前に読む。セットアップ、テスト、PR運用を書く。 |
| Devin 用の組織・横断知識 | `Knowledge` | 組織共通のコーディング規約、デプロイ手順、PR命名規則などに向く。 |
| 繰り返しワークフロー | `.agents/skills/<skill-name>/SKILL.md` | Codex / Devin で明示的に使いやすい。テスト、デプロイ、Terraform plan review、Vertex AI review などに向く。 |
| PRレビュー規約 | `.github/copilot-instructions.md` + `REVIEW.md` | Copilot は custom instructions、Devin Review は `AGENTS.md` と `REVIEW.md` を読む。 |

---

### 21.2 Claude Code 概念別の対応表

| Claude Code の概念 | 役割 | Codex 相当 | Cursor 相当 | GitHub Copilot 相当 | Devin 相当 | 運用コメント |
| --- | --- | --- | --- | --- | --- | --- |
| `CLAUDE.md` | 常時読み込ませるプロジェクト規約、build/test、禁止事項、設計方針 | `AGENTS.md` / `AGENTS.override.md` | `AGENTS.md` または `.cursor/rules/*.mdc` | `.github/copilot-instructions.md`、`AGENTS.md`、`/CLAUDE.md` | `AGENTS.md`、Knowledge | 共通ソースは `AGENTS.md`、Claude Code 専用は `CLAUDE.md` にする。内容を二重管理しないよう、どちらかを生成元にする。 |
| `.claude/rules/` | 常時またはファイルパス別の細かいルール | ディレクトリ別 `AGENTS.md`、linters / hooks | `.cursor/rules/*.mdc` | `.github/instructions/*.instructions.md` | `AGENTS.md`、Knowledge、`REVIEW.md` | 言語別・ディレクトリ別ルールは各ツールの path-specific 機能に分ける。 |
| Subagents / custom agents | 専門役割を持つ分離コンテキストのエージェント | Subagents / custom agents | Subagents | Copilot custom agents、Copilot cloud agent、Copilot CLI custom agents | Agent Mode、Agent selector、Managed Devins | 並列調査やレビューは agents。日常ルールは agents ではなく rules / instructions に置く。 |
| Skills | 必要時に読み込む再利用可能な手順・知識・ワークフロー | `.agents/skills/<name>/SKILL.md` | Agent Skills | Copilot agent skills | `.agents/skills/<name>/SKILL.md` | テスト前確認、Terraform plan review、BigQuery SQL review、Vertex AI review などを skill 化する。 |
| Slash commands / commands | 手動で起動する定型プロンプト | Slash commands / skills | Commands | Copilot CLI slash commands / prompt files | Custom slash commands / Playbooks | 「毎回人間が起動するもの」は command。AIが必要時に選ぶものは skill。 |
| Hooks | 編集後・停止時などに決定的なスクリプトを実行 | Hooks | Hooks | Copilot CLI / cloud agent hooks | 明示的な hook 機能より Skills / Commands / CI で代替 | destructive な処理を直接 hook 化しない。lint/test/secret scan など安全な検証に使う。 |
| MCP | 外部ツール・DB・GitHub・Linear・Docs への接続 | MCP | MCP | MCP servers | Devin MCP / DeepWiki MCP | Cloud / DB / 本番系は権限を最小化する。 |
| Review rules | レビュー方針、検出したいバグパターン | Skill または AGENTS.md | Rules / Bugbot 対応ルール | `.github/copilot-instructions.md`、code review custom instructions | `REVIEW.md`、Devin Review | PRレビュー方針は通常の実装ルールと分けると運用しやすい。 |

---

### 21.3 ツール別の対応表

| ツール | `CLAUDE.md` 相当 | `Rules` 相当 | `Agents` 相当 | `Skills` 相当 | Commands 相当 | 備考 |
| --- | --- | --- | --- | --- | --- | --- |
| Claude Code | `CLAUDE.md` | `.claude/rules/` | Subagents | `.claude/skills/<name>/SKILL.md` | Slash commands / skills | 主力。CLI、Python/Rust、Terraform、DB、ML/MLOps に向く。 |
| Codex | `AGENTS.md`、`AGENTS.override.md` | ディレクトリ別 `AGENTS.md`、hooks / linters | Subagents / custom agents | `.agents/skills/<name>/SKILL.md` | Slash commands。旧 custom prompts は skills に寄せる | 設計整理、実装委譲、レビュー、ドキュメント化に向く。 |
| Cursor | `AGENTS.md`、`.cursor/rules` | `.cursor/rules/*.mdc` | Subagents / Cloud Agent | Agent Skills | `.cursor/commands` | IDE内実装速度、React + pico.css、局所修正に向く。 |
| GitHub Copilot | `.github/copilot-instructions.md`、`AGENTS.md`、`/CLAUDE.md` | `.github/instructions/*.instructions.md` | Copilot cloud agent、custom agents、CLI custom agents | Copilot agent skills | Copilot CLI commands / prompt files | GitHub PR、レビュー、組織運用に向く。 |
| Devin | `AGENTS.md`、Knowledge | Knowledge、`AGENTS.md`、`REVIEW.md` | Agent Mode、Agent selector、Managed Devins | `.agents/skills/<name>/SKILL.md` | Custom slash commands、Playbooks | チケット委譲、ブラウザ検証、PR作成、バックログ処理に向く。 |

---

### 21.4 この技術スタック向けの推奨配置

```text
repo-root/
  AGENTS.md
  CLAUDE.md
  REVIEW.md

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

#### 配置方針

| ファイル / ディレクトリ | 目的 | 主な対象ツール |
| --- | --- | --- |
| `AGENTS.md` | 全AIエージェント向けの共通ルール | Codex / Devin / GitHub Copilot / Cursor |
| `CLAUDE.md` | Claude Code 向けの最重要ルール | Claude Code / GitHub Copilot cloud agent |
| `REVIEW.md` | PRレビュー方針、検出したいバグパターン | Devin Review、レビュー運用全般 |
| `.github/copilot-instructions.md` | GitHub Copilot 全体向け指示 | GitHub Copilot |
| `.github/instructions/*.instructions.md` | 言語別・パス別 Copilot 指示 | GitHub Copilot |
| `.cursor/rules/*.mdc` | Cursor の常時・条件付きルール | Cursor |
| `.cursor/commands/*.md` | Cursor の手動起動ワークフロー | Cursor |
| `.agents/skills/*/SKILL.md` | 再利用可能な手順・知識・検証フロー | Codex / Devin / 一部Agent Skills対応ツール |

---

### 21.5 何を書くべきか

#### `AGENTS.md` に書く内容

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

#### `CLAUDE.md` に書く内容

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

#### `.cursor/rules/react-pico-no-nextjs.mdc` に書く内容

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

#### `.github/copilot-instructions.md` に書く内容

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

#### `REVIEW.md` に書く内容

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

#### `.agents/skills/terraform-plan-review/SKILL.md` の例

```md
---
name: terraform-plan-review
description: Review Terraform changes and terraform plan output for AWS/GCP safety risks before apply.
---

Use this skill when reviewing Terraform changes.

Steps:

1. Identify changed Terraform files.
2. Summarize resources added, changed, or destroyed.
3. Highlight IAM, service account, bucket, dataset, network, and production resource changes.
4. Check whether the change requires explicit human approval.
5. Do not run `terraform apply`.
6. Produce a risk summary with:
   - Safe changes
   - Risky changes
   - Unknowns
   - Required approvals
   - Suggested rollback or mitigation
```

---

### 21.6 使い分けルール

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

### 21.7 このリポジトリで最初に作るべきもの

優先順位は次の通り。

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

### 21.8 運用上の注意

- `AGENTS.md`、`CLAUDE.md`、`.github/copilot-instructions.md`、`.cursor/rules/project.mdc` に同じ内容を完全コピーし続けると、すぐに不整合が起きる。
- 共通ルールは `AGENTS.md` を source of truth にし、他ファイルでは「AGENTS.md に従う」と書いた上で、ツール固有の補足だけを書く。
- 長いAPI仕様、Terraform手順、Vertex AI手順、BigQueryコストレビュー手順は、常時指示ではなく `SKILL.md` に逃がす。
- 本番DB、IAM、Terraform apply、GCP/AWS本番リソース、Vertex AI本番endpointは、人間承認なしで実行させない。
- `Next.js禁止` は `AGENTS.md`、`CLAUDE.md`、`.cursor/rules/react-pico-no-nextjs.mdc`、`.github/copilot-instructions.md` のすべてに明記する。
