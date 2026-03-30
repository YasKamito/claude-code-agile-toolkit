# 業務フロー — IoTアラート

> 最終更新: 2026-03-30

## 概要

東海ミスリル工業の工場保全業務における3つの主要フローを整理する。
現状（AS-IS）の課題を明確にし、システム導入後（TO-BE）でどう変わるかを定義する。

---

## 主要業務フロー

### フロー1: 設備監視フロー（日常の監視業務）

#### AS-IS（現状）

```mermaid
flowchart TD
    A[出勤・引き継ぎ] --> B[PLC画面を1台ずつ巡回確認]
    B --> C{異常あり?}
    C -->|あり| D[経験と勘で判断]
    C -->|なし| E[次の巡回まで待機]
    E --> F[2時間ごとに巡回]
    F --> B
    D --> G[対応 or エスカレーション]

    style B fill:#fdd,stroke:#c00
    style D fill:#fdd,stroke:#c00
    style F fill:#fdd,stroke:#c00
```

**課題:**
- PLC画面の巡回に毎回約30分かかる（17台を1台ずつ）
- 巡回間隔（2時間）の間に異常が進行するリスク
- 判断がベテラン（本田さん）の経験に依存
- 夜勤帯は1名体制で巡回品質が低下

#### TO-BE（システム導入後）

```mermaid
flowchart TD
    A[出勤] --> B[ダッシュボードで全設備を一覧確認]
    B --> C{赤・黄シグナルあり?}
    C -->|あり| D[トレンドグラフで詳細確認]
    C -->|なし| E[通常業務]
    E --> F[Slack通知を待つ]
    F --> G{通知受信?}
    G -->|あり| D
    G -->|なし| F
    D --> H[通知レベルに応じて対応判断]

    style B fill:#dfd,stroke:#0a0
    style D fill:#dfd,stroke:#0a0
    style F fill:#dfd,stroke:#0a0
    style H fill:#dfd,stroke:#0a0
```

#### システムの関与ポイント

| 区分 | 内容 |
|------|------|
| 【自動化】 | PLC巡回 → ダッシュボードでリアルタイム一覧表示 |
| 【自動化】 | 異常検知 → 閾値ベースの自動判定・通知 |
| 【効率化】 | 経験による判断 → トレンドグラフで数値ベースの判断支援 |
| 【新規】 | 巡回間隔の待機 → リアルタイム監視 + プッシュ通知 |

---

### フロー2: 異常検知・対応フロー（アラート発生から対応完了まで）

#### AS-IS（現状）

```mermaid
flowchart TD
    A[設備から異音・異常値] --> B{気づける?}
    B -->|気づいた| C[ベテランが経験で判断]
    B -->|気づけない| D[故障発生]
    D --> E[生産ライン停止]
    E --> F[事後対応・修理]
    C --> G{対応必要?}
    G -->|はい| H[設備停止・保全作業]
    G -->|監視継続| I[紙ノートに記録]
    H --> J[復旧・再稼働]

    style B fill:#fdd,stroke:#c00
    style D fill:#fdd,stroke:#c00
    style E fill:#fdd,stroke:#c00
    style I fill:#fdd,stroke:#c00
```

**課題:**
- 異常に気づけないケースがある（特に夜勤帯）
- 故障してから対応 = 事後保全。予防保全ができていない
- 記録が紙ノートで検索・分析不可
- 夜勤の担当者は判断に迷い、深夜にベテランに電話する

#### TO-BE（システム導入後）

```mermaid
flowchart TD
    A[センサーデータ取得] --> B[閾値判定]
    B --> C{通知レベル}
    C -->|注意 80%超| D[Slack通知]
    C -->|警告 閾値超| E[Slack + メール通知]
    C -->|異常 120%超/センサー断| F[全員に緊急通知]
    D --> G[担当者がダッシュボードで確認]
    E --> G
    F --> G
    G --> H[トレンドグラフで状況判断]
    H --> I{対応判断}
    I -->|経過観察| J[監視継続。アラート履歴に記録]
    I -->|要対応| K[保全作業実施]
    K --> L[復旧確認。アラート解消]

    style A fill:#dfd,stroke:#0a0
    style B fill:#dfd,stroke:#0a0
    style D fill:#dfd,stroke:#0a0
    style E fill:#dfd,stroke:#0a0
    style F fill:#dfd,stroke:#0a0
    style H fill:#dfd,stroke:#0a0
    style J fill:#dfd,stroke:#0a0
```

#### システムの関与ポイント

| 区分 | 内容 |
|------|------|
| 【自動化】 | 異常検知 → センサーデータの閾値自動判定 |
| 【自動化】 | 通知 → レベル別の自動通知（Slack/メール） |
| 【自動化】 | 通信異常検知 → センサー断のアラート |
| 【効率化】 | 判断支援 → トレンドグラフ・過去データの可視化 |
| 【新規】 | アラート履歴の自動記録 |

---

### フロー3: 閾値設定・管理フロー

#### AS-IS（現状）

```mermaid
flowchart TD
    A[本田さんの頭の中に閾値がある] --> B[設備ごとに全部違う]
    B --> C[金型の種類でも変わる]
    C --> D[若手に伝えられない]

    style A fill:#fdd,stroke:#c00
    style D fill:#fdd,stroke:#c00
```

**課題:**
- 閾値が暗黙知（本田さんの頭の中）
- 設備ごと × 金型種類ごとに異なり、複雑
- 若手への知識移転ができていない

#### TO-BE（システム導入後）

```mermaid
flowchart TD
    A[保全担当者が閾値設定画面を開く] --> B[設備を選択]
    B --> C[センサー項目ごとに閾値を設定]
    C --> D[注意80% / 警告100% / 異常120% の3段階]
    D --> E[保存 → 即時反映]
    E --> F[変更履歴が記録される]

    style A fill:#dfd,stroke:#0a0
    style C fill:#dfd,stroke:#0a0
    style E fill:#dfd,stroke:#0a0
    style F fill:#dfd,stroke:#0a0
```

#### システムの関与ポイント

| 区分 | 内容 |
|------|------|
| 【新規】 | 閾値のデジタル管理（暗黙知→形式知） |
| 【効率化】 | IT部門に依頼不要。保全担当者が自分で設定変更 |
| 【新規】 | 変更履歴の自動記録（いつ誰が何を変えたか） |

---

## データフロー概要

```mermaid
flowchart LR
    PLC[PLC/センサー] -->|センサーデータ| GW[PLCゲートウェイ]
    GW -->|REST API/JSON| Cloud[クラウド基盤]
    Cloud -->|閾値判定| Alert[アラートエンジン]
    Alert -->|通知| Slack[Slack]
    Alert -->|通知| Email[メール]
    Cloud -->|表示| Dashboard[ダッシュボード]
    Cloud -->|蓄積| DB[(データストア)]
    User[保全担当者] -->|閾値設定| Dashboard
    User -->|確認| Dashboard
    Manager[管理者] -->|確認| Dashboard
```
