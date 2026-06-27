# AGENTS.md

AI コーディングエージェント共通の作業ガイド。ツール固有指示は各ツールのファイル（Claude Code は `CLAUDE.md`）に置く。

## プロジェクト概要

TODO: このリポジトリが何を扱うか。

## Harness（AI 制御一式）

- AI 制御の全体像は `.claude/README.md`（Kurosawa Thin Harness Architecture の実装）。
- アーキ本体は `docs/specs/kurosawa-thin-harness-architecture.md`、repo 固有の脅威モデルは `docs/specs/{capability-boundary,change-boundary,runtime-protocol,evidence-policy,judgment-memory}.md`。
- skills は classify-task → create-task → scan-decisions → plan-skeleton → execute-task → verify-completion → review-task のライフサイクル（16本、詳細は `.claude/README.md`）。
- permissions の ask/deny と保護パスは脅威モデルで決める。他プロジェクトの設定をそのまま移植しない。
