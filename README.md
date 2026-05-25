# 杉並区 葬儀LP（むさしの葬祭）— フラット構成版

杉並区向け葬儀ランディングページの **`index.html` / `style.css` / `main.js` フラット分離版** です。ルート直下に3ファイルが並ぶ最小構成で、Codex での編集および GitHub Pages / Cloudflare Pages / Netlify 等への即時デプロイに最適化しています。

ビルドツールもパッケージマネージャもフレームワークも不要で、`index.html` をブラウザで直接開けば動作します。

---

## クイックスタート

```bash
# このディレクトリで
python3 -m http.server 8080
# ブラウザで http://localhost:8080/ を開く
```

VS Code の「Live Server」拡張機能でも、`index.html` を右クリック → Open with Live Server で動作します。

---

## フォルダ構成

```
suginami-lp-flat/
├── index.html      ← エントリーポイント。全セクションをHTMLコメントで明示分割
├── style.css       ← 全スタイル（リセット + デザイントークン + コンポーネント）
├── main.js         ← FAQアコーディオン、スクロールリビール、年表示の更新
├── README.md       ← このファイル
├── .gitignore      ← GitHub向け（OS / IDE関連を除外）
└── images/         ← 全画像（webp 形式）
    ├── hero-flower-altar.webp   Hero の花祭壇
    ├── lily-symbol.webp         装飾用の白百合
    ├── staff-warmth.webp        共感セクションのスタッフ手元
    ├── altar-overview.webp      花祭壇の全景
    ├── horinouchi-area.webp     堀ノ内斎場周辺
    └── florist-hands.webp       花職人の手（予備）
```

`index.html` の内部参照パスは下記のとおりルート直下を指しています。

```html
<link rel="stylesheet" href="./style.css" />
<script src="./main.js" defer></script>
<img src="./images/hero-flower-altar.webp" ... />
```

---

## セクション分割

`index.html` は以下のセクションを **HTML コメントブロック** で明示的に区切っています。Codex でセクション単位の編集を行うときは、この見出しを目印にしてください。

| セクションコメント | 内容 |
|---|---|
| `SECTION: HEADER` | ロゴ、追従ヘッダーの電話CTA |
| `SECTION: HERO` | キャッチコピー、不安解消バー、主CTA／副CTA、価格バッジ |
| `SECTION: EMPATHY` | 共感ストーリー（病院から急かされる遺族へのメッセージ） |
| `SECTION: REASONS` | 選ばれる4つの理由 + 自社生花の専用ブロック |
| `SECTION: PLANS` | 火葬式・一日葬・家族葬の3プラン + 含む／含まない／追加条件 |
| `SECTION: LOCAL` | 堀ノ内斎場対応・対応エリアタグ |
| `SECTION: STEPS` | お電話から葬儀後サポートまで5ステップ |
| `SECTION: VOICES` | 杉並区在住者のお客様の声 |
| `SECTION: FAQ` | 6問のアコーディオン式FAQ |
| `SECTION: FINAL CTA` | ページ最終の電話CTA |
| `SECTION: FOOTER` | 会社情報、関連リンク |
| `STICKY MOBILE CTA` | 画面下部固定の電話／資料請求CTA |

---

## SEO 編集ガイド

メタ情報は `index.html` の `<head>` 内の **`META - SEO / Social`** コメントブロックに集約しています。修正対象は以下のとおりです。

```html
<title>...</title>
<meta name="description" content="..." />
<meta name="keywords" content="..." />
<link rel="canonical" href="..." />
<meta property="og:title" content="..." />
<meta property="og:description" content="..." />
<meta property="og:image" content="..." />
<meta name="twitter:title" content="..." />
<meta name="twitter:description" content="..." />
<meta name="twitter:image" content="..." />
```

さらに、ページ末尾近くに JSON-LD（`@type: FuneralHome`）構造化データを埋め込んでいます。会社情報・電話番号を変更する場合はあわせて更新してください。

---

## Tailwind について

このバージョンでは **Tailwind CSS は使用していません**。元のReact版ではTailwindで実装していましたが、Codex / 静的サイトでの取り回しを優先し、すべてのスタイルを純粋なCSS（`style.css`）に変換しています。CSS変数（`:root`内のデザイントークン）とBEMライクなコンポーネントクラスで、Tailwindと同等の保守性を確保しています。

---

## デザイントークン

`style.css` 冒頭の `:root` ブロックにすべて集約されています。配色やフォントの変更は基本的にここだけで完結します。

```css
:root {
  --color-paper:        #FAF7F2;  /* ベース */
  --color-cream:        #EFE8DC;  /* セカンダリ背景 */
  --color-ink:          #1B3A57;  /* メインテキスト（濃紺） */
  --color-forest-deep:  #2F6B4F;  /* CTAアクセント（深緑） */
  --color-gold:         #C49A2C;  /* 補助アクセント（山吹） */
  --color-sakura:       #E8C5C0;  /* 割引バッジ（桜淡ピンク） */
  --font-serif:         "Noto Serif JP", ...;
  --font-sans:          "Noto Sans JP", ...;
  --font-numeric:       "Cormorant Garamond", ...;
}
```

---

## レスポンシブ対応

スマホファースト設計で、以下のブレイクポイントでレイアウトが切り替わります。

| 幅 | 主な変化 |
|---|---|
| ～639px | 単一カラム。下部固定の電話／資料CTA表示 |
| 640px～ | Hero CTA が横並び化 |
| 768px～ | プラン3列、ステップ5列、お客様の声3列に展開 |
| 1024px～ | 主要セクションが2カラムの非対称グリッドに展開。下部固定CTA非表示、ヘッダー右の常駐電話CTA表示 |

---

## デプロイ

ビルド不要のため、以下のいずれの方法でもそのまま公開できます。

| 方法 | 設定 |
|---|---|
| GitHub Pages | Repository Settings → Pages → Branch: main / Folder: `/ (root)` |
| Cloudflare Pages | Build command: 空欄 / Output directory: `/` |
| Vercel / Netlify | Build command: 空欄 / Publish directory: `/` |
| 自社サーバ | ディレクトリ全体をWebルートにアップロード |

### GitHub への初回アップロード

```bash
cd suginami-lp-flat
git init
git add .
git commit -m "Initial commit: 杉並区葬儀LP フラット構成版"
git branch -M main
git remote add origin https://github.com/<your-account>/<repo-name>.git
git push -u origin main
```

---

## 主な機能

| 機能 | 実装場所 | 説明 |
|---|---|---|
| FAQアコーディオン | `main.js` の `initFaq()` | クリックで開閉。初期は最初の質問のみ展開 |
| スクロールリビール | `main.js` の `initReveal()` | `data-reveal` 要素を画面進入で `reveal` クラス付与 |
| CTAパルス | `style.css` の `.cta-pulse` | 電話CTAが2.6秒周期で呼吸するように脈動 |
| 年表示の自動更新 | `main.js` の `initYear()` | フッターの `#current-year` を実行時の年に更新 |

---

## Codex 編集の参考

1. **画像差し替え**: `images/` の同名ファイルを置き換えるだけで反映。
2. **コピー修正**: 対応する `SECTION:` コメントブロックを目印に編集。
3. **色変更**: `style.css` の `:root` 変数のみ編集。
4. **計測タグ**: `<head>` 末尾に GA4 / Microsoft Clarity / GTM のスニペットを追加。
5. **新規ランディング派生**: `index.html` を `suginami-kawakita.html` 等に複製し、Hero と FAQ のコピーだけ差し替えると、流入経路別の最適化LPを増やせます。

---

## クレジット

- Webフォント: Google Fonts（Noto Serif JP / Noto Sans JP / Cormorant Garamond）。
- 画像は AI 生成素材を含みます。実運用時には自社撮影写真への差し替えを推奨します。
