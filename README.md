# SFn道場 — AWS Step Functions / JSONata を手を動かして身につける

AWS Step Functions を「読む → 式を書く → 機械を組む」の 3 段で実務レベルへ橋渡しする学習教材一式です。
ブログ連載・JSONata ドリルアプリ・ハンズオン設計書・実機デプロイ用テンプレートで構成されます。

> **コンセプト**: Lambda を 1 個も書かずに AWS サービスを直接叩く（最適化統合 / SDK 統合）感覚を、最小の題材で 1 つずつ体得する。

---

## 学習導線

| 段階 | 教材 | 形態 |
|---|---|---|
| **① 読む** | ブログ連載「ゼロから学ぶ Step Functions」全5回 | Markdown 解説記事 |
| **② 式を書く** | JSONata ノック道場（`Step-Functions-/index.html`） | ブラウザで動く 50 問ドリル |
| **③ 機械を組む** | SFn道場 ハンズオン（24 カタ） | AWS アカウント上で実機構築 |

JSONata ノック道場が「式（部品）」の練習、ハンズオンが「ステートマシン（完成品）」の練習。

---

## ディレクトリ構成

```
SFn道場/
├─ README.md                              … 本ファイル
├─ Step-Functions-/                       … 公開アプリ（git: ukkiy8787/Step-Functions-）
│  ├─ index.html                          … JSONata ノック道場（単一HTML・オフライン動作）
│  ├─ contact-form-lab.html               … 問い合わせフォーム実機ラボ（Launch Stack）
│  └─ templates/contact-form.yaml         … 素の CloudFormation テンプレート
├─ .selftest/                             … Node セルフテストハーネス（採点回帰テスト）
│
├─ AWS Step Functions 入門 第0回 全体地図編.md   … 【第0回】全体地図
├─ AWS Step Functions 入門.md                    … 【第1回】入門（指揮者のはなし）
├─ AWS Step Functions 入門 第2回 ハンズオン編.md … 【第2回】ハンズオン & JSONata
├─ AWS Step Functions 入門 第3回 ノック道場編.md … 【第3回】ノック道場のすすめ
├─ AWS Step Functions 入門 第4回 実践設計編.md   … 【第4回】実践設計（上級者編）
│
├─ SFn道場 ハンズオン設計書.md            … ハンズオン 24 カタの設計書
├─ JSONata ノック道場 設計書 v2.md         … ノック道場アプリの設計書（最新）
├─ JSONata ノック道場 設計書.md            … 同（旧版）
├─ Replace-Lambda シリーズ 詳細設計.md     … Lambda 置き換え題材の詳細設計
├─ Replace-Lambda Lv9-Lv10 PROBLEMS.js     … Lv9/Lv10 問題データの原本
└─ index-archaive.html                     … 旧版アプリ（アーカイブ）
```

---

## ② JSONata ノック道場（`Step-Functions-/index.html`）

JSONata 式だけをドリルで鍛える単一 HTML アプリ。**全 50 問 / 10 レベル × 5 問**。
`index.html` をブラウザで開くだけで動作します（`file://` でも可・オフライン動作・サーバ不要）。

| Lv | テーマ | 学ぶこと |
|---|---|---|
| 1 | アクセス基礎 | パス参照・配列インデックス・射影・フィルタ |
| 2 | 変換 | 文字列連結・オブジェクト構築・三項・正規化 |
| 3 | 集計・並べ替え | `$sum`/`$count`・フィルタ集計・ソート `^()`・`$distinct` |
| 4 | 関数を使いこなす | 日時変換・`$merge`・`$keys`・`$exists` + デフォルト値 |
| 5 | Step Functions 実践 | `$states.result`・`$parse`・Variables・ネスト集計 |
| 6 | 高階関数 | `$map`/`$filter`/`$reduce`/`$sort`・グルーピング |
| 7 | 文字列・数値・日時 | `$split`/`$replace`/`$substringAfter`/`$formatNumber`/`$fromMillis` |
| 8 | Step Functions 実戦 | Map 集計・Catch 整形・Assign 加算・body パース集計 |
| 9 | 直接統合：入力を取り出す | API GW body / Pipes・SQS / Slack blocks / Bedrock / ECS（Lambda 置き換え） |
| 10 | 直接統合：結果を組み立てる | Bedrock 応答 / DynamoDB 型付き形式 / reCAPTCHA / 終了コード分岐 |

### 主な機能
- 各問に **お題・ヒント・模範解答・解説** を収録。`= vs ==`、`??`/`?:`/`$eval` 非対応など SFn 固有のつまずきを明示
- **採点エンジン**: 構造一致 + 浮動小数の許容誤差比較 + 順不同採点。裸のフィールド参照は本番同様に不正解
- **本番 SFn 非対応構文の lint**: `$eval`/`$now`/`$millis`/`$random`/`??`/`?:` を検出して警告（道場のブラウザ版 JSONata では動くが本番で落ちる構文をガード）
- **ASL コピー**: 各問を「空入力 `{}` でそのまま実機実行できる ASL」に変換してクリップボードへ
- 進捗の localStorage 保存・ダーク/ライトテーマ・チートシート・モバイル対応

---

## ③ SFn道場 ハンズオン（`SFn道場 ハンズオン設計書.md`）

ステートマシンを 1 本まるごと作って動かすカタ集。**Lv.0（環境準備）＋本編 Lv.1〜6 の全 24 カタ**。

| Lv | 章テーマ | カタ数 | 主に学ぶこと |
|---|---|---|---|
| 0 | 道場入門（環境とコスト見張り） | 1 | IAM・CLI・Budgets・コンソール準備 |
| 1 | 基礎の型 | 5 | Wait / Choice / Map / Parallel / Catch |
| 2 | 起動と連携 | 2 | CLI・SDK・EventBridge・API Gateway からの起動 |
| 3 | データ変換（JSONata） | 2 | 入出力加工・バリデーション（ノック道場と接続） |
| 4 | 実行モデルと構造化 | 3 | IaC 化（SAM/CDK）・Standard/Express・Fragment |
| 5 | 高度なパターン | 4 | Callback / ETL / Human-in-the-loop / HTTP Task |
| 6 | Lambda 置き換え（SDK統合） | 8 | SQS / Bedrock / チャット通知 / 問い合わせフォーム / ECS / DynamoDB CRUD |

> 記法は **JSONata（`QueryLanguage: JSONata`）に統一**。旧 JSONPath 記法は「読めるように」必要箇所で注記。

---

## 実機ラボ（問い合わせフォーム）

`Step-Functions-/contact-form-lab.html` は Lambda を使わない問い合わせフォームの実機デプロイラボ。

- **デプロイ方式**: 素の CloudFormation（`templates/contact-form.yaml`）+ S3 配置 + **Launch Stack** ボタン
- テンプレート配信バケット: `stepfunctions-dojo2026`（`ap-northeast-1`）
- API Gateway → Step Functions 直接統合 → SES などで、Lambda レスにフォーム処理を実現

---

## セルフテスト

採点ロジックの回帰テスト（`index.html` を編集したら必ず実行）。

```bash
cd .selftest
node extract.cjs   # index.html から同梱 JSONata と問題/採点エンジンを抽出
node run.cjs       # テスト実行（失敗時 exit 1）
```

検証内容: 全 50 問の模範解答が正解判定 / 裸参照が不正解 / 浮動小数の許容誤差 / 順不同採点 / 問題データの静的検証（ID 一意・10レベル×5問・必須フィールド・予約キー）。
詳細は [.selftest/README.md](.selftest/README.md)。

---

## 公開

JSONata ノック道場アプリは GitHub リポジトリ [ukkiy8787/Step-Functions-](https://github.com/ukkiy8787/Step-Functions-) で管理。
`index.html` は単一ファイルで完結するため、GitHub Pages 等での配信のほか、ローカルで `file://` 直接オープンでも動作します。
