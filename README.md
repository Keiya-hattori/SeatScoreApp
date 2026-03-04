# SeatScore 

K-POP公演の座席診断 × ヒートマップ × 共有カードを中心にしたWeb（PWA）アプリケーション。

## 特徴

- 🎯 **座席スコア診断**: 距離・正面度・段差・演出相性から総合スコアを算出
- 🔥 **リアルタイムヒートマップ**: 当日の参加者の回答から動的に更新
- 📱 **PWA対応**: ホーム画面に追加してアプリのように使用可能
- 🏆 **バッジシステム**: Tour Explorer / City Collector バッジを収集
- 📊 **5タップ投票**: 当日の演出配置を報告してヒートマップの精度向上

## 技術スタック

- **Framework**: Next.js 14 (App Router)
- **Language**: TypeScript
- **Styling**: Tailwind CSS + shadcn/ui
- **Database**: Supabase (PostgreSQL)
- **Deployment**: Vercel
- **Analytics**: PostHog
- **Rate Limiting**: Upstash Redis

## セットアップ

### 1. 依存関係のインストール

```bash
npm install
```

### 2. 環境変数の設定

`.env.local` ファイルを作成し、以下の環境変数を設定してください：

```env
NEXT_PUBLIC_SITE_URL=http://localhost:3000
NEXT_PUBLIC_POSTHOG_KEY=your_posthog_key_here
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url_here
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key_here
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key_here
UPSTASH_REDIS_REST_URL=your_upstash_url_here
UPSTASH_REDIS_REST_TOKEN=your_upstash_token_here
```

### 3. Supabaseのセットアップ

1. Supabaseプロジェクトを作成
2. SQL Editorでマイグレーションを実行:
   ```bash
   # 001_initial_schema.sql を実行
   # seed.sql を実行してサンプルデータを投入
   ```

### 4. 開発サーバーの起動

```bash
npm run dev
```

http://localhost:3000 でアプリケーションが起動します。

## プロジェクト構成

```
.
├── app/                    # App Routerのページとレイアウト
│   ├── api/               # API Routes
│   ├── artist/            # アーティストページ
│   ├── venue/             # 会場ガイド
│   ├── seat/              # 座席診断
│   ├── map/               # ヒートマップ
│   └── badge/             # バッジページ
├── components/            # Reactコンポーネント
│   ├── ui/               # shadcn/ui コンポーネント
│   ├── seat-diagnostic-ui.tsx
│   ├── heatmap-ui.tsx
│   └── ...
├── lib/                   # ビジネスロジック
│   ├── seat/             # スコアリング・ヒートマップ
│   ├── badges/           # バッジシステム
│   ├── supabase/         # Supabaseクライアント
│   └── types/            # 型定義
├── supabase/
│   ├── migrations/       # DBマイグレーション
│   └── seed.sql          # シードデータ
└── public/               # 静的ファイル
```

## 主要機能

### 座席診断

`/seat/[venue]/[event]` で座席情報を入力すると、以下を表示：

- **スコア** (0-100点)
- **上位パーセンタイル**
- **特徴タグ** (例: "ステージに近い", "正面の良い角度")
- **詳細な内訳** (距離・正面度・段差・演出相性)

### ヒートマップ

`/map/[venue]/[event]` で会場全体のヒートマップを表示：

- ブロック単位の色分け
- 信頼度インジケーター
- トップブロックランキング

### 5タップ投票

当日の演出配置を以下の5項目で報告：

1. センターステージの有無
2. 花道の長さ
3. 十字型ステージの有無
4. バックステージの有無
5. スクリーン配置

### バッジシステム

- **Tour Explorer**: 同一ツアー内の複数会場を訪問
- **City Collector**: 複数地域の会場を訪問

※ ジオフェンス認証が必要

## データモデル

### Archetype (レイアウト型)

演出のパターンを定義：

- `end_only_v1`: エンドステージのみ
- `end_thrust_short_v1`: エンドステージ + 短い花道
- `end_center_v1`: エンドステージ + センターステージ
- `center_cross_v1`: センター + 十字型ステージ
- `end_back_v1`: エンド + バックステージ

### Prior → Posterior 更新

イベント開始前は `layout_prior` (事前確率) を使用。
当日の5タップ投票により、ベイズ更新で `layout_posterior` (事後確率) に更新。

## デプロイ

### Vercel

```bash
npm run build
vercel
```

環境変数をVercelのプロジェクト設定で追加してください。

### Supabase

Supabaseプロジェクトでマイグレーションとシードを実行後、
本番環境のURLとキーを環境変数に設定。

## ライセンス

MIT

## 注意事項

### 法務・倫理

- **写真アップロード機能なし**: 演者・演出の権利保護のため
- **座席公開はブロックまで**: プライバシー保護
- **転売・価格情報は扱わない**: 体験価値に限定
- **個人最適化レコは将来機能**: MVPではオフ

### 投稿ルール

詳細は `/guidelines` を参照してください。




