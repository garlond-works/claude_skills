# スライドパターン実装集

各パターンはそのままコピーして使える実装コード。
変数 `P`（Primary）`A`（Accent）`W`（White）`BG`（Background）は
SKILL.md のテーマカラー定義から受け取ること。

---

## パターン1：タイトルスライド（title）

ダーク背景＋左サイドアクセントライン＋アイコン右上。

```javascript
const s = pres.addSlide();
s.background = { color: P.dark };

// 左サイドアクセントライン
s.addShape(pres.shapes.RECTANGLE, {
  x: 0, y: 0, w: 0.18, h: 5.625,
  fill: { color: P.accent }, line: { color: P.accent }
});

// メインタイトル（ターコイズ）
s.addText(mainTitle, {
  x: 0.5, y: 0.9, w: 8.8, h: 0.9,
  fontSize: 40, bold: true, color: P.accent,
  fontFace: "Arial Black", margin: 0
});

// サブタイトル（ホワイト）
s.addText(subTitle, {
  x: 0.5, y: 1.85, w: 8.8, h: 0.75,
  fontSize: 28, bold: true, color: "FFFFFF",
  fontFace: "Arial Black", margin: 0
});

// クライアント名
s.addText(clientName + "  様", {
  x: 0.5, y: 2.8, w: 8.8, h: 0.45,
  fontSize: 15, color: "AAAAAA", fontFace: "Calibri", margin: 0
});

// 右上アイコン
const ico = await icon(IconComponent, "#" + P.accent, 256);
s.addImage({ data: ico, x: 8.1, y: 0.7, w: 1.4, h: 1.4 });

// フッター
s.addText("GarlondWorks  |  " + year, {
  x: 0.5, y: 5.1, w: 9, h: 0.3,
  fontSize: 11, color: "555555", fontFace: "Calibri", margin: 0
});
```

---

## パターン2：課題・問題提起（problem）

3列カードで現状の課題を整理。各カードにカラーコーディング。

```javascript
const s = pres.addSlide();
s.background = { color: "F5F8F8" };

// タイトル帯
s.addShape(pres.shapes.RECTANGLE, {
  x: 0, y: 0, w: 10, h: 0.68,
  fill: { color: P.dark }, line: { color: P.dark }
});
s.addText(slideTitle, {
  x: 0.35, y: 0, w: 9.3, h: 0.68,
  fontSize: 20, bold: true, color: "FFFFFF",
  fontFace: "Arial Black", valign: "middle", margin: 0
});

// 3列カード（colors: green/navy/amber など案件に合わせて）
const cards = [
  { x: 0.3,  color: "059669", headerBg: "059669", label: "現場", items: [...] },
  { x: 3.55, color: "1E3A5F", headerBg: "1E3A5F", label: "事務", items: [...] },
  { x: 6.8,  color: "D97706", headerBg: "D97706", label: "経理", items: [...] },
];
const cardW = 2.9, cardH = 3.8;

cards.forEach(card => {
  // ベース
  s.addShape(pres.shapes.RECTANGLE, {
    x: card.x, y: 0.88, w: cardW, h: cardH,
    fill: { color: "FFFFFF" },
    line: { color: card.color, pt: 2 },
    shadow: makeShadow()
  });
  // ヘッダー
  s.addShape(pres.shapes.RECTANGLE, {
    x: card.x, y: 0.88, w: cardW, h: 0.46,
    fill: { color: card.headerBg }, line: { color: card.headerBg }
  });
  s.addText(card.label, {
    x: card.x, y: 0.88, w: cardW, h: 0.46,
    fontSize: 14, bold: true, color: "FFFFFF",
    align: "center", valign: "middle",
    fontFace: "Arial Black", margin: 0
  });
  // 項目（最大4つ）
  card.items.forEach((item, i) => {
    s.addText("  " + item, {
      x: card.x + 0.12, y: 1.5 + i * 0.62,
      w: cardW - 0.24, h: 0.54,
      fontSize: 11.5, color: "2D2D2D", fontFace: "Calibri"
    });
  });
});

// 下部ボトルネックボックス
s.addShape(pres.shapes.RECTANGLE, {
  x: 2.8, y: 4.88, w: 4.4, h: 0.56,
  fill: { color: "E05252" }, line: { color: "E05252" },
  shadow: makeShadow()
});
s.addText(bottleneckText, {
  x: 2.8, y: 4.88, w: 4.4, h: 0.56,
  fontSize: 12, bold: true, color: "FFFFFF",
  align: "center", valign: "middle", fontFace: "Calibri", margin: 0
});
```

---

## パターン3：方針・アプローチ（approach）

左右2列比較。左：提案内容、右：アナログ維持 など。

```javascript
const s = pres.addSlide();
s.background = { color: "F5F8F8" };

// タイトル帯（共通）
// ... （パターン2と同じ）

// キャッチフレーズ
s.addText(catchphrase, {
  x: 0.5, y: 0.85, w: 9, h: 0.58,
  fontSize: 21, bold: true, color: P.accentDark,
  fontFace: "Arial Black", margin: 0
});

const cols = [
  { x: 0.3,  color: P.accent,   label: "デジタル化する",   items: [...] },
  { x: 5.15, color: "6B7280",   label: "アナログのまま残す", items: [...] },
];
const colW = 4.5, colH = 3.8;

cols.forEach(col => {
  s.addShape(pres.shapes.RECTANGLE, {
    x: col.x, y: 1.55, w: colW, h: colH,
    fill: { color: "FFFFFF" },
    line: { color: col.color, pt: 2 },
    shadow: makeShadow()
  });
  s.addShape(pres.shapes.RECTANGLE, {
    x: col.x, y: 1.55, w: colW, h: 0.44,
    fill: { color: col.color }, line: { color: col.color }
  });
  s.addText(col.label, {
    x: col.x + 0.1, y: 1.55, w: colW - 0.2, h: 0.44,
    fontSize: 12, bold: true, color: "FFFFFF",
    valign: "middle", fontFace: "Calibri", margin: 0
  });
  col.items.forEach((item, i) => {
    s.addText(item, {
      x: col.x + 0.15, y: 2.12 + i * 0.6,
      w: colW - 0.3, h: 0.52,
      fontSize: 11.5, color: "2D2D2D", fontFace: "Calibri"
    });
  });
});

// 最下部アクセントバー（接着剤メッセージ等）
s.addShape(pres.shapes.RECTANGLE, {
  x: 0.3, y: 5.32, w: 9.4, h: 0.22,
  fill: { color: P.accent }, line: { color: P.accent }
});
s.addText(footerMessage, {
  x: 0.3, y: 5.30, w: 9.4, h: 0.26,
  fontSize: 10, bold: true, color: "2D2D2D",
  align: "center", valign: "middle", fontFace: "Calibri", margin: 0
});
```

---

## パターン4：システム構成・アーキテクチャ（architecture）

ダーク背景＋階層コンテナ。上から下へデータフローを表現。

```javascript
const s = pres.addSlide();
s.background = { color: P.dark };

// タイトル（ターコイズ）
s.addText(slideTitle, {
  x: 0.3, y: 0.12, w: 9.4, h: 0.58,
  fontSize: 22, bold: true, color: P.accent,
  fontFace: "Arial Black", margin: 0
});
// サブタイトル
s.addText(subTitle, {
  x: 0.3, y: 0.66, w: 9.4, h: 0.28,
  fontSize: 11, color: "AAAAAA", fontFace: "Calibri", margin: 0
});

// 外部サービス行（上段）
// 各ボックスは { label, sub, x } で定義
externalBoxes.forEach(b => {
  s.addShape(pres.shapes.RECTANGLE, {
    x: b.x, y: 1.05, w: 2.95, h: 0.72,
    fill: { color: "1A4035" },
    line: { color: P.accent, pt: 1.5 }
  });
  s.addText(b.label, {
    x: b.x + 0.1, y: 1.05, w: 2.75, h: 0.38,
    fontSize: 12, bold: true, color: P.accent,
    valign: "middle", fontFace: "Calibri", margin: 0
  });
  s.addText(b.sub, {
    x: b.x + 0.1, y: 1.43, w: 2.75, h: 0.28,
    fontSize: 10, color: "AAAAAA", fontFace: "Calibri", margin: 0
  });
});

// 縦矢印（各ボックスの中心X座標）
arrowXs.forEach(cx => {
  s.addShape(pres.shapes.LINE, {
    x: cx, y: 1.77, w: 0, h: 0.36,
    line: { color: P.accent, width: 1.5, dashType: "sysDash" }
  });
});

// レイヤーコンテナ（SharePoint, Power Platform, Teams）
layers.forEach(layer => {
  // 外枠
  s.addShape(pres.shapes.RECTANGLE, {
    x: 0.3, y: layer.y, w: 9.4, h: layer.h,
    fill: { color: layer.bg },
    line: { color: layer.border, pt: 1.5 }
  });
  // ラベル
  s.addText(layer.label, {
    x: 0.5, y: layer.y, w: 9, h: 0.38,
    fontSize: 13, bold: true, color: layer.textColor,
    valign: "middle", fontFace: "Calibri", margin: 0
  });
  // 内側ボックス
  layer.boxes.forEach((box, i) => {
    const bx = 0.5 + i * (layer.boxW + 0.15);
    s.addShape(pres.shapes.RECTANGLE, {
      x: bx, y: layer.y + 0.42, w: layer.boxW, h: 0.46,
      fill: { color: layer.boxBg },
      line: { color: layer.border }
    });
    s.addText(box, {
      x: bx, y: layer.y + 0.42, w: layer.boxW, h: 0.46,
      fontSize: 10.5, color: "FFFFFF",
      align: "center", valign: "middle", fontFace: "Calibri", margin: 0
    });
  });
  // 次レイヤーへの矢印
  if (layer.showArrow) {
    s.addShape(pres.shapes.LINE, {
      x: 5.0, y: layer.y + layer.h, w: 0, h: 0.24,
      line: { color: P.accent, width: 1.5 }
    });
  }
});
```

---

## パターン5：フェーズ別ロードマップ（roadmap）

3列フェーズカード。フェーズごとに色を変えて進行感を演出。

```javascript
const s = pres.addSlide();
s.background = { color: "F5F8F8" };

// タイトル帯（共通）
// ...

const phases = [
  { label: "Phase 1", period: "~1ヶ月目", title: "基盤を整える",
    color: "059669", items: [...] },
  { label: "Phase 2", period: "~3ヶ月目", title: "自動化で負担半減",
    color: P.accentDark, items: [...] },
  { label: "Phase 3", period: "~6ヶ月目", title: "可視化で経営強化",
    color: "D97706", items: [...] },
];
const cardW = 3.05, cardH = 4.6;

phases.forEach((ph, i) => {
  const x = 0.3 + i * (cardW + 0.14);
  // ベース
  s.addShape(pres.shapes.RECTANGLE, {
    x, y: 0.82, w: cardW, h: cardH,
    fill: { color: "FFFFFF" },
    line: { color: ph.color, pt: 2 },
    shadow: makeShadow()
  });
  // ヘッダー
  s.addShape(pres.shapes.RECTANGLE, {
    x, y: 0.82, w: cardW, h: 0.88,
    fill: { color: ph.color }, line: { color: ph.color }
  });
  s.addText(ph.label, {
    x: x + 0.1, y: 0.82, w: cardW - 0.2, h: 0.44,
    fontSize: 17, bold: true, color: "FFFFFF",
    valign: "middle", fontFace: "Arial Black", margin: 0
  });
  s.addText(ph.period, {
    x: x + 0.1, y: 1.24, w: cardW - 0.2, h: 0.4,
    fontSize: 11, color: "FFFFFF",
    valign: "middle", fontFace: "Calibri", margin: 0
  });
  // フェーズタイトル
  s.addText(ph.title, {
    x: x + 0.1, y: 1.8, w: cardW - 0.2, h: 0.5,
    fontSize: 12.5, bold: true, color: ph.color,
    fontFace: "Calibri", margin: 0
  });
  // 項目（最大4つ）
  ph.items.forEach((item, j) => {
    s.addText([
      { text: ">  ", options: { color: ph.color, bold: true } },
      { text: item, options: { color: "2D2D2D" } }
    ], {
      x: x + 0.12, y: 2.42 + j * 0.68,
      w: cardW - 0.24, h: 0.6,
      fontSize: 11, fontFace: "Calibri"
    });
  });
});
```

---

## パターン6：まとめ・クロージング（closing）

ダーク背景＋3点ボックス＋フッター署名。

```javascript
const s = pres.addSlide();
s.background = { color: P.dark };

// 左サイドライン
s.addShape(pres.shapes.RECTANGLE, {
  x: 0, y: 0, w: 0.18, h: 5.625,
  fill: { color: P.accent }, line: { color: P.accent }
});

s.addText(slideTitle, {
  x: 0.5, y: 0.25, w: 9, h: 0.6,
  fontSize: 26, bold: true, color: P.accent,
  fontFace: "Arial Black", margin: 0
});

// 3点ポイントボックス
const points = [
  { num: "1", title: "ポイントA", body: "説明テキスト" },
  { num: "2", title: "ポイントB", body: "説明テキスト" },
  { num: "3", title: "ポイントC", body: "説明テキスト" },
];

points.forEach((pt, i) => {
  const y = 1.05 + i * 1.38;
  // ベースカード（ダークより少し明るい）
  s.addShape(pres.shapes.RECTANGLE, {
    x: 0.5, y, w: 9.0, h: 1.2,
    fill: { color: "2A2626" },
    line: { color: P.accent, pt: 1 }
  });
  // 番号バッジ（ターコイズブロック）
  s.addShape(pres.shapes.RECTANGLE, {
    x: 0.5, y, w: 0.7, h: 1.2,
    fill: { color: P.accent }, line: { color: P.accent }
  });
  s.addText(pt.num, {
    x: 0.5, y, w: 0.7, h: 1.2,
    fontSize: 20, bold: true, color: P.dark,
    align: "center", valign: "middle", fontFace: "Arial Black", margin: 0
  });
  // タイトル
  s.addText(pt.title, {
    x: 1.35, y: y + 0.1, w: 7.9, h: 0.42,
    fontSize: 14, bold: true, color: P.accent,
    fontFace: "Calibri", margin: 0
  });
  // 本文
  s.addText(pt.body, {
    x: 1.35, y: y + 0.54, w: 7.9, h: 0.56,
    fontSize: 11, color: "CCCCCC",
    fontFace: "Calibri", margin: 0
  });
});

// フッター
s.addText("GarlondWorks  |  宍戸", {
  x: 0.5, y: 5.2, w: 9.2, h: 0.3,
  fontSize: 11, color: "555555",
  align: "right", fontFace: "Calibri", margin: 0
});
```

---

## パターン7：KPI・数値強調スライド（kpi）

大きな数値を前面に出したいときに使用。左に数値callout、右に説明リスト。

```javascript
const s = pres.addSlide();
s.background = { color: "F5F8F8" };

// タイトル帯（共通）
s.addShape(pres.shapes.RECTANGLE, {
  x: 0, y: 0, w: 10, h: 0.68,
  fill: { color: P.dark }, line: { color: P.dark }
});
s.addShape(pres.shapes.RECTANGLE, {
  x: 0, y: 0.68, w: 10, h: 0.04,
  fill: { color: P.accent }, line: { color: P.accent }
});
s.addText(slideTitle, {
  x: 0.35, y: 0, w: 9.3, h: 0.68,
  fontSize: 20, bold: true, color: "FFFFFF",
  fontFace: "Arial Black", valign: "middle", margin: 0
});

// KPIカード（左側）
const kpis = [
  { value: "98", unit: "%", label: "現場報告デジタル化率" },
  { value: "1/3", unit: "", label: "事務作業時間削減" },
];

kpis.forEach((kpi, i) => {
  const kx = 0.35 + i * 3.0, ky = 0.9;
  s.addShape(pres.shapes.RECTANGLE, {
    x: kx, y: ky, w: 2.7, h: 1.8,
    fill: { color: P.dark },
    line: { color: P.accent, pt: 2 },
    shadow: makeShadow()
  });
  // 薄いアクセント背景
  s.addShape(pres.shapes.RECTANGLE, {
    x: kx, y: ky, w: 2.7, h: 1.8,
    fill: { color: P.accent, transparency: 88 },
    line: { color: P.accent, pt: 0 }
  });
  // 数値（大）
  s.addText(kpi.value, {
    x: kx + 0.1, y: ky + 0.2, w: 2.2, h: 0.95,
    fontSize: 52, bold: true, color: P.accent,
    fontFace: "Arial Black", align: "center", margin: 0
  });
  // 単位（小）
  if (kpi.unit) {
    s.addText(kpi.unit, {
      x: kx + 2.0, y: ky + 0.7, w: 0.5, h: 0.4,
      fontSize: 18, color: P.accent,
      fontFace: "Calibri", margin: 0
    });
  }
  // ラベル
  s.addText(kpi.label, {
    x: kx + 0.1, y: ky + 1.3, w: 2.5, h: 0.4,
    fontSize: 10.5, color: "AAAAAA",
    fontFace: "Calibri", align: "center", margin: 0
  });
});

// 右側：説明・補足リスト
const descX = 6.4;
bullets.forEach((b, i) => {
  // ドット
  s.addShape(pres.shapes.RECTANGLE, {
    x: descX, y: 1.12 + i * 0.7, w: 0.08, h: 0.08,
    fill: { color: P.accent }, line: { color: P.accent }
  });
  s.addText(b, {
    x: descX + 0.18, y: 0.92 + i * 0.7, w: 3.3, h: 0.5,
    fontSize: 12, color: "2D3748", fontFace: "Calibri"
  });
});
```

---

## パターン8：ブリッジスライド・セクション区切り（bridge）

長い提案書（8枚以上）のセクション間に挟む。次のセクションを予告する。

```javascript
const s = pres.addSlide();
s.background = { color: P.dark };

// 左サイドライン
s.addShape(pres.shapes.RECTANGLE, {
  x: 0, y: 0, w: 0.18, h: 5.625,
  fill: { color: P.accent }, line: { color: P.accent }
});

// セクション番号（超大）
s.addText(sectionNum, {  // e.g. "02"
  x: 0.5, y: 0.6, w: 9, h: 2.4,
  fontSize: 140, bold: true, color: P.accent,
  fontFace: "Arial Black", align: "center",
  transparency: 20, margin: 0
});

// セクションタイトル
s.addText(sectionTitle, {
  x: 0.5, y: 3.2, w: 9, h: 0.7,
  fontSize: 28, bold: true, color: "FFFFFF",
  fontFace: "Arial Black", align: "center", margin: 0
});

// 予告テキスト（1行）
s.addText(nextDescription, {
  x: 0.5, y: 4.0, w: 9, h: 0.5,
  fontSize: 14, color: "AAAAAA",
  fontFace: "Calibri", align: "center", margin: 0
});
```

---

## パターン9：境界線分割タイトル（title-split）

design-rules.md の [A] を完全実装。
左エリア72%をダーク、右エリア28%をさらに濃いダークに分割する洗練スタイル。
通常の `title` パターンより格上感が出るため、重要案件に使う。

```javascript
const DARK_ALT = "0A1F33";  // P.dark より濃い（建設業例）

const s = pres.addSlide();
s.background = { color: P.dark };

// 右エリア（濃いダーク）
s.addShape(pres.shapes.RECTANGLE, {
  x: 7.2, y: 0, w: 2.8, h: 5.625,
  fill: { color: DARK_ALT }, line: { color: DARK_ALT }
});

// 境界縦線（アクセント色）
s.addShape(pres.shapes.RECTANGLE, {
  x: 7.2, y: 0, w: 0.04, h: 5.625,
  fill: { color: P.accent }, line: { color: P.accent }
});

// 左エリア：メインタイトル
s.addText(mainTitle, {
  x: 0.5, y: 0.9, w: 6.5, h: 0.9,
  fontSize: 38, bold: true, color: P.accent,
  fontFace: "Arial Black", margin: 0
});

// サブタイトル
s.addText(subTitle, {
  x: 0.5, y: 1.9, w: 6.5, h: 0.7,
  fontSize: 24, bold: true, color: "FFFFFF",
  fontFace: "Arial Black", margin: 0
});

// クライアント名
s.addText(clientName + "  様", {
  x: 0.5, y: 2.8, w: 6.5, h: 0.45,
  fontSize: 14, color: "AAAAAA", fontFace: "Calibri", margin: 0
});

// 左エリア：水平区切り線
s.addShape(pres.shapes.LINE, {
  x: 0.5, y: 2.72, w: 5.5, h: 0,
  line: { color: P.accent, width: 0.8 }
});

// 右エリア：アイコン3点散りばめ
const iconPositions = [
  { x: 7.5, y: 0.5 },
  { x: 8.5, y: 2.1 },
  { x: 7.7, y: 3.8 },
];
for (const [idx, pos] of iconPositions.entries()) {
  const ico = await icon(iconComponents[idx], "#" + P.accent, 256);
  s.addImage({ data: ico, x: pos.x, y: pos.y, w: 1.1, h: 1.1 });
}

// フッター
s.addText("GarlondWorks  |  " + year, {
  x: 0.5, y: 5.1, w: 9, h: 0.3,
  fontSize: 11, color: "555555",
  align: "right", fontFace: "Calibri", margin: 0
});
```

---

## 共通ユーティリティ関数

```javascript
// テーマカラーオブジェクト（案件ごとに差し替える）
const THEME = {
  dark:       "1E3A5F",  // 建設業DXの例
  accent:     "00D5D5",
  accentDark: "009999",
  white:      "FFFFFF",
  bg:         "F5F8F8",
  gray1:      "6B7280",
  gray2:      "AAAAAA",
};

// shadowファクトリ（必ず関数で生成）
const makeShadow = () => ({
  type: "outer", color: "000000",
  blur: 8, offset: 2, angle: 135, opacity: 0.10
});

// アイコン生成
async function icon(IconComp, colorHex = "#FFFFFF", size = 256) {
  const svg = ReactDOMServer.renderToStaticMarkup(
    React.createElement(IconComp, { color: colorHex, size: String(size) })
  );
  const buf = await sharp(Buffer.from(svg)).png().toBuffer();
  return "image/png;base64," + buf.toString("base64");
}

// requires
const pptxgen = require("pptxgenjs");
const React = require("react");
const ReactDOMServer = require("react-dom/server");
const sharp = require("sharp");
```
