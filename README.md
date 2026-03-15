# GitHub Copilot でプレゼンが変わる、世界が変わる。

**GitHub Copilot に指示するだけで、インタラクティブなプレゼンテーションが単一HTMLファイルで生成される** — そのデモと、再利用可能なインストラクションファイルです。

🔗 **ライブデモ**: [https://agreeable-glacier-02c619200.1.azurestaticapps.net](https://agreeable-glacier-02c619200.1.azurestaticapps.net)

---

## ✨ 6つのインタラクション仕掛け

| # | 仕掛け | 概要 |
|---|--------|------|
| ① | **マウスオーバー強調** | テキスト行にカーソルを乗せると拡大＋アクセント色に変化 |
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

## 🚀 作り方（Step by Step）

### Step 1: 以下をコピペして GitHub Copilot に指示

プレゼン内容を伝える＆プレゼンHTML作成指示のインストラクションファイルを Copilot に渡します。
[`presentation-transform.instructions.md`](presentation-transform.instructions.md) を参考にしつつ、GitHub Copilot（Agent モード）へ、以下のように指示します（適宜自分好みにカスタマイズしてください）：

```
# Presentation Transformation — GitHub Copilot 用インストラクション

## 🎯 ゴール

ユーザーのプレゼン内容（テーマ・スライド構成・メッセージ）を受け取り、**単一の HTML ファイル**として以下をすべて備えたインタラクティブプレゼンテーションを生成する。

## テーマ
[あなたのプレゼンテーマ]

## スライド構成
1. タイトル: [タイトル]
2. 課題提起: [問題意識]
3. デモ概要: [仕掛けの一覧]
4. 仕掛け①: [内容]
5. 仕掛け②: [内容]
6. 仕掛け③: [内容]
7. エンディング: [締めの言葉]

## GitHub Copilot にお願いしたいこと
- プレゼン内容にあわせて後述の6種類のインタラクション仕掛けを実装
- 日本語のプレゼン内容を英語に切り替えできる
- フィードバックメモ機能も実装
- 各スライドの背景にはプレゼン内容にあわせたキャンバス背景アニメーションを実装
- レスポンシブ対応（タッチスワイプ含む）

---

## 📐 アーキテクチャ

### ファイル構成
- **単一HTMLファイル**（CSS・JavaScript すべてインライン）
- 外部依存は Google Fonts のみ
- `<!DOCTYPE html>` + `<html lang="ja">`

### フォント構成（3書体）
| 用途 | フォント | 役割 |
|------|---------|------|
| 見出し・タイトル | Shippori Mincho B1 | 明朝体、格調 |
| 本文・UI | Noto Sans JP | ゴシック体、可読性 |
| 装飾数字・英字 | Cormorant Garamond | セリフ、エレガンス |

---

### 切替アニメーション
- `translateX(±60px)` + `cubic-bezier(.22,1,.36,1)`
- `busy` フラグで連打防止

### ナビゲーション
- **矢印ボタン** `←` `→` + **キーボード** `ArrowLeft` `ArrowRight` `Space`
- **タッチスワイプ**: 50px 以上の指移動で方向判定
- **ドットインジケーター** + **プログレスバー** + **スライド一覧パネル**

---

## 📑 スライドタイプの例

### 1. タイトルスライド
中央揃え。pill（バッジ）→ h1 → p。h1 内の `.it` で Cormorant Garamond イタリック装飾。

### 2. 課題提起スライド
全幅。eyebrow → h2 → カード群（2×2）→ 結論バナー。カードクリックでモーダル展開（`CARD_DATA` 駆動）。

### 3. 概要スライド
中央揃え。pill → h2 → p → グリッド（各仕掛けの概要カード：icon + 番号 + タイトル + 説明）。

### 4. 詳細紹介スライド（2カラム型）
`.ucw` グリッド（`fr 1.1fr`）。左（`.ucl`）: bignum → icon → h2 → p。右（`.uccard`）: ヘッダー → インタラクティブデモ。

### 5. エンディングスライド
中央揃え。`.big`（キャッチコピー）→ p（メッセージ）→ リンクカード。Cormorant Garamond イタリック。

---

## スライドのプレゼン内容にあわせて実装する6つのインタラクション仕掛け

### 仕掛け① マウスオーバー強調（全スライドに実装）
JS で `<br>` 区切りごとに `<span class="hline">` ラップ。ホバーで `scale(1.22)` + アクセントカラー。リスト行は拡大 + 太字。`.blue-hover` で背景がアクセントカラー + 文字もアクセントカラーに対して見やすい色に。

### 仕掛け② スポットライトモード（全スライドに実装）
`S` キーでトグル。全画面オーバーレイに `radial-gradient(circle 220px)` でマウス追従の光。バッジ表示付き。

### 仕掛け③ アニメーション演出（この演出が最適なスライドでのみ発動）
- **ライブタイピング**: チャットUI風の1文字ずつ表示。カーソル点滅 + 速度揺らぎ。
- **カウントアップ**: `requestAnimationFrame` でイーズ付きカウント。大きな Cormorant Garamond 数字。

### 仕掛け④ Before/After 比較スライダー（この演出が最適なスライドでのみ発動）
各行が独立した `.ba-comp-row`。Before レイヤーを `clip-path: inset()` で制御。縦線ハンドルをドラッグで移動。マウス・タッチ・クリック対応。リセットボタン付き。

### 仕掛け⑤ 多言語切り替え
右上トグルボタンで全スライドを瞬時に日英切替。`data-i18n` 属性 + `I18N` 辞書オブジェクト。翻訳時に `rebuildHlines()` で行ラッパーを再構築。`CARD_DATA` / `OV_DATA` も連動更新。

#### キー命名規則
``
ui.logo / ui.hint / ui.spotBadge / ui.ovTitle
s1.pill / s1.h1 / s1.p
s2.c1.title / s2.c1.body
s6.r1.before / s6.r1.after
``

### 仕掛け⑥ フィードバックメモ
右上 📝 ボタンでサイドパネルを開閉。スライド番号 + タイムスタンプ自動付与。テキストダウンロードで GitHub Copilot に直接修正指示を渡せる。メモ入力中は矢印キー無効。PC ではパネル開閉時にスライドが自動リサイズ。

---

## 🎨 背景キャンバスアニメーション
各スライドの内容にあわせて生成。
---

## 📦 モーダルシステム

- `backdrop-filter: blur(8px)` の背景
- `scale(.92) → scale(1)` でポップイン
- `←` `→` キーでページ送り、`Escape` で閉じる
- `CARD_DATA` 配列でデータ駆動

---

## 📱 レスポンシブ対応

- `@media(max-width:700px)` でグリッドを1カラム化
- `clamp()` でフォントが流動的にスケール
- タッチスワイプ対応
- 右上ボタンは28pxに縮小して再配置

---

## ⚠️ 注意事項

- スライド数変更時 → ドット生成・`OV_DATA`・`animMap` をすべて更新
- `.ucw` グリッド内のワイドコンテンツ → グリッド外に出すことを検討
- 絵文字は UTF-8 保存（サロゲートペア注意）
- ホバー拡大カード内 → `.hline:hover { transform:none }` ではみ出し防止
- SPA前提 → `html, body { overflow: hidden }`
- 多言語切替後 → `rebuildHlines()` 必須

---

### カラーシステム（CSS Custom Properties） ※好みに合わせて変えてください。適藤におまかせ！でもよいです。
```css
:root {
  --bg: #faf7f3;       /* 背景：温かみのあるオフホワイト */
  --white: #ffffff;     /* カード背景 */
  --ink: #251a11;       /* テキスト：ダークブラウン */
  --terra: #c95f2a;     /* アクセント：テラコッタオレンジ */
  --terra-l: #f2d4c0;   /* アクセント薄 */
  --terra-d: #8c3d18;   /* アクセント濃 */
  --sage: #5a8a6e;      /* セージグリーン（補助色） */
  --muted: #9a8a7c;     /* 補助テキスト */
  --border: #e8ddd3;    /* ボーダー */
  --shadow: rgba(37,26,17,.08);
}
``
> カラーはテーマに合わせて差し替え可。9変数すべてを定義すること。ユーザーから指示がなければCopilot側で最適な配色を定義。

---

## 🏗️ スライド構造

### ステージ
```html
<div id="stage">
  <div class="slide active" id="s1">...</div>
  <div class="slide" id="s2">...</div>
  <!-- position:absolute で重ねる -->
</div>
``

### 切替アニメーション
- `translateX(±60px)` + `cubic-bezier(.22,1,.36,1)`
- `busy` フラグで連打防止

### ナビゲーション
- **矢印ボタン** `←` `→` + **キーボード** `ArrowLeft` `ArrowRight` `Space`
- **タッチスワイプ**: 50px 以上の指移動で方向判定
- **ドットインジケーター** + **プログレスバー** + **スライド一覧パネル**

--

```
### Step 2: GitHub Copilot と対話しながらお好みのように修正

### Step 3: GitHub で Create a new repository & Azure上 にデプロイ

| 方式 | リポジトリ |
|------|-----------|
| 🔓 認証なし（全公開） | [azure-no-auth-demo](https://github.com/FukudaRikako/azure-no-auth-demo) |
| 🔐 Entra ID 認証付き | [azure-entra-auth-demo](https://github.com/FukudaRikako/azure-entra-auth-demo) |

---

## 📁 ファイル構成

```
├── index.html                                  # プレゼンテーション本体（10スライド）
├── copilot-transforms-presentations/
│   └── presentation-transform.instructions.md  # Copilot 用インストラクション
├── staticwebapp.config.json                    # Azure SWA 設定（認証用）
└── .github/workflows/                          # GitHub Actions デプロイ設定
```

---

## 📄 ライセンス

MIT

```
