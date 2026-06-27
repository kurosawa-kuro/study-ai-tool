# Capability Boundary（このプロジェクトの instantiation）

**Layer 4: Capability Boundary**（§12）の repo 固有 instantiation。マスター文書 [`kurosawa-thin-harness-architecture.md`](./kurosawa-thin-harness-architecture.md) は保護対象を一般的に列挙する。このファイルは*このリポジトリ*のツールと `.claude/settings.json` の強制層へ写像する。Skill: `assess-risk`。

## 脅威モデルを先に（このプロジェクトで壊れたら困るもの）

> TODO: このプロジェクトの **loss-critical 資産**を1〜数点で言語化する（例: 本番DB / マスターデータ / 顧客データ / secret）。「壊れても再構築でき金銭損害ゼロ」なら raw ツールを allow に緩めてよい。loss-critical があるなら ask/deny を厚くする。

このテンプレは**安全既定**: 生インフラ/DBツールは ask、不可逆なコード/履歴破壊のみ deny。守る対象が決まったら下表を埋めて settings.json と一致させる。

## 強制マップ（settings.json と一致させる）

| Capability | このリポジトリのツール | 強制 |
|---|---|---|
| 安全な開発ループ | `make test`/`lint`/`build`、`cargo *`、read-only git | **allow** |
| 生インフラ/DBツール | `gcloud`/`aws`/`terraform`/`psql`/`turso`/`supabase` | **ask**（TODO: 脅威モデル次第で allow へ） |
| secret 注入ランナー | `doppler run` | **ask** |
| push | `git push` | **ask** |
| 保護ファイルパス編集 | `env/secret/**`、`infra/**`、`terraform/**`、`.github/workflows/**` | **`detect-safety-boundary` hook (exit 2)** |
| 不可逆なコード/履歴破壊 | `rm -rf`、`git push --force*`、`git reset --hard` | **deny** |
| 本番デプロイ / 本番データ書込 / migration / 課金 | TODO: このプロジェクトの該当コマンド | **ask または owner approval** |

## 保護 capability チェックリスト（`assess-risk` / `docs/templates/capability-boundary.md` 参照）

- [ ] 本番デプロイ
- [ ] DB migration / schema 変更
- [ ] secret read/write
- [ ] IAM / 権限変更
- [ ] クラウドリソース作成/削除
- [ ] 課金 / quota
- [ ] 破壊的コマンド
- [ ] 本番 / マスターデータアクセス
- [ ] 認証 / 認可ロジック

## ルール

- `ask`/`deny` は*コマンド*の正本。`detect-safety-boundary` hook は*ファイル編集*をカバーする（コマンド層が見ない隙間）。
- この設定は**このプロジェクト固有**。脅威モデルが違う他プロジェクトへそのまま移植しない。
