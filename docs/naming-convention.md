# ファイル命名規則ガイド - infoHiroki ブログ

## 📋 概要

infoHirokiブログのHTMLファイル統一管理を目的とした命名規則です。

## 🎯 基本パターン

```
{日付}-{内容説明}.html
```

### 例
- `2025-06-26-go-tutorial.html`
- `2025-12-15-tax-reform-guide.html`
- `2026-01-10-ui-design-principles.html`

## 📅 日付形式

**必須**: `YYYY-MM-DD`

- 年は4桁
- 月・日は2桁（ゼロパディング）
- 作成日または主要更新日を使用


## 📝 内容説明部分

### 命名ルール
1. **英語またはローマ字を使用**
2. **単語間はハイフン（-）で区切り**
3. **簡潔で分かりやすく**
4. **20文字以内を推奨**

### 良い例
- `go-tutorial` （Go言語チュートリアル）
- `claude-code-features` （Claude Code機能紹介）
- `ai-seo-strategy` （AI時代のSEO戦略）
- `education-training-support` （教育訓練支援制度）

### 避けるべき例
- `GoLanguageTutorialForBeginners` （長すぎる）
- `go_tutorial` （アンダースコア使用）
- `Go言語` （日本語使用）
- `tutorial123` （無意味な数字）

## 🛠️ 実装ガイド

### 新規ファイル作成時

1. **命名規則に従ってファイル名を決定**
2. **`files.json`に対応するエントリを追加**
3. **SEO対策: canonical URLを追加（推奨）**
   ```html
   <link rel="canonical" href="https://infohiroki.com/html-files/YYYY-MM-DD-description.html">
   ```
4. **`sitemap.xml`への追加を検討**
5. **blog.htmlで表示確認**

### ファイル名生成例

```javascript
// 現在の日付を取得
const today = new Date().toISOString().split('T')[0]; // "2025-06-26"

// 内容から生成
const content = "react-hooks-guide";
const filename = `${today}-${content}.html`;
// 結果: "2025-06-26-react-hooks-guide.html"
```

## 📈 メリット

1. **時系列管理**: 日付でソートが容易
2. **内容の明確化**: ファイル名で記事内容が即座に判別
3. **一貫性**: 統一された形式で管理しやすい
4. **検索性**: パターンマッチングによる検索が容易
5. **ブログ統合**: infoHirokiサイトとの統合が容易
6. **SEO対策**: canonical URLで重複コンテンツ問題を回避
7. **検索エンジン最適化**: robots.txt、sitemap.xmlで適切にインデックス

---

**作成日**: 2025年6月28日  
**更新日**: 2025年6月29日  
**対象**: infoHiroki ブログシステム  
**バージョン**: 2.1.0  
**変更内容**: SEO対策（canonical URL、sitemap.xml）の手順を追加