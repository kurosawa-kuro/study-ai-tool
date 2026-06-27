# Evidence Policy（このプロジェクトの instantiation）

**Layer 8: Evidence / Reality Check**（§16）の repo 固有 instantiation。汎用 Evidence Level を*このリポジトリ*の確実な成果へ写像する。Skill: `verify-completion`（+ `check-claims`）。reminder hook: `detect-unverified-claim`。

## 中核ルール

> エージェントの「完了しました」は証拠ではない。証拠とは diff / test 出力 / コマンドログ / 実ファイル / 実行時の挙動 / 永続化された成果である。
> **done は Evidence Level ≥2 が下限（docs-only タスクを除く）。本番影響のある変更は Level 4 + owner approval。**

## Evidence Level

| Level | 名前 | これで証明される |
|---:|---|---|
| 0 | Claim Only | 「動くはず」 — 受理しない |
| 1 | Static | diff、実ファイル内容、`cargo check` / 型チェック — docs-only や trivial 編集 |
| 2 | Test | `make test` 出力、修正前fail→修正後pass — **標準の下限** |
| 3 | Runtime | ローカル実行＋永続化された成果（対象データが期待通り増減 / 重複なし / 状態が正しく保存）。TODO: このプロジェクトの「確実な成果」を定義 |
| 4 | Production | 本番ログ / 監視チェック / ロールバック準備（`docs/08_release_runbook.md`）— owner approval 必須 |

## 確実な成果（作業に応じて最低1つ確認する）

> TODO: このプロジェクト固有の durable outcome を列挙する（例: 対象行が実際に増えた / 重複が入っていない / 通知が二重送信されていない / 結果行が保存された / docs が同一変更で更新された）。

## ルール

- すべての PASS に `file:line` かコマンド出力を引用する。未証明は UNVERIFIED であり PASS にしない。
- 確実な成果チェックの無い「緑のワークフロー」は最大でも Level 1 で、done ではない。
- 本番検証は runbook（`docs/08_release_runbook.md`）であって、ビルド成功ではない。
