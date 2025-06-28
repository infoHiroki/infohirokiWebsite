# ファイル命名規則ガイド - infoHiroki ブログ

## 📋 概要

infoHirokiブログのHTMLファイル統一管理を目的とした命名規則です。

## 🎯 基本パターン

```
{日付}-{カテゴリ}-{内容説明}.html
```

### 例
- `2025-06-26-tech-go-tutorial.html`
- `2025-12-15-system-tax-reform.html`
- `2026-01-10-design-ui-guide.html`

## 📅 日付形式

**必須**: `YYYY-MM-DD`

- 年は4桁
- 月・日は2桁（ゼロパディング）
- 作成日または主要更新日を使用

## 🏷️ カテゴリコード

| カテゴリ | コード | 説明 | 例 |
|---------|-------|------|-----|
| 制度・補助金 | `system` | 政府制度、補助金、法制度 | `system-education-training` |
| 教育・AI | `edu` | 教育、AI、学習コンテンツ | `edu-ai-summary` |
| 技術・開発 | `tech` | プログラミング、開発技術 | `tech-go-tutorial` |
| デザイン | `design` | UI/UX、グラフィック | `design-color-guide` |
| ビジネス | `biz` | 経営、マーケティング | `biz-strategy-plan` |

## 📝 内容説明部分

### 命名ルール
1. **英語またはローマ字を使用**
2. **単語間はハイフン（-）で区切り**
3. **簡潔で分かりやすく**
4. **20文字以内を推奨**

### 良い例
- `go-tutorial` （Go言語チュートリアル）
- `claude-code` （Claude Code関連）
- `llmo-checklist` （LLMOチェックリスト）
- `education-training` （教育訓練制度）

### 避けるべき例
- `GoLanguageTutorialForBeginners` （長すぎる）
- `go_tutorial` （アンダースコア使用）
- `Go言語` （日本語使用）
- `tutorial123` （無意味な数字）

## 🛠️ 実装ガイド

### 新規ファイル作成時

1. **命名規則に従ってファイル名を決定**
2. **`files.json`に対応するエントリを追加**
3. **blog.htmlで表示確認**

### ファイル名生成例

```javascript
// 現在の日付を取得
const today = new Date().toISOString().split('T')[0]; // "2025-06-26"

// カテゴリと内容から生成
const category = "tech";
const content = "react-hooks";
const filename = `${today}-${category}-${content}.html`;
// 結果: "2025-06-26-tech-react-hooks.html"
```

## 📈 メリット

1. **時系列管理**: 日付でソートが容易
2. **カテゴリ識別**: ファイル名でカテゴリが即座に判別
3. **一貫性**: 統一された形式で管理しやすい
4. **検索性**: パターンマッチングによる検索が容易
5. **ブログ統合**: infoHirokiサイトとの統合が容易

---

**作成日**: 2025年6月28日  
**対象**: infoHiroki ブログシステム  
**バージョン**: 1.0.0