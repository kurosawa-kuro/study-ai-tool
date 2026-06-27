# プロジェクトドキュメント索引

このリポジトリの AI 制御（ハーネス）と作業運用の入口。`CLAUDE.md` は Claude Code の司令塔、`.claude/` は AI 制御一式、`docs/tasks/` は日次の作業計画・証跡。

## ハーネス（AI 制御）

AI エージェント制御の全体像は `.claude/README.md`（Kurosawa Thin Harness Architecture の実装）。アーキ本体とその repo 固有 instantiation は `docs/specs/`。

| ドキュメント | 役割 |
|---|---|
| [specs/kurosawa-thin-harness-architecture.md](./specs/kurosawa-thin-harness-architecture.md) | Thin Harness アーキ本体（tool-agnostic マスター） |
| [specs/runtime-protocol.md](./specs/runtime-protocol.md) | 実行手順と停止条件（repo 固有） |
| [specs/capability-boundary.md](./specs/capability-boundary.md) | 保護 capability と permissions 写像（脅威モデル） |
| [specs/change-boundary.md](./specs/change-boundary.md) | 保護パスと変更境界 |
| [specs/evidence-policy.md](./specs/evidence-policy.md) | Evidence Level と done の下限 |
| [specs/judgment-memory.md](./specs/judgment-memory.md) | 判断記憶のパイプライン |
| [templates/](./templates/) | 各 Layer のテンプレ |
| [decisions/decision-log.md](./decisions/decision-log.md) | 判断日誌（trade journal） |
| [memory/](./memory/) | 蒸留済み判断記憶 |
| [tasks/README.md](./tasks/README.md) | 日次運用の実行ハブ、作業計画 |

> このリポジトリにアプリ仕様が増えたら `docs/01_requirements.md` 〜 `08_release_runbook.md` を追加し、この索引に足す。
