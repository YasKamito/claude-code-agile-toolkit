# IoT Alert サンプルプロジェクト

> **注意: このサンプルに登場する企業名・人物名・プロダクト名はすべて架空のものです。**
> 実在の企業・団体・個人とは一切関係ありません。

---

## 概要

AI駆動プロジェクト運営ツールキットの全スキルを通しで実行した場合の出力例です。

**架空案件の設定:**
- 顧客: 東海ミスリル工業 株式会社（架空の自動車部品メーカー）
- プロダクト: IoT Alert — 工場設備のセンサー監視・異常通知システム
- チーム: PO 1名、SM 1名、Dev 3名（計5名）

## スキル実行順序

```
/tailoring        → docs/project/project-setting.md
/inception-deck   → docs/project/inception-deck.md, risk-backlog.md
/requirement      → docs/requirements/ 配下
/refinement check → docs/backlog/ 配下
/planning         → docs/sprints/sprint-1/planning.md
/daily            → docs/sprints/sprint-1/daily-log.md
/sprint-review    → docs/sprints/sprint-1/review.md
/retro            → docs/sprints/sprint-1/retrospective.md
```

## ファイル構成

```
docs/
├── project/          # プロジェクト設定・インセプションデッキ・リスク等
├── requirements/     # 要件定義成果物（業務フロー・ストーリーマッピング・PBL等）
│   └── inputs/       # 追加資料（プリセールスヒアリング等）
├── backlog/          # リファインメント済みPBIファイル
├── sprints/sprint-1/ # Sprint 1 の計画・デイリー・レビュー・レトロ
└── adr/              # アーキテクチャ判断記録
```
