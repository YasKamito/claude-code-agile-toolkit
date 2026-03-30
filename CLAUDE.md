# AI駆動プロジェクト運営ツールキット

このCLAUDE.mdは、アジャイル開発プロジェクトの運営をAIが支援するためのツールキットです。
プロジェクトの特性に応じてプラクティスをテーラリングし、選ばれたプラクティスに対応するskillsを提供します。

---

## 思想

### 崩さない原則（背骨）

どんな案件でも、以下は変わらない：

1. **最初にすべての要件は決められない** — 不確実性を受け入れ、学びながら進む
2. **小さく作って小さくリリースする** — ビッグバンリリースはしない
3. **リリースのリズムを作る** — 持続可能なデリバリーのペースを確立する
4. **フィードバックループで軌道修正する** — 作って→見せて→学んで→直す
5. **動くソフトウェアが進捗の指標** — ドキュメントの量ではなく

### テーラリングの考え方

アジャイルな進め方は「背骨」であって選択肢ではない。
テーラリングするのは、**その背骨の上にどんなプラクティスを載せるか**。

```
❌ WF or アジャイル、どっちを選ぶ？
✅ アジャイルで進める。その中でどんなプラクティスが必要？
```

プロジェクトの特性（契約形態、チーム規模、ドメインの不確実性など）に応じて、
必要なプラクティスを選定する。「自分たちの進め方は、自分たちで選ぶ」という考え方。

---

## プロジェクト固有情報の管理

`/tailoring` を実行すると、以下の情報が `docs/project/project-setting.md` に記録される:
- プロジェクト情報（名前、チーム構成、スプリント期間等）
- プロジェクト特性（C1-C10のヒアリング結果）
- 採用プラクティス
- Playbook（Sprint 0 でやること、毎スプリントでやること）

各スキルは実行時に `docs/project/project-setting.md` を読み込み、
プロジェクト固有のコンテキスト（採用プラクティス、Playbook等）を把握する。

プロジェクトの背景情報（顧客情報、ステークホルダー、制約・前提等）は
`docs/requirements/project-brief.md` に記録する。

---

## テーラリング参照資料

プロジェクト特性のヒアリング項目（C1-C10）とプロセスゴール詳細は、
`/tailoring` 実行時にオンデマンドで参照する資料として分離している。

- **プロジェクト特性ヒアリング項目**: @docs/reference/tailoring-criteria.md
- **プロセスゴール詳細（17ゴール）**: @docs/reference/process-goals.md

> `/tailoring` 以外のスキルは、テーラリング結果が記録された `docs/project/project-setting.md` を参照する。

---

## Skills一覧

### メタskill（プロジェクト設計）

| skill | 説明 | 状態 |
|-------|------|------|
| `/tailoring` | プロジェクト特性をヒアリングし、コンテキストに基づいてプラクティスを推薦する。採用プラクティスとPlaybookを`docs/project/project-setting.md`に記録する | ✅ |

### 立ち上げ（Inception）

| skill | 説明 | 状態 |
|-------|------|------|
| `/inception-deck` | インセプションデッキ作成を対話形式で支援する。10の問いにインクリメンタルに回答でき、追加・更新が可能 | ✅ |

### 要件定義（Requirement）

| skill | 説明 | 状態 |
|-------|------|------|
| `/requirement` | プロジェクト特性に応じた要件定義を対話形式で支援する。RDRAの段階的詳細化をベースに、テーラリング結果に応じてLight/Standard/Thoroughの3段階のアプローチを提案し、追加資料も活用して要件を整理する。成果物は`docs/requirements/`に出力し、PBL初期版を導出する | ✅ |

### 計画（Planning）

| skill | 説明 | 状態 |
|-------|------|------|
| `/refinement check` | PBIのINVEST基準チェック | ✅ |
| `/refinement ac` | 受け入れ条件の品質チェック | ✅ |
| `/release-plan` | リリース計画の策定支援 | 🔲 今後実装予定。現在は `/planning` のスプリント単位計画と、`/tailoring` のリリース方針推薦でカバー |
| `/test-strategy` | テスト戦略の策定支援 | 🔲 今後実装予定。現在は `/tailoring` のテスト戦略推薦（CI組み込み/ATDD/探索的テスト等）でカバー |
| `/planning` | スプリントプランニング支援 | ✅ |

### 実行（Execution）

| skill | 説明 | 状態 |
|-------|------|------|
| `/daily` | デイリースクラム支援 | ✅ |

### ふりかえり（Review & Retrospective）

| skill | 説明 | 状態 |
|-------|------|------|
| `/sprint-review` | スプリントレビュー支援 | ✅ |
| `/retro` | レトロスペクティブ支援 | ✅ |

---

## docs構造

```
docs/
├── reference/                             # 参照資料（テーラリング用。スキルがオンデマンドで読み込む）
│   ├── tailoring-criteria.md              # C1-C10 プロジェクト特性ヒアリング項目
│   ├── process-goals.md                   # 17プロセスゴールの詳細・推薦条件
│   └── usage-guide.md                     # ツールキットの使い方・セッション管理ベストプラクティス
├── project/
│   ├── project-setting.md                 # /tailoring の出力（プロジェクト設定・採用プラクティス・Playbook）
│   ├── inception-deck.md                  # /inception-deck の出力
│   ├── schedule.md                        # スプリント日程・会議体情報
│   └── risk-backlog.md                    # リスクバックログ（初期: /inception-deck Q4、更新: /retro）
├── requirements/
│   ├── project-brief.md                   # プロジェクトブリーフ（顧客情報・ステークホルダー・制約）
│   ├── inputs/                            # 追加資料（RFP、議事録等）の格納先
│   ├── business-flow.md                   # 業務フロー（/requirement: Standard以上）
│   ├── story-mapping.md                   # ユーザーストーリーマッピング（/requirement: 全レベル）
│   ├── business-rules.md                  # ビジネスルール（/requirement: Standard以上）
│   ├── screen-list.md                     # 画面一覧・遷移（/requirement: 全レベル）
│   ├── product-backlog.md                 # PBL初期版（/requirement の最終成果物）
│   ├── traceability.md                    # トレーサビリティマトリクス（BR/SCR ↔ PBI 双方向参照）
│   ├── non-functional.md                  # 非機能要件（/requirement: Standard以上）
│   ├── context.md                         # システム外部環境（/requirement: Thoroughのみ）
│   └── domain-model.md                    # ドメインモデル（/requirement: Thoroughのみ）
├── backlog/
│   ├── index.md                           # PBIインデックス（ステータス管理。/refinement check で初期化）
│   └── PBI-xxx.md                         # 個別PBIファイル（AC・タスク・INVEST結果。/refinement check で作成）
├── adr/
│   └── ADR-NNN_タイトル.md                # アーキテクチャ判断記録（要件定義中に発生した技術判断）
└── sprints/
    └── sprint-N/
        ├── planning.md                    # /planning の出力
        ├── daily-log.md                   # /daily の出力（日付ごとに追記）
        ├── review.md                      # /sprint-review の出力
        └── retrospective.md               # /retro の出力

examples/                                  # ← サンプルプロジェクトの出力例（リポジトリルート直下）
└── iot-alert/
    └── docs/                              # IoT Alert案件（架空）でのスキル実行結果一式
```

**ルール:**
- プロジェクト全体に関わる情報は `docs/project/` に置く
- スプリント固有の情報は `docs/sprints/sprint-N/` に置く
- 各スキルは上記のパスに出力する。フォルダが存在しなければ作成する
- `examples/` はサンプルプロジェクトの出力例。実際のプロジェクトでは参照のみ

---

## 使い方

@docs/reference/usage-guide.md

---

## コンテキストウィンドウ最適化

### Compact Policy

`/compact` 実行時に以下を保持すること:
1. 現在のスプリント番号とスプリントゴール
2. アクティブなPBI ID一覧とステータス
3. 未解決のブロッカー
4. 実行中のスキル名とステップ番号
5. セッション中のユーザー判断（アプローチ選択、プラクティス調整等）
6. 書き出し済みファイルのパス（内容の保持は不要 — 再読み込み可能）

破棄してよいもの:
- ディスクに保存済みのファイル全文
- 保存済み成果物に至る途中の対話
- サマリーが記録済みの詳細チェック結果

### サブエージェント

大量のファイル読み込みが必要な場合は `.claude/agents/` のサブエージェントを活用する。
サブエージェントは別のコンテキストウィンドウで実行され、メインの会話を圧迫せずに要約を返す。

| サブエージェント | 用途 |
|----------------|------|
| `backlog-reader` | PBI一覧の分析・ステータス別サマリー |
| `requirements-reader` | 特定PBIに関連する要件の抽出 |
| `sprint-summarizer` | スプリント全体の要約（planning + daily + review） |

---

*このツールキットは Claude Code のカスタムスラッシュコマンド（skills）として動作します。*
