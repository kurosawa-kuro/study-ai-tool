# CLAUDE.md

このファイルは Claude Code がこのリポジトリで作業する際の最小ガイド。

## プロジェクト概要

TODO: このリポジトリが何を扱うか（現状はドキュメント / ノート中心）。

## Source of Truth

- Documentation index: `docs/00_index.md`
- Harness 全体像: `.claude/README.md`
- Task notes: `docs/tasks/`

## ハーネス（AI 制御）

このリポジトリの AI エージェント制御一式は `.claude/README.md`（Kurosawa Thin Harness Architecture の実装）。アーキ本体は `docs/specs/kurosawa-thin-harness-architecture.md`（tool-agnostic マスター）、repo 固有の脅威モデルは `docs/specs/{runtime-protocol,capability-boundary,change-boundary,evidence-policy,judgment-memory}.md`。判断日誌は `docs/decisions/decision-log.md`、蒸留記憶は `docs/memory/`。

- **Scope invariant（常時）**: 作業中に Goal/Scope 外の変更・前提崩れ・追加副作用・docs と実装の矛盾を見つけたら即実装しない。`control-change` で分類するか follow-up task に残す。「ついで修正/共通化/改名」は scope creep。
- permissions（`.claude/settings.json`）は安全既定: 生インフラ/DBツールは ask、不可逆破壊のみ deny。このプロジェクトの脅威モデルに合わせ `docs/specs/capability-boundary.md` と一緒に調整する。

### AI Runtime Protocol（薄い・常時）

詳細は `.claude/README.md` と `docs/specs/runtime-protocol.md`、停止条件は同 §「停止して owner に確認する」。

1. Goal / Scope / Done を言い直す。
2. Weight Class を判定（Light / Standard / Heavy → `classify-task`）。Light は以降の重い手順をスキップしてよい。
3. owner-only 判断・保護 capability・allowed/forbidden paths を確認（`scan-decisions` / `capability-boundary.md` / `change-boundary.md`）。
4. Standard/Heavy は skeleton + TDD contract を先に（`plan-skeleton`）。
5. scope 内で最小変更を当てる。複数 attempt が要るなら停止条件付きで回す（`reconcile-task`）。
6. 必要 Evidence Level（≥2、本番は4）で検証（`verify-completion` / `evidence-policy.md`）。
7. scope 拡大・保護境界接触・二度違う理由で検証失敗 → 停止して owner へ。
8. 非自明な判断を下した時だけ記録（`log-decision`）。
