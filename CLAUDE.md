# CLAUDE.md - terakoya-web プロジェクト

## プロジェクト概要

埼玉県朝霞市の学習支援・不登校支援団体「寺子屋」の公式ウェブサイト。
静的サイトとしてFirebase Hostingでホスティングしている。

## 技術スタック

- HTML5 / CSS3 / JavaScript (ES6+)
- Firebase Hosting
- GitHub Actions (CI/CD)

## ディレクトリ構造

```
terakoya-web/
├── public/           # デプロイ対象（Firebase Hostingの公開ディレクトリ）
│   ├── index.html    # メインページ（HTML構造）
│   ├── style.css     # スタイルシート
│   └── *.png, *.svg  # 画像ファイル
├── development/      # 開発用データ（デプロイ対象外）
│   ├── plan_list.csv             # 活動予定リスト
│   ├── external_link_list.csv    # 外部リンク一覧
│   ├── YYYY_MM_schedule.html     # 月次Instagram投稿用テンプレート
│   └── YYYY_MM_schedule.png      # 月次Instagram投稿用画像
├── backup/           # 過去のファイルのバックアップ
└── .github/workflows/         # GitHub Actions設定
```

## 開発ルール

### ファイル編集

- HTMLの編集は `public/index.html` を直接編集する
- CSSの編集は `public/style.css` を直接編集する
- 画像ファイルは `public/` ディレクトリに配置する
- 活動予定の更新は `development/plan_list.csv` を編集する
- 過去のファイルやバックアップは `backup/` ディレクトリに配置する

### コーディング規約

- インデントはスペース2つ
- 日本語コメントを積極的に使用する
- CSSは `public/style.css` に記述する（HTMLとCSSは分離する）

### コミットメッセージ

- 日本語で簡潔に記述する
- 例: `活動予定を更新`、`SEO対策を実施`

## デプロイ

### 手動デプロイ

```bash
firebase deploy
```

### 自動デプロイ

- PRを作成するとGitHub Actionsでプレビューデプロイが実行される

## 月次タスク：活動予定の更新

毎月、活動予定を更新するときは **必ず以下3点をセットで実施** する。

### 1. 活動予定データの更新

- `development/plan_list.csv` に新しい月の予定を記載する

### 2. Webページの更新

- `public/index.html` の活動予定テーブル（`#schedule-tab` 内の `tbody`）を CSV に合わせて更新する

### 3. Instagram投稿用画像の生成

Instagram フィードに投稿するための画像を作成する。

**ファイル命名規則**

- HTML: `development/YYYY_MM_schedule.html`（例：`2026_05_schedule.html`）
- PNG:  `development/YYYY_MM_schedule.png`（例：`2026_05_schedule.png`）

**前月のテンプレートを踏襲**

- 直近の `development/YYYY_MM_schedule.html` をベースにコピーして使う
- レイアウト構造（ヘッダー → スケジュールカード → インフォボックス → フッター）は変えない
- フォントサイズ・余白・要素配置も基本的に維持する

**毎月変えるところ（飽き対策）**

季節感とバリエーションを出すため、月ごとに以下を変更する。

- **配色**：背景グラデーション、月バッジ、タイトル文字、日付バッジを月のテーマカラーに変える
- **チャーム（背景装飾の絵文字）**：その月らしい絵柄に差し替える（10個程度配置）
- **「みんな来てね！」のひとこと**：その月にちなんだ短いメッセージにする

月ごとのテーマ例：

| 月 | テーマカラー | チャーム例 |
|----|-------------|-----------|
| 1月 | 赤・金・白 | 🎍🌅🐰❄️🎌 |
| 2月 | ピンク・赤・白 | ❄️☃️💝🍫🐯 |
| 3月 | 桃色・若草色 | 🌸🎎🍡🌱🦋 |
| 4月 | 桜色・新緑 | 🌸🌷🐣🎒🌿 |
| 5月 | ピンク・クリーム・ミント | 🌸🌿🌷🍀🌼🦋 |
| 6月 | 青・紫・水色 | ☂️🐌🐸🌧️💧🐢 |
| 7月 | 青・黄 | 🎋🌊🍉🎆🐠⭐ |
| 8月 | 黄・橙・水色 | 🌻🍉🌊🍧🎐🦀 |
| 9月 | 金・橙・紫 | 🌕🍇🍂🌾🦗🐇 |
| 10月 | 橙・赤・紫 | 🍁🎃👻🍄🌰🦊 |
| 11月 | 茶・黄・赤 | 🍂🍁🌰🍄🦔🍠 |
| 12月 | 赤・緑・白 | 🎄⛄🎁🦌❄️✨ |

**生成コマンド（Windows / ヘッドレス Edge）**

```bash
"/c/Program Files (x86)/Microsoft/Edge/Application/msedge.exe" \
  --headless --disable-gpu --hide-scrollbars \
  --window-size=1080,1350 --virtual-time-budget=8000 \
  --screenshot="c:/Users/つぐとし/terakoya-web/development/YYYY_MM_schedule.png" \
  "file:///c:/Users/つぐとし/terakoya-web/development/YYYY_MM_schedule.html"
```

- 画像サイズは Instagram 縦長フィード推奨の **1080×1350px**
- Google Fonts（Mochiy Pop One / Kiwi Maru）を CDN から読み込むため `--virtual-time-budget` を指定する

## 注意事項

- `*_back.png` パターンのファイルはgit管理対象外
- `backup/` ディレクトリはgit管理対象外
- `development/` 内のCSVファイルは開発用データなのでデプロイされない
