# AI指示テンプレート：トレード記録HTML生成

> このテンプレートをAIに渡し、下記の【入力データ】を埋めることで、
> 統一フォーマットのトレード記録HTMLを生成させる。

---

## 【指示文】

以下の仕様と入力データをもとに、トレード記録HTMLファイルを1ファイル生成してください。
既存ファイルとの一貫性を保つため、構造・クラス名・属性名は仕様に完全に従ってください。
推測や省略は禁止です。不明な項目は「—」を使用してください。

---

## 【入力データ】

```
日付          : YYYY-MM-DD
通貨ペア      : （例：USD/CAD）
方向          : 買い / 売り
ロット        : （例：0.01）
エントリー価格: （例：1.36497）
決済価格      : （例：1.36397）
損切り価格    : （例：1.36697）
損益 pips     : （例：+10 / -20）
時間（開始）  : HH:MM
時間（終了）  : HH:MM
市場セッション: （例：NY時間 / ロンドン時間 / 東京時間）
ランク        : S / A / B / C
要注意フラグ  : true / false
一行メモ      : （インデックス表示用・40字以内）

■ 分析チェック項目（カテゴリ・項目名・評価）
  カテゴリ    | 項目名              | 評価
  -----------------------------------------------
  環境認識    | （項目）            | OK / NG / △
  インジケーター | （項目）          | OK / NG / △
  エントリー  | （項目）            | OK / NG / △
  時間帯      | （項目）            | OK / NG / △
  ※カテゴリ・項目数は実態に合わせて増減可

■ 感情ログ（1=冷静 ／ 5=パニック）
  エントリー前: 1〜5
  保有中      : 1〜5
  決済後      : 1〜5

■ 振り返りメモ（各セクション本文）
  今回の本質  : （フリーテキスト）
  負けの原因  : （勝ちトレードの場合は「改善点」に変更）
  改善ルール  : （フリーテキスト）
  一言        : （フリーテキスト）

■ チャート画像ファイル名
  日足  : （例：20260421_day.png）
  1時間足: （例：20260421_hour.png）

■ 戻り先インデックスファイル名
  （例：index_tradenissi_2026_4.html）
```

---

## 【HTML仕様】

### ファイル名
`YYYY-MM-DD.html`（日付をそのままファイル名に使用）

---

### 全体構造

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>📅 YYYY-MM-DD トレード記録</title>
  <link rel="stylesheet" href="../style.css">
</head>
<body>
  <!-- 1. 隠しデータ要素 -->
  <!-- 2. スティッキーヘッダー -->
  <!-- 3. .page-wrap（以降すべてのカードを内包） -->
</body>
</html>
```

---

### 1. 隠しデータ要素

インデックスページが読み取る。**すべての属性を必ず出力すること。**

```html
<div class="trade-data"
  data-date    = "YYYY-MM-DD"
  data-pair    = "通貨ペア"
  data-dir     = "買い または 売り"
  data-session = "市場セッション名"
  data-pips    = "+10 または -20（符号付き整数）"
  data-rank    = "S / A / B / C"
  data-comment = "一行メモ（40字以内）"
  data-check   = "true または false"
  style="display:none">
</div>
```

**ルール**
- `data-pips` は必ず符号付き整数（`+10`、`-20`）
- `data-check` は要注意トレードのみ `"true"`、通常は `"false"`
- `data-comment` はダブルクォート内にそのまま記述（HTMLエスケープ不要）

---

### 2. スティッキーヘッダー

```html
<header class="top-header">
  <div class="top-header-inner">
    <h1>📅 YYYY-MM-DD &nbsp;<span style="color:var(--text-muted);font-weight:400;font-size:14px;">通貨ペア</span></h1>
    <a href="../インデックスファイル名.html" class="rule-link">← トップへ戻る</a>
  </div>
</header>
```

---

### 3. トレード情報バー（カード内・最上部）

カードとして出力。`style="padding:0;"` を忘れずに。

```html
<div class="card" style="padding:0;">
  <div class="trade-info-bar">

    <div class="info-item">
      <span class="info-label">通貨ペア</span>
      <span class="info-value">USD/CAD</span>
    </div>
    <div class="sep"></div>

    <!-- 方向：買いは class="buy"、売りは class="sell" -->
    <div class="info-item">
      <span class="info-label">方向</span>
      <span class="info-value buy">▲ 買い</span>
      <!-- または -->
      <span class="info-value sell">▼ 売り</span>
    </div>
    <div class="sep"></div>

    <div class="info-item">
      <span class="info-label">ロット</span>
      <span class="info-value">0.01</span>
    </div>
    <div class="sep"></div>

    <div class="info-item">
      <span class="info-label">エントリー</span>
      <span class="info-value">1.36497</span>
    </div>
    <div class="sep"></div>

    <div class="info-item">
      <span class="info-label">決済</span>
      <span class="info-value">1.36397</span>
    </div>
    <div class="sep"></div>

    <!-- 損切りは常に color:var(--red) -->
    <div class="info-item">
      <span class="info-label">損切り</span>
      <span class="info-value" style="color:var(--red);">1.36697</span>
    </div>
    <div class="sep"></div>

    <!-- 損益：プラスは class="plus"、マイナスは class="minus" -->
    <div class="info-item">
      <span class="info-label">損益</span>
      <span class="info-value plus">+10 pips</span>
      <!-- または -->
      <span class="info-value minus">−20 pips</span>
    </div>
    <div class="sep"></div>

    <div class="info-item">
      <span class="info-label">時間</span>
      <span class="info-value" style="font-size:13px;">22:02 → 22:45</span>
    </div>
    <div class="sep"></div>

    <div class="info-item">
      <span class="info-label">市場</span>
      <span class="info-value" style="font-size:13px; color:var(--yellow);">NY時間</span>
    </div>

  </div>
</div>
```

**ルール**
- 各項目の間には必ず `<div class="sep"></div>` を挿入
- 最後の項目の後には `sep` は不要
- マイナス記号は `−`（全角マイナス）を使用（`-` ではなく）

---

### 4. 分析チェックカード

カテゴリ列・項目列・評価列の3列テーブルを使用。
同一カテゴリの複数行は `rowspan` で結合する。

```html
<div class="card">
  <p class="card-title">✅ 分析チェック</p>
  <div class="table-wrap">
    <table class="check-table">
      <thead>
        <tr>
          <th>カテゴリ</th>
          <th>項目</th>
          <th>評価</th>
        </tr>
      </thead>
      <tbody>
        <!-- 同カテゴリ2行の例 -->
        <tr>
          <td rowspan="2" style="color:var(--text-muted); font-size:12px; border-right:1px solid var(--border-sub);">環境認識</td>
          <td>トレンド方向（下降）</td>
          <td class="text-ok">OK</td>
        </tr>
        <tr>
          <td>MA位置（戻り確認）</td>
          <td class="text-ok">OK</td>
        </tr>

        <!-- 同カテゴリ3行の例 -->
        <tr>
          <td rowspan="3" style="color:var(--text-muted); font-size:12px; border-right:1px solid var(--border-sub);">インジケーター</td>
          <td>ストキャス（デッドクロス）</td>
          <td class="text-ok">OK</td>
        </tr>
        <tr>
          <td>MACD（下降方向）</td>
          <td class="text-ok">OK</td>
        </tr>
        <tr>
          <td>クロス確認</td>
          <td class="text-warn">△</td>
        </tr>

        <!-- 単独カテゴリ（rowspan不要）の例 -->
        <tr>
          <td rowspan="1" style="color:var(--text-muted); font-size:12px; border-right:1px solid var(--border-sub);">エントリー</td>
          <td>エントリータイミング</td>
          <td class="text-ng">NG</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>
```

**評価クラス対応表**

| 評価 | クラス | 用途 |
|------|--------|------|
| OK | `class="text-ok"` | 条件クリア |
| △ | `class="text-warn"` | 軽微な問題・要注意 |
| NG | `class="text-ng"` | 条件未達・ルール違反 |

**ルール**
- `rowspan` の値 = 同カテゴリの行数（必ず一致させる）
- カテゴリ列のスタイルは常に `style="color:var(--text-muted); font-size:12px; border-right:1px solid var(--border-sub);"` を適用
- 単独行でも `rowspan="1"` を明示する

---

### 5. 結果カード

```html
<div class="card">
  <p class="card-title">📊 結果</p>
  <div class="result-row">
    <div class="result-item">
      <label>損益 Pips</label>
      <!-- プラスは class="plus"、マイナスは class="minus" -->
      <span class="big-value plus">+10</span>
    </div>
    <div class="result-item">
      <label>総合評価</label>
      <!-- ランクに応じたクラスを使用 -->
      <span class="big-value rank-a">A</span>
    </div>
  </div>
</div>
```

**ランククラス対応表**

| ランク | クラス |
|--------|--------|
| S | `rank-s` |
| A | `rank-a` |
| B | `rank-b` |
| C | `rank-c` |

---

### 6. 評価基準カード（固定・毎回出力）

内容は変更しない。全ファイルで同一テキストを使用する。

```html
<div class="card">
  <p class="card-title">🏅 評価基準（ランク定義）</p>
  <div class="rank-legend">
    <div class="rank-legend-item">
      <div class="rank-head rank-s">S</div>
      <div class="rank-desc">
        ルール完全遵守<br>
        勝ち or 想定内の負け<br>
        <span style="color:var(--green);">最高評価</span>
      </div>
    </div>
    <div class="rank-legend-item">
      <div class="rank-head rank-a">A</div>
      <div class="rank-desc">
        ルール遵守<br>
        軽微ミスあり<br>
        <span style="color:var(--blue);">良好</span>
      </div>
    </div>
    <div class="rank-legend-item">
      <div class="rank-head rank-b">B</div>
      <div class="rank-desc">
        一部ルール違反<br>
        結果は勝ち<br>
        <span style="color:var(--yellow);">要改善</span>
      </div>
    </div>
    <div class="rank-legend-item">
      <div class="rank-head rank-c">C</div>
      <div class="rank-desc">
        ルール違反あり<br>
        負け or 大きなミス<br>
        <span style="color:var(--red);">反省必須</span>
      </div>
    </div>
  </div>
  <p style="margin:12px 0 0; font-size:12px; color:var(--text-muted);">
    ※勝敗ではなくルール遵守度で評価。S・Aは「プロセス正解」としてカウント。
  </p>
</div>
```

---

### 7. 感情ログカード

```html
<div class="card">
  <p class="card-title">🧠 感情ログ <span style="font-size:11px; color:var(--text-muted); font-weight:400; text-transform:none; letter-spacing:0;">（1=冷静 ／ 5=パニック）</span></p>
  <ul class="emotion-log">
    <li>
      <span class="phase">エントリー前</span>
      <span class="score">1</span>
    </li>
    <li>
      <span class="phase">保有中</span>
      <span class="score">2</span>
    </li>
    <li>
      <span class="phase">決済後</span>
      <span class="score">1</span>
    </li>
  </ul>
</div>
```

**ルール**
- `<span class="phase">` にフェーズ名、`<span class="score">` に数値（1〜5の整数）
- フェーズは常に「エントリー前」「保有中」「決済後」の3項目固定

---

### 8. 振り返りメモカード

セクション構成は**勝ちトレードと負けトレードで切り替え**る。

```html
<div class="card">
  <p class="card-title">📝 振り返りメモ</p>
  <div class="memo-block">

    <section>
      <div class="memo-heading">今回の本質</div>
      <p>（トレードの本質・判断の根拠を簡潔に記述）</p>
    </section>

    <!-- 勝ちトレード（ランクS/A/B）の場合 -->
    <section>
      <div class="memo-heading">改善点</div>
      <p>（軽微な問題点や次回への改善事項）</p>
    </section>

    <!-- 負けトレード（ランクC）の場合は上記を下記に差し替え -->
    <section>
      <div class="memo-heading">負けの原因</div>
      <p>（箇条書き可。原因を具体的に記述）</p>
    </section>

    <section>
      <div class="memo-heading">改善ルール</div>
      <p>（次回から適用するルールを箇条書きで記述）</p>
    </section>

    <section>
      <div class="memo-heading">一言</div>
      <p>（一文でトレードを総括）</p>
    </section>

  </div>
</div>
```

**ルール**
- 箇条書きは `・` で始まる行を `<br>` 区切りで記述（`<ul>` は使用しない）
- 改行は `<br>` を使用。段落間に空白が必要な場合は `<br><br>`
- セクション数は4固定。見出し名のみ勝ち/負けで変わる

---

### 9. チャートカード（固定・毎回出力）

```html
<div class="card">
  <p class="card-title">📷 チャート</p>
  <div class="chart-grid">
    <div class="chart-item">
      <label>日足 — 方向認識</label>
      <img src="../images/YYYYMMDD_day.png" alt="日足チャート"
           onerror="this.closest('.chart-item').style.display='none'">
    </div>
    <div class="chart-item">
      <label>1時間足 — エントリー根拠</label>
      <img src="../images/YYYYMMDD_hour.png" alt="1時間足チャート"
           onerror="this.closest('.chart-item').style.display='none'">
    </div>
  </div>
</div>
```

**ルール**
- 画像パスは `../images/` 固定。ファイル名は入力データから取得
- `onerror` 属性は必ず付与する（画像未配置時の表示崩れ防止）

---

### カード出力順序（固定）

| 順番 | カード | 備考 |
|------|--------|------|
| 1 | トレード情報バー | `style="padding:0;"` 必須 |
| 2 | 分析チェック | カテゴリ3列テーブル |
| 3 | 結果 | |
| 4 | 評価基準 | 固定テキスト・毎回出力 |
| 5 | 感情ログ | |
| 6 | 振り返りメモ | |
| 7 | チャート | 毎回出力・最下部固定 |

---

## 【禁止事項】

- `<form>` タグの使用禁止
- `style.css` 以外の外部CSSを追加しない
- インラインスタイルの新規追加は最小限に留める（仕様に定義があるもののみ）
- 仕様に存在しないクラス名を使用しない
- `data-pips` に小数点を使用しない（整数のみ）
- マイナス符号は `−`（U+2212）を使用し、半角ハイフン `-` を損益表示に使用しない

---

## 【出力チェックリスト】

AIは出力前に以下を自己確認すること。

- [ ] `data-check` が `"true"` または `"false"` になっている
- [ ] `data-pips` が符号付き整数になっている
- [ ] 方向に応じて `buy` / `sell` クラスが正しく設定されている
- [ ] 損益の符号と `plus` / `minus` クラスが一致している
- [ ] `rowspan` の値とカテゴリの実際の行数が一致している
- [ ] 評価基準カードが固定テキストで出力されている
- [ ] 感情ログが3フェーズ固定で出力されている
- [ ] 振り返りメモが4セクション構成になっている
- [ ] チャート画像パスの日付部分が正しい（`YYYYMMDD` 形式）
- [ ] カード出力順序が仕様通りになっている
