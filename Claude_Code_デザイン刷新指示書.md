# Claude Code 指示書: いるまウェブ制作所サイトのデザイン全面刷新

> **使い方**: 既存のAstroプロジェクトのルートディレクトリで `claude` を起動し、以下の内容を貼り付けて実行依頼してください。
> **重要**: モックアップHTML(`トップページデザイン_モックアップ.html`)も同じディレクトリに配置し、「mockup.htmlを視覚的リファレンスとして読み込んでください」と伝えてください。

---

## 0. ミッション

既存のコーポレートサイト( https://iruma-web-studio.vercel.app/ ) のデザインを全面刷新します。**コンテンツ・ページ構成は維持**したまま、配色・フォント・コンポーネントの見た目を統一感のある新デザインに置き換えてください。

### 現状の問題点
- 濃紺#1F4E78とオレンジ#F59E0Bの2大原色併用で目がチカチカする
- 絵文字(🔍📱💰🛡️🔧🍽️✂️🏥🏗️🚗📚🦷)を多用してスタイルがバラバラ
- 全体的に視覚的な情報密度が高く、洗練感に欠ける

### 新デザインのコンセプト
- **kintone(cybozu)風のシングルアクセント配色** — オレンジを唯一の主役に
- **明朝体 × ゴシックの和文タイポグラフィ** — 洗練+地元の趣を両立
- **絵文字を全廃しSVGアイコンに統一** — 一貫性とプロフェッショナル感
- **クリーム基調の温かみ** — 純白より柔らかく、目に優しい
- **余白を増やしたミニマルレイアウト** — 情報密度を下げて読みやすく

---

## 1. ビジュアルリファレンス

このプロジェクトと同じディレクトリに配置している **`トップページデザイン_モックアップ.html`** を最初に読み込み、視覚的な完成イメージを把握してください。このモックアップHTMLには以下が含まれています:
- 完成版ヘッダーとヒーローセクション
- 「こんなお悩み、ありませんか?」セクションの新デザイン
- 使用カラーパレット8色の表示
- 全SVGアイコンのインライン定義
- アニメーション・装飾の実装例

**実装はこのモックアップを忠実に再現することを目指してください。**

---

## 2. デザインシステム仕様

### 2.1 カラートークン (CSS変数)

`src/styles/global.css` または相当の場所で以下のカラートークンを定義してください:

```css
:root {
  /* ===== Primary (Orange) ===== */
  --color-orange: #E97132;        /* 主役オレンジ — CTA、ロゴ、見出しアクセント */
  --color-orange-deep: #C95A1F;   /* 濃いめ — ホバー、強調 */
  --color-orange-light: #FFF1E6;  /* 淡色 — 軽い背景、ボタンHover下地 */
  --color-orange-tint: #FDE5D2;   /* マーカー風アンダーライン用 */

  /* ===== Neutral (Cream / Charcoal) ===== */
  --color-cream: #FAF7F2;         /* メイン背景 */
  --color-cream-deep: #F4EFE6;    /* セクション区切り */
  --color-white: #FFFFFF;         /* カード、コンテンツエリア */
  --color-charcoal: #2A2520;      /* 見出し(温かい黒) */
  --color-gray: #6B6259;          /* 本文 */
  --color-gray-light: #E8E2DA;    /* ボーダー、区切り線 */

  /* ===== Sub Accent (used sparingly) ===== */
  --color-sage: #6B8175;          /* セージ — 信頼・安全感のアイコンに極控えめに */
}
```

**重要**: 既存の濃紺(#1F4E78)・明るい青系・既存オレンジ(#F59E0B)はすべて削除し、上記新パレットのみで構成してください。

### 2.2 タイポグラフィ

Google Fontsから以下を読み込み:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Shippori+Mincho:wght@500;700;800&family=Noto+Sans+JP:wght@300;400;500;600;700&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
```

フォントスタック:
```css
:root {
  --font-serif: 'Shippori Mincho', 'YuMincho', 'Hiragino Mincho ProN', serif;
  --font-sans: 'Noto Sans JP', 'Hiragino Sans', 'Yu Gothic UI', sans-serif;
  --font-en: 'Inter', sans-serif;
}

body { font-family: var(--font-sans); font-weight: 400; line-height: 1.7; }
.serif, h1, h2, h3 { font-family: var(--font-serif); }
.en-label { font-family: var(--font-en); letter-spacing: 0.18em; }
```

**タイポグラフィルール**:
- **見出し(h1, h2, h3)**: Shippori Mincho 700 — 和文の趣、洗練、地元感
- **本文(p, li)**: Noto Sans JP 400 — 読みやすさ
- **英語ラベル(SECTION HEADERS, "SE × DESIGNER × AI"等)**: Inter 500-600、letter-spacing 0.18em〜0.25em、大文字
- **数字・価格表示**: Inter 600 — 視認性が高い

### 2.3 タイポグラフィスケール

```css
/* ヒーロー見出し: clamp で可変 */
h1.hero-title { font-size: clamp(40px, 4.8vw, 64px); line-height: 1.25; letter-spacing: 0.01em; }

/* セクションタイトル */
.section-title { font-size: 38px; line-height: 1.4; }

/* カード見出し */
.card-title { font-size: 19px; line-height: 1.4; }

/* 本文 */
body { font-size: 16px; }
.subtitle { font-size: 17px; line-height: 1.9; }
.small { font-size: 14px; }
.eyebrow-label { font-size: 12-13px; letter-spacing: 0.18-0.25em; }
```

### 2.4 ボーダー半径(Border Radius)

```css
:root {
  --radius-sm: 8px;     /* 小さい要素 */
  --radius-md: 16px;    /* アイコン背景 */
  --radius-lg: 20px;    /* カード */
  --radius-pill: 999px; /* ボタン、バッジ */
}
```

ボタンとバッジは**必ず pill 形状(999px)**を使用。カードは20px。

### 2.5 スペーシング

セクション間: 80px〜100px
セクション内パディング(横): 80px (デスクトップ) / 32px (モバイル)
カード間ギャップ: 28px
要素間: 8/16/24/40px のリズムで

### 2.6 シャドウ

```css
:root {
  --shadow-sm: 0 4px 12px rgba(233, 113, 50, 0.15);  /* オレンジボタン */
  --shadow-md: 0 4px 16px rgba(233, 113, 50, 0.2);
  --shadow-lg: 0 8px 24px rgba(233, 113, 50, 0.3);   /* オレンジボタンHover */
  --shadow-card: 0 12px 32px rgba(42, 37, 32, 0.06); /* カードHover */
}
```

### 2.7 トランジション

すべてのインタラクティブ要素に統一トランジション:
```css
transition: all 0.25s ease;
```

---

## 3. コンポーネント別の具体仕様

### 3.1 Header (`src/components/Header.astro`)

```
- 高さ: 約 70px (パディング 18px 48px)
- 背景: rgba(250, 247, 242, 0.85) + backdrop-filter: blur(10px)
- 下ボーダー: 1px solid var(--color-gray-light)
- position: sticky; top: 0;
- ロゴ: 36px の円形SVG(オレンジ背景に白の"W"風マーク) + "いるまウェブ制作所"(Shippori Mincho 700, 20px)
- ナビゲーション: 6項目を 38px gap で並べる、フォント14px Medium
- 最右の「無料相談」ボタンはオレンジ pill ボタン(10px 20px パディング、13px)
- ホバー時: ナビ文字色がオレンジに、ボタンは2px浮く
```

**ロゴSVG**(参考):
```html
<svg viewBox="0 0 36 36">
  <circle cx="18" cy="18" r="16" fill="#E97132"/>
  <path d="M11 14 L11 22 M14 22 L14 14 L19 22 L19 14 M22 14 L25 22 L28 14"
        stroke="white" stroke-width="2" fill="none"
        stroke-linecap="round" stroke-linejoin="round"/>
</svg>
```

### 3.2 Hero (`src/components/Hero.astro` または `src/pages/index.astro`)

**重要**: モックアップHTMLのヒーローセクションを完全再現してください。

```
レイアウト:
- 高さ: min-height: 88vh
- パディング: 100px 80px 80px
- グリッド: 1.15fr 1fr (左:右)
- ギャップ: 60px
- 背景: var(--color-cream)

装飾:
- 右上に大きな放射状グラデーション(光るオレンジ淡色) — radial-gradient(circle, #FFF1E6 0%, transparent 70%) 600x600px
- 全体に微細なグレインテクスチャ(SVG noise filter, opacity 0.6)

左側コンテンツ:
1. eyebrow バッジ:
   - 背景 var(--color-orange-light)
   - パディング 8px 18px、border-radius 999px
   - 左に小さなオレンジ円(6px) + "SE × デザイナー × 生成AI"
   - フォント Inter 13px、letter-spacing 0.18em、色 var(--color-orange-deep)

2. メイン見出し(Shippori Mincho 700, clamp(40px, 4.8vw, 64px)):
   "地域の[小さな会社]に、確かなホームページを。"
   - "小さな会社" の部分にオレンジマーカー風アンダーライン
     (擬似要素 ::after で position: absolute; bottom: 4px; height: 8px; background: #FDE5D2; z-index: -1)

3. サブタイトル(Noto Sans JP 17px、line-height 1.9、color var(--color-gray)):
   "<strong>5万円から始められる</strong>、入間の地元密着Web制作。<br>安く・速く・安心の、ワンストップサービスです。"

4. CTAグループ (gap: 16px):
   - Primary ボタン: "料金プランを見る →" — オレンジ塗り、白文字、shadow-md
   - Secondary ボタン: "サンプルを見る" — 透明背景、charcoal文字、charcoalボーダー

5. メタ情報(13px、color var(--color-gray)、gap: 24px):
   - "✓ 無料相談・お見積もり"
   - "✓ 最短7日で公開"
   - "✓ すべて税抜価格"
   - チェックマークの色は var(--color-sage)

右側イラスト:
- 480x480 の SVG (モックアップHTMLの SVG コードをそのまま使用)
- SE × Designer × AI の3つの円が重なるベン図風
- 中央に白円の中に「いるまウェブ制作所」の縦書きロゴ
- 周囲に "SE", "DESIGNER", "GENERATIVE AI" の英語ラベル
- 浮遊する装飾円(deco-1, deco-2, deco-3)が float アニメーション(6-8s)
```

**SVGイラストのコード**は `トップページデザイン_モックアップ.html` 内の `<div class="hero-illustration">` 配下を参照し、そのまま流用してください。

### 3.3 ボタン (`src/components/Button.astro`)

**Primary ボタン**:
```css
background: var(--color-orange);
color: var(--color-white);
padding: 16px 32px;
border-radius: 999px;
font-weight: 600;
font-size: 15px;
box-shadow: 0 4px 16px rgba(233, 113, 50, 0.2);

/* hover */
&:hover {
  background: var(--color-orange-deep);
  transform: translateY(-2px);
  box-shadow: 0 8px 24px rgba(233, 113, 50, 0.3);
}
```

**Secondary ボタン**:
```css
background: transparent;
color: var(--color-charcoal);
border: 1.5px solid var(--color-charcoal);
padding: 16px 32px;
border-radius: 999px;

/* hover */
&:hover {
  background: var(--color-charcoal);
  color: var(--color-white);
}
```

ボタン内の矢印 `→` には class="arrow" を付与し、ホバー時に4pxスライド:
```css
.btn .arrow { transition: transform 0.25s ease; }
.btn:hover .arrow { transform: translateX(4px); }
```

### 3.4 セクションラベル (英文小見出し)

各セクションの先頭に配置する小さな英文ラベル:
```html
<div class="section-label">
  <span class="section-label-line"></span>
  <span class="section-label-text">YOUR CONCERNS</span>
</div>
```

```css
.section-label { display: flex; align-items: center; gap: 14px; margin-bottom: 16px; }
.section-label-line { width: 40px; height: 1px; background: var(--color-orange); }
.section-label-text {
  font-family: var(--font-en);
  font-size: 12px; font-weight: 600;
  color: var(--color-orange);
  letter-spacing: 0.25em;
  text-transform: uppercase;
}
```

各セクションのラベル例:
- お悩みセクション: `YOUR CONCERNS`
- 解決策セクション: `OUR SOLUTIONS`
- 業種サンプル: `INDUSTRY SAMPLES`
- 料金プラン: `PRICING`
- お客様の声: `TESTIMONIALS`
- FAQ: `FREQUENTLY ASKED`
- CTA: `GET STARTED`

### 3.5 Card コンポーネント (Pain Card / Pricing Card / Sample Card)

**共通スタイル**:
```css
.card {
  background: var(--color-cream);  /* または var(--color-white) for emphasis */
  padding: 40px 32px;
  border-radius: 20px;
  border: 1px solid transparent;
  transition: all 0.3s;
}
.card:hover {
  background: var(--color-white);
  border-color: var(--color-gray-light);
  transform: translateY(-4px);
  box-shadow: 0 12px 32px rgba(42, 37, 32, 0.06);
}
```

**カード内アイコン**:
```css
.card-icon {
  width: 56px;
  height: 56px;
  background: var(--color-orange-light);
  border-radius: 16px;
  display: flex; align-items: center; justify-content: center;
  margin-bottom: 24px;
  transition: all 0.3s;
}
.card:hover .card-icon { background: var(--color-orange); }
.card:hover .card-icon svg path,
.card:hover .card-icon svg circle,
.card:hover .card-icon svg line { stroke: var(--color-white); }
```

**カード見出し**: Shippori Mincho 700, 19px
**カード本文**: Noto Sans JP, 14px, line-height 1.85, color var(--color-gray)

### 3.6 Pricing Card の特別扱い

3プランカードのうち中央の Standard プランは**強調**:
- スケールを 1.05 倍程度に
- 「人気No.1」リボンを上部にオレンジ背景・白文字で配置
- ボーダーを var(--color-orange) で 2px

### 3.7 Footer (`src/components/Footer.astro`)

```
- 背景: var(--color-charcoal)
- 文字色: var(--color-cream)
- 構成: 4カラム(会社情報、ページリンク、業種サンプル、SNS) + 下部コピーライト
- 文字色のアクセント: ホバー時にオレンジ(var(--color-orange))に
- パディング: 80px 上下、48px 左右
```

---

## 4. 絵文字 → SVGアイコン置き換えマップ

**絶対要件**: 全ページから絵文字をすべて排除し、以下のSVGアイコンに置き換えてください。すべて 24x24 viewBox で stroke="currentColor" stroke-width="1.8" を基本とし、`var(--color-orange)` で表示します。

| 旧絵文字 | 用途 | SVGコード |
|---|---|---|
| 🔍 | 「見つけてもらえない」 | `<svg viewBox="0 0 24 24" fill="none"><circle cx="11" cy="11" r="7" stroke="currentColor" stroke-width="1.8"/><path d="M21 21L16 16" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg>` |
| 📱 | 「Webから来ない」 | `<svg viewBox="0 0 24 24" fill="none"><rect x="6" y="3" width="12" height="18" rx="2" stroke="currentColor" stroke-width="1.8"/><line x1="11" y1="18" x2="13" y2="18" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg>` |
| 💰 | 「高すぎて断念」「安い」 | `<svg viewBox="0 0 24 24" fill="none"><path d="M12 2V22" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/><path d="M16 6H10C8.5 6 7.5 7 7.5 8.5C7.5 10 8.5 11 10 11H14C15.5 11 16.5 12 16.5 13.5C16.5 15 15.5 16 14 16H7" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>` |
| 🛡️ | 「高品質・セキュア」 | `<svg viewBox="0 0 24 24" fill="none"><path d="M12 2L3 6V12C3 17 6.5 21 12 22C17.5 21 21 17 21 12V6L12 2Z" stroke="currentColor" stroke-width="1.8" stroke-linejoin="round"/><path d="M9 12L11 14L15 10" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>` |
| 🔧 | 「制作後も安心」 | `<svg viewBox="0 0 24 24" fill="none"><path d="M14 6L17 3L21 7L18 10M14 6L4 16V20H8L18 10M14 6L18 10" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>` |
| 🍽️ | 飲食店 | `<svg viewBox="0 0 24 24" fill="none"><path d="M3 2V8C3 9 4 10 5 10V22M9 2V8C9 9 8 10 7 10M5 10V2" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/><path d="M19 2C16 2 14 5 14 9C14 11 15 12 16 13L17 22M19 2V13" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>` |
| ✂️ | 美容室 | `<svg viewBox="0 0 24 24" fill="none"><circle cx="6" cy="6" r="3" stroke="currentColor" stroke-width="1.8"/><circle cx="6" cy="18" r="3" stroke="currentColor" stroke-width="1.8"/><line x1="20" y1="4" x2="8.12" y2="15.88" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/><line x1="14.47" y1="14.48" x2="20" y2="20" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/><line x1="8.12" y1="8.12" x2="12" y2="12" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg>` |
| 🏥 | 整体・接骨院 | `<svg viewBox="0 0 24 24" fill="none"><path d="M9 12H15M12 9V15M5 22V8L12 3L19 8V22M5 22H19M5 22H3M19 22H21" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>` |
| 🏗️ | 工務店 | `<svg viewBox="0 0 24 24" fill="none"><path d="M3 21H21M5 21V10L12 5L19 10V21M9 21V14H15V21" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>` |
| 🚗 | 自動車整備 | `<svg viewBox="0 0 24 24" fill="none"><path d="M5 17H3V12L6 7H18L21 12V17H19M5 17V19H8V17M5 17H19M19 17V19H16V17" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/><circle cx="7.5" cy="14" r="1.5" fill="currentColor"/><circle cx="16.5" cy="14" r="1.5" fill="currentColor"/></svg>` |
| 📚 | 学習塾 | `<svg viewBox="0 0 24 24" fill="none"><path d="M4 19.5C4 18 5 17 6.5 17H20V3H6.5C5 3 4 4 4 5.5V19.5ZM4 19.5C4 21 5 22 6.5 22H20V20" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/><line x1="8" y1="7" x2="16" y2="7" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/><line x1="8" y1="11" x2="16" y2="11" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg>` |
| 🦷 | 歯科医院 | `<svg viewBox="0 0 24 24" fill="none"><path d="M12 22C10 22 9 18 9 14C9 12 8 10 6 10C5 10 4 9 4 7C4 4 7 2 12 2C17 2 20 4 20 7C20 9 19 10 18 10C16 10 15 12 15 14C15 18 14 22 12 22Z" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>` |
| ⭐ (お客様の声の星) | 評価表示 | `<svg viewBox="0 0 24 24" fill="currentColor"><path d="M12 2L15.09 8.26L22 9.27L17 14.14L18.18 21.02L12 17.77L5.82 21.02L7 14.14L2 9.27L8.91 8.26L12 2Z"/></svg>` (オレンジで塗り) |
| ✓ (チェックマーク) | リスト、メタ情報 | `<svg viewBox="0 0 24 24" fill="none"><path d="M5 12L10 17L20 7" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/></svg>` |

### アイコン使用方針
- 業種アイコン7種は **Sample Card** 内で使用、サイズ 28x28、色 var(--color-orange)
- 解決策3つ(💰🛡️🔧) は **Solution Card** 内で使用、サイズ 28x28、オレンジ淡色背景の上に配置
- お悩み3つ(🔍📱💰) は **Pain Card** 内で使用、同上
- ⭐ は **Testimonial** で5個並べる、サイズ 16x16、オレンジ塗り
- ✓ は メタ情報・FAQ・プラン特徴リストで使用、サイズ 16x16、色 var(--color-sage)

---

## 5. ページ別の具体的な変更内容

### 5.1 トップページ (`src/pages/index.astro`)

セクション構成は維持しますが、各セクションを新デザインに置き換え:

1. **ヒーロー** → 完全置き換え(モックアップHTML参照)
2. **「こんなお悩み」** → 新デザイン Pain Card 3つ + オレンジ線アクセントの section-label
3. **「3つの強み」** → 新デザイン Solution Card 3つ + 番号バッジ(①②③)を Shippori Mincho で大きく
4. **「業種別サンプル」** → 新デザイン Sample Card 7つ + SVGアイコン
5. **「料金プラン」** → 新デザイン Pricing Card 3つ(中央Standard強調)
6. **「お客様の声」** → 新デザイン Testimonial カード(イニシャル円のbackground-colorはランダムでオレンジ系統(var(--color-orange-light), var(--color-orange-tint))を使う)
7. **「FAQ」** → アコーディオン形式、ボーダーは var(--color-gray-light)、ホバー時にオレンジ
8. **「フッター直前 CTA」** → クリーム背景に Shippori Mincho 大見出し + Primaryボタン
9. **フッター** → 新デザイン

### 5.2 サービス詳細(`src/pages/service.astro`)

- 制作プロセス 5ステップを縦タイムラインで表現(各ステップは丸番号 + 説明、左に縦線)
- 「技術と品質の担保」セクションは**チェックリストカード**(2カラム)
- 「生成AIをどう使うのか」は引用ブロック風のデザイン(オレンジ縦線で区切る)

### 5.3 料金プラン(`src/pages/pricing.astro`)

- 3プランカードを大きく表示(中央 Standard を強調)
- 保守プラン3つは別途表で見やすく
- 料金FAQ は新FAQデザインで

### 5.4 サンプル一覧(`src/pages/samples/index.astro`)

- 7業種を 3カラム × 3行 のグリッドで(モバイルは1カラム)
- 各カードにSVG業種アイコン + 業種名(Shippori Mincho) + 短い説明 + 「サンプルを見る →」

### 5.5 業種別サンプル(`src/pages/samples/[業種]/index.astro`)

各業種サンプルは **業種固有のサブカラー** を採用しつつ、**全体のフレーム(ヘッダー・フッター・ナビ)は本サイトと統一**:

- 飲食店(restaurant): 深緑#2D5F4F + 生成りクリーム
- 美容室(beauty-salon): ピンクベージュ#D4A5A5 + 白
- 整体院(clinic): 落ち着いた青緑#5B8B8B + 白
- 工務店(construction): ウッドブラウン#8B6F47 + 生成り
- 自動車整備(auto-repair): 黒#1A1A1A + ビビッドオレンジ#FF6B35(これは業種特性で例外的に強い色OK)
- 学習塾(cram-school): 明るい青#4A90E2 + クリーム
- 歯科医院(dental): 清潔感ある水色#7FB3D5 + 白

各サンプルページ最上部に注釈バー:
```
📌 これはサンプルサイトです。架空の事業者として作成しています。 [元のページに戻る]
```
注釈バーは var(--color-charcoal) 背景 + var(--color-cream) 文字、目立たせるが邪魔しない。

### 5.6 会社概要(`src/pages/about.astro`)

- 大きな引用風のミッションステートメント
- 代表挨拶セクションは丸型の写真プレースホルダー(オレンジ縁) + 本文
- 会社情報は table風のリストで

### 5.7 お問い合わせ(`src/pages/contact.astro`)

- フォーム入力欄: ボーダー var(--color-gray-light)、focus時に var(--color-orange) ボーダー
- ボタンは Primary 大型(width: 100% on mobile)
- 連絡方法カード(電話/メール/LINE)は3カラムグリッド

### 5.8 プライバシーポリシー(`src/pages/privacy.astro`)

- シンプルな1カラムレイアウト
- 見出しは Shippori Mincho、本文は Noto Sans JP

---

## 6. レスポンシブ対応

### ブレークポイント
- モバイル: 〜640px
- タブレット: 641〜1024px
- デスクトップ: 1025px〜

### モバイル時の調整
```css
@media (max-width: 1024px) {
  .hero {
    grid-template-columns: 1fr;
    padding: 60px 32px;
    text-align: center;
  }
  .hero-illustration { max-width: 360px; margin: 0 auto; }
  .pain-grid, .solution-grid, .pricing-grid { grid-template-columns: 1fr; }
  nav { display: none; /* ハンバーガーメニュー化 */ }
  header { padding: 16px 24px; }
  .cta-group { justify-content: center; }
  .section-title { font-size: 28px; }
}

@media (max-width: 640px) {
  .hero h1.hero-title { font-size: 32px; }
}
```

ハンバーガーメニュー: モバイル時のみ表示、タップで全画面オーバーレイメニュー(クリーム背景、Shippori Mincho大文字リンク)

---

## 7. アニメーション

### Hero section
- ヘッドラインとイラストに `animation: fadeInUp 0.8s ease-out` を適用
- イラストは0.2s遅延

```css
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}
```

### Float animation (装飾円)
```css
@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-12px); }
}
```

### スクロールトリガーアニメーション(オプショナル)
各セクションが viewport に入ったときに fadeInUp を発動。Intersection Observer で実装:
```js
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) entry.target.classList.add('visible');
  });
}, { threshold: 0.1 });
document.querySelectorAll('.fade-in-section').forEach(el => observer.observe(el));
```

---

## 8. アクセシビリティ要件

- すべてのSVGアイコンに `role="img"` と `<title>` を追加
- ボタンに適切な `aria-label`
- カラーコントラスト WCAG AA 基準を満たす(チャコール#2A2520 on クリーム#FAF7F2 = 13.5:1 で十分)
- `prefers-reduced-motion: reduce` 時はアニメーション無効化
- フォーム要素にラベルを必ず関連付け

---

## 9. パフォーマンス

- フォントは `display=swap` で読み込み(既に指定済み)
- SVGは可能な限りインライン(外部ファイル化しない、HTTPリクエスト削減)
- 画像はAVIF/WebP優先、`loading="lazy"` 属性
- Lighthouse スコア目標: Performance 90+, Accessibility 100, Best Practices 95+, SEO 100

---

## 10. 削除すべき要素

### 削除リスト
- 既存の濃紺#1F4E78、明るい青、既存オレンジ#F59E0Bの全カラー定義
- すべての絵文字(🔍📱💰🛡️🔧🍽️✂️🏥🏗️🚗📚🦷⭐ 等)
- 過度な装飾的グラデーション(虹色、強い対比のもの)
- 色の異なる複数の枠線が混在しているコンポーネント

### 残す要素
- ページ構造、URL、ファイル構成
- すべてのテキストコンテンツ(日本語コピー)
- 配置の論理(ヒーロー → 課題 → 解決策 → サンプル → 料金 → 声 → FAQ → CTA)
- フォーム機能、リンク先

---

## 11. 実装の進め方(推奨順序)

1. **デザイントークン設定** — `src/styles/global.css` を更新、CSS変数を全置き換え
2. **共通レイアウト** — `MainLayout.astro` 内のフォント読み込み、ベース文字色、背景色を更新
3. **Header / Footer** — 新デザインに置き換え
4. **共通コンポーネント** — Button, Card, SectionLabel など
5. **トップページ** — Hero(モックアップ忠実再現)→ 各セクション順に
6. **サービス詳細・料金・サンプル一覧** — トップページのコンポーネント流用
7. **業種別サンプル7ページ** — 業種カラーアレンジしつつ統一フレーム適用
8. **会社概要・お問い合わせ・プライバシー** — 補助ページ
9. **絵文字置換チェック** — grep で絵文字残存ないか確認
10. **レスポンシブ確認** — 320px / 768px / 1280px で表示確認
11. **Lighthouse 実行** — スコア確認、不足あれば改善

---

## 12. 完了確認チェックリスト

実装完了後、以下をすべて満たしていることを確認してください:

- [ ] 既存の濃紺#1F4E78が残っていない(`grep -r "1F4E78" src/` で空)
- [ ] 既存のオレンジ#F59E0Bが残っていない(`grep -r "F59E0B" src/` で空)
- [ ] 全ページから絵文字が排除されている(`grep -rE "[🔍📱💰🛡️🔧🍽️✂️🏥🏗️🚗📚🦷⭐]" src/` で空)
- [ ] Shippori Mincho と Noto Sans JP が読み込まれている
- [ ] ヒーローセクションが モックアップHTML と視覚的に一致している
- [ ] 全7業種のSVGアイコンが正しく表示されている
- [ ] モバイル(375px)で破綻なく表示される
- [ ] Lighthouse Performance スコアが 90 以上
- [ ] 全ページで `npm run build` が成功する
- [ ] 全ページのリンクが正しく機能する

---

## 13. 完了後の報告

実装完了したら、以下を報告してください:

1. 変更したファイル一覧
2. 各ページのスクリーンショット(または `localhost:4321/...` のURLリスト)
3. Lighthouse スコア
4. 残った絵文字や旧色がある場合はその箇所
5. 不明点や追加検討事項

---

以上の指示に従って、デザインを完全刷新してください。**モックアップHTML(`トップページデザイン_モックアップ.html`)を必ず最初に読み込み、視覚的リファレンスとして参照しながら実装してください。**
