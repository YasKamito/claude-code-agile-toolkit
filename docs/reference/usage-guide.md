# 使い方

> このファイルはツールキットの使い方ガイドです。
> CLAUDE.md から `@` 参照でオンデマンド読み込みされます。

---

### 初回（プロジェクト立ち上げ時）

1. `/tailoring` を実行 → プロジェクト特性ヒアリング → 採用プラクティス・Playbook生成
2. `/inception-deck` でプロダクトの方向性を揃える
3. `docs/requirements/project-brief.md` にプロジェクトの背景情報を記録
4. `/requirement` で要件定義 → 業務フロー・ストーリーマッピング・PBL導出
5. `docs/project/schedule.md` にスプリント日程と会議体を記録
6. Playbookに沿って、必要なskillを使いながらプロジェクトを進める

### Sprint 0（立ち上げスプリント）

Playbookの「Sprint 0でやること」に従い、開発基盤を整備

### バックログリファインメント

```
/requirement が出力した product-backlog.md（粗いPBI一覧）
  │
  ↓
/refinement check（PBI詳細化 + INVESTチェック）
  └─ 要件定義成果物（BR/SCR等）を自動参照
  └─ 対話でAC作成・タスク分解
  └─ → docs/backlog/PBI-xxx.md（個別PBIファイル）
  └─ → docs/backlog/index.md（ステータス更新: Draft → Ready）
  │
  ↓
/refinement ac（AC品質チェック — 必要に応じて）
  └─ 具体性・検証可能性・完了判断をチェック
  └─ Given-When-Then形式への変換（オプション）
  │
  ↓
Ready状態のPBIが十分に揃ったらスプリントプランニングへ
```

### スプリントサイクル

```
Sprint Planning（月曜）
  └─ /planning → docs/sprints/sprint-N/planning.md
  └─ Ready状態のPBIからスプリントに投入するPBIを選択
      ↓
Daily（毎日）
  └─ /daily → docs/sprints/sprint-N/daily-log.md
  └─ SMがDevの報告をとりまとめて入力（Slack等から）
  └─ PBI状態はDevが管理・更新
      ↓
Sprint Review（金曜）
  └─ /sprint-review → docs/sprints/sprint-N/review.md
      ↓
Retrospective（金曜）
  └─ /retro → docs/sprints/sprint-N/retrospective.md
  └─ ベロシティ = 完了PBIのポイントのみ（部分完了は含めない）
  └─ planning.md の実績セクションも更新
```

### マイルストーン時

Playbookの「マイルストーンごとにやること」に従い、
リリース判定、ステークホルダー報告等を実施する。

---

## セッション管理ベストプラクティス

### `/clear` のタイミング

| 遷移 | 推奨 |
|------|------|
| `/tailoring` → `/inception-deck` | **クリア推奨**。tailoringの参照資料がコンテキストに残る |
| `/inception-deck` → `/requirement` | **クリア推奨**。異なるコンテキストが必要 |
| `/requirement` 内（長時間セッション） | アプローチ選定後、アクティビティ実行前にクリア検討 |
| `/refinement check` 複数PBI | **3-5 PBIごとにクリア**。PBIごとに異なる要件参照が必要 |
| スプリントサイクル内（planning→daily→review→retro） | **セッション継続OK**。スプリントコンテキストを共有 |
| `/daily` を日をまたいで | **日ごとに新セッション**。デイリーは軽量 |

### コンテキストサイズ目安

| スキル | コンテキスト負荷 | 備考 |
|--------|----------------|------|
| `/tailoring` | **重い**（参照ファイル2つを追加読み込み） | 初回のみ、完了後クリア |
| `/requirement` | **重い**（アクティビティ間で蓄積） | 長時間セッションに注意 |
| `/refinement-check` | **中**（PBIごとに要件参照） | バッチ処理にはサブエージェント活用 |
| `/planning` | 軽〜中 | スプリントファイルのみ |
| `/daily` | **軽い** | 当日スプリントファイルのみ |
| `/inception-deck` | **軽い** | インクリメンタル設計 |
| `/refinement-ac` | **軽い** | 単一PBIフォーカス |
| `/sprint-review` | 軽〜中 | 当スプリントファイル |
| `/retro` | **中** | 当＋前スプリント参照 |

### サブエージェントの活用

大量のファイルを読み込む必要がある場合は、サブエージェント（`.claude/agents/`）を活用する。
サブエージェントは別のコンテキストウィンドウで実行され、メインの会話を圧迫せずに要約を返す。

| サブエージェント | 用途 | 活用スキル |
|----------------|------|-----------|
| `backlog-reader` | PBI一覧の分析・サマリー | `/planning`, `/refinement-check` |
| `requirements-reader` | 特定PBIに関連する要件の抽出 | `/refinement-check` |
| `sprint-summarizer` | スプリント全体の要約 | `/retro`, `/planning` |
