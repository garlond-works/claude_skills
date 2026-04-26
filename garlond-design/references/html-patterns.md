# HTML 出力パターン集

単一ファイル HTML（外部依存なし）を原則とする。
Google Fonts CDN は例外的に使用可（オフライン環境では除外）。

共通 CSS 変数（全パターン共通で `<style>` 冒頭に記述）：

```css
:root {
  --primary:   #3D3939;   /* ダークチャコール（案件に応じて差し替え） */
  --accent:    #00D5D5;   /* ターコイズ（固定） */
  --bg:        #F5F8F8;
  --white:     #FFFFFF;
  --gray1:     #6B7280;
  --gray2:     #AAAAAA;
  --text:      #1F2937;
  --radius:    8px;
  --shadow:    0 2px 8px rgba(0,0,0,0.08);
}
* { box-sizing: border-box; margin: 0; padding: 0; }
body {
  font-family: 'Noto Sans JP', 'Hiragino Sans', sans-serif;
  background: var(--bg);
  color: var(--text);
  line-height: 1.7;
}
```

---

## パターン1：プロジェクト進捗レポート

フェーズ進捗バー＋リスクバッジ＋アクションリスト。

```html
<!-- ヘッダー -->
<header style="background:var(--primary);color:#fff;padding:1.5rem 2rem;border-bottom:4px solid var(--accent)">
  <div style="font-size:0.8rem;opacity:.6;margin-bottom:.3rem">GarlondWorks</div>
  <h1 style="font-size:1.5rem;font-weight:700">{案件名} 進捗レポート</h1>
  <div style="font-size:0.85rem;opacity:.7;margin-top:.3rem">{YYYY-MM-DD}</div>
</header>

<!-- エグゼクティブサマリー -->
<section style="background:var(--white);border-left:4px solid var(--accent);margin:1.5rem 2rem;padding:1rem 1.2rem;border-radius:0 var(--radius) var(--radius) 0;box-shadow:var(--shadow)">
  <div style="font-size:.75rem;color:var(--accent);font-weight:700;letter-spacing:.08em;margin-bottom:.4rem">EXECUTIVE SUMMARY</div>
  <p>{サマリー3行以内}</p>
</section>

<!-- フェーズ進捗 -->
<section style="margin:0 2rem 1.5rem">
  <h2 style="font-size:1rem;font-weight:700;color:var(--primary);border-bottom:2px solid var(--accent);padding-bottom:.4rem;margin-bottom:1rem">フェーズ進捗</h2>
  <!-- 1フェーズ分 -->
  <div style="margin-bottom:1rem">
    <div style="display:flex;justify-content:space-between;margin-bottom:.3rem">
      <span style="font-size:.9rem;font-weight:600">{フェーズ名}</span>
      <span style="font-size:.85rem;color:var(--gray1)">{進捗%}</span>
    </div>
    <div style="background:#E5E7EB;border-radius:4px;height:8px">
      <div style="background:var(--accent);width:{進捗%};height:8px;border-radius:4px;transition:width .4s"></div>
    </div>
  </div>
</section>

<!-- リスク項目 -->
<section style="margin:0 2rem 1.5rem">
  <h2 style="font-size:1rem;font-weight:700;color:var(--primary);border-bottom:2px solid var(--accent);padding-bottom:.4rem;margin-bottom:1rem">リスク・課題</h2>
  <!-- 1件分 -->
  <div style="background:var(--white);border-radius:var(--radius);padding:.8rem 1rem;margin-bottom:.6rem;box-shadow:var(--shadow);display:flex;align-items:flex-start;gap:.8rem">
    <span style="background:#E05252;color:#fff;font-size:.7rem;font-weight:700;padding:.2rem .5rem;border-radius:3px;white-space:nowrap">高</span>
    <!-- 中：#D97706 / 低：#059669 -->
    <p style="font-size:.9rem">{リスク内容}</p>
  </div>
</section>

<!-- アクションアイテム -->
<section style="margin:0 2rem 2rem">
  <h2 style="font-size:1rem;font-weight:700;color:var(--primary);border-bottom:2px solid var(--accent);padding-bottom:.4rem;margin-bottom:1rem">次のアクション</h2>
  <div style="background:var(--white);border-radius:var(--radius);padding:.8rem 1rem;margin-bottom:.6rem;box-shadow:var(--shadow);display:flex;justify-content:space-between;align-items:center">
    <span style="font-size:.9rem">{アクション内容}</span>
    <span style="font-size:.8rem;color:var(--gray1);white-space:nowrap">{担当} / {期限}</span>
  </div>
</section>
```

---

## パターン2：設計比較レポート（フィルタ付き）

複数案をカードで横並び比較。フィルタボタンで並び替え可能。

```html
<!-- フィルタボタン -->
<div style="display:flex;gap:.5rem;margin:1.5rem 2rem .5rem">
  <button onclick="sortCards('cost')"
    style="background:var(--accent);color:#fff;border:none;padding:.4rem .9rem;border-radius:20px;cursor:pointer;font-size:.8rem">コスト順</button>
  <button onclick="sortCards('risk')"
    style="background:var(--white);color:var(--text);border:1px solid #D1D5DB;padding:.4rem .9rem;border-radius:20px;cursor:pointer;font-size:.8rem">リスク順</button>
</div>

<!-- 比較カードグリッド -->
<div id="card-grid" style="display:grid;grid-template-columns:repeat(auto-fit,minmax(260px,1fr));gap:1rem;margin:0 2rem 2rem">
  <!-- 1案分（推奨案には data-recommended="true"） -->
  <div data-cost="{数値}" data-risk="{数値}" data-recommended="true"
    style="background:var(--white);border-radius:var(--radius);box-shadow:var(--shadow);border:2px solid var(--accent);overflow:hidden">
    <div style="background:var(--primary);color:#fff;padding:.7rem 1rem;display:flex;justify-content:space-between;align-items:center">
      <span style="font-weight:700">{案名}</span>
      <span style="background:var(--accent);color:var(--primary);font-size:.7rem;font-weight:700;padding:.2rem .5rem;border-radius:3px">推奨</span>
    </div>
    <div style="padding:1rem">
      <div style="margin-bottom:.8rem">
        <div style="font-size:.75rem;color:var(--gray1);margin-bottom:.2rem">メリット</div>
        <ul style="font-size:.85rem;padding-left:1.1rem">{メリット箇条書き}</ul>
      </div>
      <div style="margin-bottom:.8rem">
        <div style="font-size:.75rem;color:var(--gray1);margin-bottom:.2rem">デメリット</div>
        <ul style="font-size:.85rem;padding-left:1.1rem">{デメリット箇条書き}</ul>
      </div>
      <div style="display:flex;gap:.5rem;font-size:.8rem">
        <span style="background:#EFF6FF;color:#1D4ED8;padding:.2rem .6rem;border-radius:4px">コスト: {金額}</span>
        <span style="background:#FEF3C7;color:#92400E;padding:.2rem .6rem;border-radius:4px">リスク: {高/中/低}</span>
      </div>
    </div>
  </div>
</div>

<script>
function sortCards(key) {
  const grid = document.getElementById('card-grid');
  const cards = Array.from(grid.children);
  cards.sort((a,b) => Number(a.dataset[key]) - Number(b.dataset[key]));
  cards.forEach(c => grid.appendChild(c));
}
</script>
```

---

## パターン3：コードレビューレポート

指摘をカードで表示。重大度フィルタ付き。

```html
<!-- フィルタボタン -->
<div style="display:flex;gap:.5rem;margin:1.5rem 2rem .5rem">
  <button onclick="filter('all')" style="background:var(--accent);color:#fff;border:none;padding:.4rem .9rem;border-radius:20px;cursor:pointer;font-size:.8rem">すべて</button>
  <button onclick="filter('high')" style="background:#FEE2E2;color:#991B1B;border:none;padding:.4rem .9rem;border-radius:20px;cursor:pointer;font-size:.8rem">高 ({件数})</button>
  <button onclick="filter('mid')" style="background:#FEF3C7;color:#92400E;border:none;padding:.4rem .9rem;border-radius:20px;cursor:pointer;font-size:.8rem">中 ({件数})</button>
  <button onclick="filter('low')" style="background:#D1FAE5;color:#065F46;border:none;padding:.4rem .9rem;border-radius:20px;cursor:pointer;font-size:.8rem">低 ({件数})</button>
</div>

<!-- 指摘カード -->
<div class="review-card" data-severity="high"
  style="background:var(--white);border-left:4px solid #E05252;border-radius:var(--radius);padding:1rem;margin:0 2rem .6rem;box-shadow:var(--shadow)">
  <div style="display:flex;align-items:center;gap:.6rem;margin-bottom:.5rem">
    <span style="background:#E05252;color:#fff;font-size:.7rem;font-weight:700;padding:.2rem .5rem;border-radius:3px">高</span>
    <code style="font-size:.8rem;color:var(--gray1)">{ファイルパス}:{行番号}</code>
  </div>
  <p style="font-size:.9rem;margin-bottom:.5rem">{指摘内容}</p>
  <div style="background:#F3F4F6;border-radius:4px;padding:.5rem .8rem;font-size:.8rem;color:#374151">
    修正案: {修正提案}
  </div>
</div>

<script>
function filter(level) {
  document.querySelectorAll('.review-card').forEach(c => {
    c.style.display = (level === 'all' || c.dataset.severity === level) ? 'block' : 'none';
  });
}
</script>
```

---

## パターン4：クライアント向け提案サマリー

課題→解決策→効果の3ステップ構成。折り畳みQ&A付き。印刷CSS対応。

```html
<style>
  @media print {
    .no-print { display: none !important; }
    body { background: white; }
    header { break-after: avoid; }
  }
</style>

<!-- 3ステップ -->
<div style="display:grid;grid-template-columns:repeat(3,1fr);gap:1rem;margin:1.5rem 2rem">
  <!-- 課題 -->
  <div style="background:var(--white);border-radius:var(--radius);overflow:hidden;box-shadow:var(--shadow)">
    <div style="background:#E05252;color:#fff;padding:.6rem 1rem;font-size:.85rem;font-weight:700">01 課題</div>
    <div style="padding:.8rem 1rem;font-size:.9rem">{課題内容}</div>
  </div>
  <!-- 解決策 -->
  <div style="background:var(--white);border-radius:var(--radius);overflow:hidden;box-shadow:var(--shadow)">
    <div style="background:var(--primary);color:#fff;padding:.6rem 1rem;font-size:.85rem;font-weight:700">02 解決策</div>
    <div style="padding:.8rem 1rem;font-size:.9rem">{解決策内容}</div>
  </div>
  <!-- 効果 -->
  <div style="background:var(--white);border-radius:var(--radius);overflow:hidden;box-shadow:var(--shadow)">
    <div style="background:var(--accent);color:var(--primary);padding:.6rem 1rem;font-size:.85rem;font-weight:700">03 効果</div>
    <div style="padding:.8rem 1rem;font-size:.9rem">{効果内容}</div>
  </div>
</div>

<!-- Q&A 折り畳み -->
<section style="margin:0 2rem 2rem">
  <h2 style="font-size:1rem;font-weight:700;color:var(--primary);border-bottom:2px solid var(--accent);padding-bottom:.4rem;margin-bottom.8rem">よくある質問</h2>
  <details style="background:var(--white);border-radius:var(--radius);margin-bottom:.5rem;box-shadow:var(--shadow)">
    <summary style="padding:.8rem 1rem;cursor:pointer;font-weight:600;font-size:.9rem;list-style:none">
      ▶ {質問}
    </summary>
    <div style="padding:.6rem 1rem 1rem;font-size:.9rem;border-top:1px solid #E5E7EB">{回答}</div>
  </details>
</section>
```

---

## パターン5：ダッシュボード（KPI集約）

数値KPIを大きく表示。リアルタイム更新不要の静的版。

```html
<!-- KPIカードグリッド -->
<div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:1rem;margin:1.5rem 2rem">
  <!-- 1KPI分 -->
  <div style="background:var(--white);border-radius:var(--radius);padding:1.2rem;box-shadow:var(--shadow);border-top:3px solid var(--accent)">
    <div style="font-size:.75rem;color:var(--gray1);margin-bottom:.4rem">{KPIラベル}</div>
    <div style="display:flex;align-items:baseline;gap:.2rem">
      <span style="font-size:2.4rem;font-weight:700;color:var(--primary);line-height:1">{数値}</span>
      <span style="font-size:.9rem;color:var(--gray1)">{単位}</span>
    </div>
    <div style="font-size:.75rem;margin-top:.4rem">
      <span style="color:#059669">↑ {前期比}</span>
    </div>
  </div>
</div>

<!-- データテーブル -->
<div style="margin:0 2rem 2rem;overflow-x:auto">
  <table style="width:100%;border-collapse:collapse;background:var(--white);border-radius:var(--radius);overflow:hidden;box-shadow:var(--shadow)">
    <thead>
      <tr style="background:var(--primary);color:#fff">
        <th style="padding:.7rem 1rem;text-align:left;font-size:.85rem">{列名}</th>
      </tr>
    </thead>
    <tbody>
      <tr style="border-bottom:1px solid #E5E7EB">
        <td style="padding:.65rem 1rem;font-size:.9rem">{値}</td>
      </tr>
    </tbody>
  </table>
</div>
```

---

## 出力ファイル命名規則

```
YYYYMMDD_[案件イニシャル]_[種別].html
例：
20260519_H社_進捗レポート.html
20260519_KT_比較検討.html
```

保存先：プロジェクトフォルダ内 `outputs/`
