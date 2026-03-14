# GitHub Copilot でプレゼンが変わる、世界が変わる。

**GitHub Copilot に指示するだけで、インタラクティブなプレゼンテーションが単一HTMLファイルで生成される** — そのデモと、再利用可能なインストラクションファイルです。

🔗 **ライブデモ**: [https://happy-forest-0c9392700.2.azurestaticapps.net](https://happy-forest-0c9392700.2.azurestaticapps.net)

---

## ✨ 6つのインタラクション仕掛け

| # | 仕掛け | 概要 |
|---|--------|------|
| ① | **マウスオーバー強調** | テキスト行にカーソルを乗せると拡大＋テラコッタ色に変化 |
| ② | **スポットライトモード** | `S` キーで画面を暗転、マウス追従のスポットライトで注目箇所を照らす |
| ③ | **アニメーション演出** | ライブタイピング＋カウントアップで数値をダイナミックに表示 |
| ④ | **Before/After 比較スライダー** | ドラッグで従来と新しいスタイルをリアルタイム比較 |
| ⑤ | **多言語切り替え** | 右上トグルで日本語⇔英語を瞬時に切替 |
| ⑥ | **フィードバックメモ** | スライドごとにメモを記録、テキストでダウンロードして Copilot への修正指示に |

---

## 📐 技術スタック

- **単一HTMLファイル**（CSS・JavaScript すべてインライン、ビルド不要）
- 外部依存は **Google Fonts のみ**（Shippori Mincho B1 / Noto Sans JP / Cormorant Garamond）
- **Canvas 背景アニメーション**（stars / rip / flow / burst の4モード）
- **レスポンシブ対応**（タッチスワイプ + モバイル最適化）
- デプロイ先: **Azure Static Web Apps**（Free プラン）

---

## 🚀 自分のプレゼンを作る

### Step 1: インストラクションファイルを Copilot に渡す

[`presentation-transform.instructions.md`](presentation-transform.instructions.md) を GitHub Copilot（Agent モード）にコピペし、以下のように指示します：

```
このinstructionファイルの仕様に従って、以下の内容で
インタラクティブなプレゼンテーション HTML を生成してください。

## テーマ
[あなたのテーマ]

## スライド構成
1. タイトル: [タイトル]
2. 課題提起: [問題意識]
3. デモ概要: [仕掛けの一覧]
4〜N. 仕掛け: [各スライドの内容]
N+1. エンディング: [締めの言葉]

## カラーテーマ
[テラコッタ / ブルー / パープル など]
```

### Step 2: Azure にデプロイ

| 方式 | リポジトリ |
|------|-----------|
| 🔓 認証なし（全公開） | [azure-no-auth-demo](https://github.com/FukudaRikako/azure-no-auth-demo) |
| 🔐 Entra ID 認証付き | [azure-entra-auth-demo](https://github.com/FukudaRikako/azure-entra-auth-demo) |

---

## 📁 ファイル構成

```
├── index.html                              # プレゼンテーション本体
├── presentation-transform.instructions.md  # Copilot 用インストラクション
└── .github/workflows/                      # GitHub Actions デプロイ設定
```

---

## 📄 ライセンス

MIT
