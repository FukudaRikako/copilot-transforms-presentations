# GitHub Copilotでプレゼンが変わる、世界が変わる。

GitHub Copilot を活用したインタラクティブなプレゼンテーションデモです。  
単一の HTML ファイルで動作し、外部依存なしにブラウザだけで閲覧できます。

## デモ

🌐 https://happy-forest-0c9392700.2.azurestaticapps.net

## 特徴

- **単一 HTML ファイル** — CSS・JavaScript すべてインライン、ビルド不要
- **6種類のインタラクション** — カードモーダル、タイムライン、ドラッグ＆ドロップなど
- **多言語切り替え（JA / EN）** — ワンクリックで日英を切替
- **キャンバス背景アニメーション** — パーティクルによる動的な背景演出
- **レスポンシブ対応** — デスクトップ・タブレット・モバイル対応、タッチスワイプ対応
- **キーボードナビゲーション** — 矢印キー・スペースキーでスライド操作

## ファイル構成

| ファイル | 説明 |
|---------|------|
| `index.html` | プレゼンテーション本体 |
| `presentation-transform.instructions.md` | GitHub Copilot 用インストラクション（任意の内容からプレゼンHTMLを生成するためのガイド） |

## 使い方

### そのまま閲覧する

`index.html` をブラウザで開くだけで動作します。

### 自分のプレゼンを作る

1. `presentation-transform.instructions.md` を GitHub Copilot に読み込ませる
2. 自分のプレゼン内容（テーマ・スライド構成・メッセージ）を伝える
3. Copilot が単一 HTML ファイルとしてプレゼンテーションを生成

## デプロイ

Azure Static Web Apps にデプロイされています。  
`main` ブランチへの push で GitHub Actions により自動デプロイされます。
