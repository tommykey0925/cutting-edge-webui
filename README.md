# Cutting-Edge Web Page 学習ガイド

このプロジェクトで使用されているHTML/CSSの技術を解説します。

---

## 目次

1. [HTML構造](#html構造)
2. [CSS基礎](#css基礎)
3. [グラスモーフィズム](#グラスモーフィズム)
4. [グラデーション](#グラデーション)
5. [アニメーション](#アニメーション)
6. [レイアウト（Flexbox / Grid）](#レイアウト)
7. [レスポンシブデザイン](#レスポンシブデザイン)
8. [その他のテクニック](#その他のテクニック)

---

## HTML構造

### 基本構造

```html
<!DOCTYPE html>           <!-- HTML5宣言 -->
<html lang="ja">          <!-- 言語設定（日本語） -->
<head>
    <meta charset="UTF-8">                              <!-- 文字コード -->
    <meta name="viewport" content="width=device-width"> <!-- レスポンシブ対応 -->
    <title>タイトル</title>
    <link rel="stylesheet" href="style.css">           <!-- CSS読み込み -->
</head>
<body>
    <!-- コンテンツ -->
</body>
</html>
```

### セマンティックHTML

意味のあるタグを使うことで、構造が明確になります。

| タグ | 用途 |
|------|------|
| `<nav>` | ナビゲーション |
| `<main>` | メインコンテンツ |
| `<section>` | セクション区切り |
| `<footer>` | フッター |
| `<h1>`〜`<h6>` | 見出し（h1が最重要） |

### このプロジェクトの構造

```
body
├── .cursor-glow        // カーソル追従エフェクト
├── .grid-bg            // 背景グリッド
├── .gradient-orbs      // 背景の光の玉
├── nav.nav-glass       // ナビゲーションバー
├── main.hero           // ヒーローセクション
│   ├── .hero-content   // テキスト部分
│   └── .hero-visual    // ビジュアル部分
├── section.features    // 機能紹介
└── footer              // フッター
```

---

## CSS基礎

### リセットCSS

ブラウザのデフォルトスタイルを統一します。

```css
*,
*::before,
*::after {
    margin: 0;
    padding: 0;
    box-sizing: border-box;  /* paddingとborderを幅に含める */
}
```

### CSS変数（カスタムプロパティ）

色やサイズを変数として定義し、再利用できます。

```css
:root {
    --bg-dark: #0a0a0f;           /* 背景色 */
    --accent-cyan: #00f5ff;       /* アクセントカラー */
    --glass-bg: rgba(255, 255, 255, 0.05);  /* 半透明背景 */
}

/* 使用時 */
.element {
    background: var(--bg-dark);
    color: var(--accent-cyan);
}
```

### 疑似要素 `::before` / `::after`

要素の前後に装飾用の要素を追加できます。

```css
.feature-card::before {
    content: '';              /* 必須（空でもOK） */
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 2px;
    background: var(--gradient-main);
}
```

---

## グラスモーフィズム

半透明のガラスのような効果です。

```css
.nav-glass {
    background: rgba(255, 255, 255, 0.05);  /* 半透明背景 */
    backdrop-filter: blur(20px);            /* 背景をぼかす */
    -webkit-backdrop-filter: blur(20px);    /* Safari対応 */
    border: 1px solid rgba(255, 255, 255, 0.1);  /* 薄いボーダー */
    border-radius: 20px;                    /* 角丸 */
}
```

**ポイント**
- `rgba()` で透明度のある色を指定
- `backdrop-filter: blur()` で背後をぼかす
- 薄いボーダーでガラスの縁を表現

---

## グラデーション

### 線形グラデーション

```css
:root {
    --gradient-main: linear-gradient(135deg, #00f5ff, #a855f7, #ec4899);
    /*                              角度     色1      色2      色3    */
}
```

### グラデーションテキスト

```css
.gradient-text {
    background: var(--gradient-main);
    -webkit-background-clip: text;  /* 背景をテキストに切り抜く */
    background-clip: text;
    color: transparent;             /* 文字色を透明に */
}
```

### 放射グラデーション

```css
.cursor-glow {
    background: radial-gradient(circle, rgba(0, 245, 255, 0.15) 0%, transparent 70%);
    /*                          形状    中心の色                    外側の色     */
}
```

---

## アニメーション

### @keyframes の定義

```css
@keyframes float {
    0%, 100% { transform: translate(0, 0); }     /* 開始と終了 */
    50% { transform: translate(50px, -50px); }   /* 中間 */
}
```

### animation プロパティ

```css
.orb {
    animation: float 20s ease-in-out infinite;
    /*         名前  時間  イージング   繰り返し */
}

.orb-2 {
    animation-delay: -7s;  /* 途中から開始（負の値） */
}
```

### transform の種類

| プロパティ | 効果 |
|-----------|------|
| `translate(x, y)` | 移動 |
| `scale(n)` | 拡大縮小 |
| `rotate(deg)` | 回転 |
| `rotateX(deg)` | X軸回転（3D） |

### transition（ホバーアニメーション）

```css
.btn-primary {
    transition: all 0.3s ease;  /* 全プロパティを0.3秒で変化 */
}

.btn-primary:hover {
    transform: translateY(-3px);  /* 上に3px移動 */
    box-shadow: 0 20px 40px rgba(255, 255, 255, 0.2);
}
```

### シマー効果（流れるグラデーション）

```css
.gradient-text {
    background-size: 200% auto;  /* 背景を2倍サイズに */
    animation: shimmer 3s linear infinite;
}

@keyframes shimmer {
    0% { background-position: 0% center; }
    100% { background-position: 200% center; }
}
```

---

## レイアウト

### Flexbox

横並び・縦並びのレイアウトに最適。

```css
.nav-glass {
    display: flex;
    align-items: center;         /* 縦方向の中央揃え */
    justify-content: space-between;  /* 両端揃え */
    gap: 10px;                   /* 要素間の余白 */
}
```

**主なプロパティ**

| プロパティ | 値の例 | 効果 |
|-----------|--------|------|
| `justify-content` | `center`, `space-between`, `flex-start` | 主軸方向の配置 |
| `align-items` | `center`, `flex-start`, `stretch` | 交差軸方向の配置 |
| `flex-direction` | `row`, `column` | 並ぶ方向 |
| `gap` | `10px`, `1rem` | 要素間の余白 |

### CSS Grid

グリッドレイアウトに最適。

```css
.features {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    /*                     自動調整      最小300px〜最大均等 */
    gap: 24px;
}
```

**`auto-fit` + `minmax()` の組み合わせ**
- カード幅が最小300px
- 画面幅に応じて列数が自動調整
- レスポンシブに便利

---

## レスポンシブデザイン

### メディアクエリ

画面幅に応じてスタイルを切り替えます。

```css
/* 1024px以下の場合 */
@media (max-width: 1024px) {
    .hero {
        flex-direction: column;  /* 縦並びに変更 */
        text-align: center;
    }

    .nav-links {
        display: none;  /* ナビリンクを非表示 */
    }
}

/* 768px以下の場合 */
@media (max-width: 768px) {
    .hero-title {
        font-size: 2.5rem;  /* 文字サイズを小さく */
    }
}
```

### clamp() 関数

最小値〜最大値の範囲で自動調整されます。

```css
.hero-title {
    font-size: clamp(3rem, 6vw, 5rem);
    /*              最小   推奨   最大 */
}
```

- 画面幅の6%のサイズ
- ただし3rem未満にならない
- 5remを超えない

### viewport単位

| 単位 | 意味 |
|------|------|
| `vw` | ビューポート幅の1% |
| `vh` | ビューポート高さの1% |

```css
.hero {
    min-height: 100vh;  /* 画面の高さいっぱい */
}
```

---

## その他のテクニック

### position の種類

```css
.cursor-glow {
    position: fixed;  /* 画面に固定（スクロールしても動かない） */
}

.floating-card {
    position: absolute;  /* 親要素（relative）を基準に配置 */
    top: 40px;
    left: 20px;
}

.hero-visual {
    position: relative;  /* 子のabsoluteの基準になる */
}
```

### inset プロパティ

`top`, `right`, `bottom`, `left` の一括指定。

```css
.grid-bg {
    position: fixed;
    inset: 0;  /* top: 0; right: 0; bottom: 0; left: 0; と同じ */
}
```

### box-shadow

```css
.center-sphere {
    box-shadow:
        0 0 60px rgba(0, 245, 255, 0.5),   /* 第1の影 */
        0 0 120px rgba(168, 85, 247, 0.3); /* 第2の影（複数指定可） */
    /* x y ぼかし 色 */
}
```

### filter

```css
.orb {
    filter: blur(80px);  /* 要素自体をぼかす */
}
```

### z-index

要素の重なり順を制御します。

```css
.nav-glass { z-index: 1000; }  /* 最前面 */
.floating-card { z-index: 10; }
.grid-bg { z-index: -2; }      /* 最背面 */
```

### カスタムスクロールバー

```css
::-webkit-scrollbar {
    width: 8px;
}

::-webkit-scrollbar-track {
    background: #0a0a0f;
}

::-webkit-scrollbar-thumb {
    background: rgba(255, 255, 255, 0.1);
    border-radius: 4px;
}
```

### 選択時のスタイル

```css
::selection {
    background: #00f5ff;
    color: #0a0a0f;
}
```

---

## JavaScript（簡易説明）

### カーソル追従

```javascript
document.addEventListener('mousemove', (e) => {
    const glow = document.querySelector('.cursor-glow');
    glow.style.left = e.clientX + 'px';  // マウスのX座標
    glow.style.top = e.clientY + 'px';   // マウスのY座標
});
```

### パララックス効果

```javascript
document.addEventListener('mousemove', (e) => {
    const x = (e.clientX / window.innerWidth - 0.5) * 20;
    const y = (e.clientY / window.innerHeight - 0.5) * 20;
    // -0.5 することで中心を0にして、±10の範囲で移動
});
```

---

## 参考リンク

- [MDN Web Docs](https://developer.mozilla.org/ja/) - Web技術の公式リファレンス
- [CSS Tricks](https://css-tricks.com/) - CSSテクニック集
- [Can I Use](https://caniuse.com/) - ブラウザ対応状況の確認
