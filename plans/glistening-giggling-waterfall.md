# 上級ワークブックのカラースキーム変更：ダーク → クールライト

## Context
上級ワークブック（claude_code_advanced.html）の現在のダークテーマ（背景 `#0d0d10`）が見にくい。
基本ワークブック（claude_workbook.html）は温かみのあるクリーム系ライトテーマなので、差別化しつつ読みやすいテーマに変更する。

## 方針：クールライト（スレート/ブルーグレー系）
- 基本ワークブック＝**温かいクリーム系** → 上級＝**冷たいブルーグレー系**で差別化
- ライトテーマにすることで読みやすさを大幅改善
- 7色のトピックカラーはライト背景に合わせて暗めに調整
- コードブロック（`.cb`）はダーク背景を維持（ターミナル風）

## 修正対象ファイル
- `claude_code_advanced.html` のみ

## 変更内容

### 1. `:root` CSS変数の全置換（L9-41）

```css
:root {
  --bg: #f0f2f7;        /* メイン背景：クールブルーホワイト */
  --bg2: #e6e9f0;       /* カードヘッド、prereqなど */
  --bg3: #dce0ea;       /* ファイルツリー背景など */
  --border: #c5c9d6;
  --border2: #a8aec0;
  --text: #1a1d2b;      /* メインテキスト：ブルー寄りの黒 */
  --muted: #5c6178;
  --dim: #8b90a5;

  --green: #1a8a52;     --green-d: #15703f;   --green-bg: #e0f5ea;
  --blue: #2563b0;      --blue-d: #1e4f8a;    --blue-bg: #e0ecf8;
  --amber: #a07010;     --amber-bg: #fdf4dc;
  --purple: #6d4dbe;    --purple-bg: #ede6fa;
  --red: #c03030;       --red-bg: #fce8e8;
  --cyan: #0e7f90;      --cyan-bg: #ddf4f8;

  --t1: #1a8a52;  --t2: #2563b0;  --t3: #6d4dbe;
  --t4: #a07010;  --t5: #0e7f90;  --t6: #c03030;  --t7: #c06020;
}
```

### 2. CSS内のハードコード色を修正

| 箇所 | 現在 | 変更後 |
|---|---|---|
| coverのh1グラデーション | `#e8e6f0 → #6b6880` | `#1a1d2b → #5c6178` |
| `.cb .cm`（コメント色） | `#4a4a62` | `#8b90a5` |
| diff before pane | `bg:#1a0e0e, color:#c07070` | `bg:#fce8e8, color:#a02020` |
| diff after pane | `color:#70c0a0` | `color:#1a7a48` |
| diff-htab before | `bg:#2a1414` | `bg:#f5d0d0` |
| `.warn` border | `#4a3810` | `#d4b870` |
| `.err` border | `#4a1818` | `#e0a0a0` |
| `.err` color | `#c07070` | `#a03030` |
| `.c7` / `bg7` | `#fb923c` / `#1e1206` | `#c06020` / `#fdf0e4` |
| `.dl-box` 系 | ハードコード暗色 | `var(--bg3)` / `var(--border2)` 等に |
| cover `::before` | `rgba(61,214,140,0.06)` | `rgba(26,138,82,0.08)` |
| scrollbar | 既にvar使用→変更不要 | — |

### 3. コードブロック（`.cb`）はダーク維持
```css
.cb {
  background: #1e2030;
  color: #d0d4e0;
  border-color: #3a3e52;
}
```
→ ターミナル風の視覚的区別を保つ（基本ワークブックの `.pblock` と同じ発想）

### 4. HTML内のインラインスタイル修正（5箇所）
- `color:#fb923c` → `color:var(--t7)`
- `border-left:3px solid #fb923c` → `border-left:3px solid var(--t7)`

### 5. `:root` に `--bg7` を追加
`--bg7: #fdf0e4;` を追加（インライン fallback に頼らない）

## 検証方法
- ブラウザで `claude_code_advanced.html` を開き以下を確認：
  - 背景がクールなライトブルーグレーになっている
  - テキストが十分なコントラストで読みやすい
  - 7色のトピックカラーがそれぞれ区別できる
  - コードブロックがダーク背景で視覚的に区別される
  - diff の before/after が赤/緑で区別できる
  - 基本ワークブック（claude_workbook.html）と並べて見た目が明確に異なる
  - レイアウト崩れがないこと
