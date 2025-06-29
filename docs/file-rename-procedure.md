# HTMLファイル名変更手順書

## 概要
この手順書は、infohirokiWebsiteプロジェクトにおいて、不適切な名前のHTMLファイルを命名規則に従って変更する手順を説明します。

## 命名規則
### フォーマット
```
{date}-{content-description}.html
```

### 例
```
2025-06-28-ai-seo-strategy.html
```

## 手順

### 1. 現在のファイル内容を確認
```bash
# ファイルの内容を確認して、適切な内容説明を考える
cat "html-files/code (6).html" | head -50
```

タイトルタグ（`<title>`）を確認して、記事の内容を把握します。カテゴリは不要になりましたので、内容を表す説明のみを考えます。

### 2. ファイルの作成日を取得
```bash
# macOSの場合
stat -f "%m" "html-files/code (6).html" | xargs -I {} date -r {} "+%Y-%m-%d"

# Linuxの場合
stat -c "%Y" "html-files/code (6).html" | xargs -I {} date -d "@{}" "+%Y-%m-%d"
```

### 3. 新しいファイル名を決定
以下の要素を組み合わせて新しいファイル名を作成：
- **日付**: YYYY-MM-DD形式（ステップ2で取得）
- **内容説明**: 
  - 英語またはローマ字
  - ハイフンで単語を区切る
  - 20文字以内推奨
  - 内容が明確に分かる説明

### 4. ファイル名を変更
```bash
# ファイル名を変更
mv "html-files/code (6).html" "html-files/2025-06-28-ai-seo-strategy.html"
```

### 5. files.jsonを更新
`files.json`に新しいエントリを追加：

```json
{
  "id": "ai-seo-strategy",
  "path": "2025-06-28-ai-seo-strategy.html",
  "title": "AI時代のSEO戦略",
  "description": "AI技術の進化に対応したSEO戦略とその実践方法",
  "tags": [
    "SEO",
    "AI",
    "マーケティング",
    "検索エンジン最適化",
    "戦略"
  ],
  "created": "2025-06-28"
}
```

#### files.json更新時の注意点：
- **id**: ファイル名の内容説明部分を使用（ハイフンはそのまま）
- **path**: 新しいファイル名
- **title**: HTMLファイルの`<title>`タグの内容
- **description**: 記事の要約（50-100文字程度）
- **tags**: 関連するキーワードを5つ程度
- **created**: ファイル名の日付部分

### 6. 変更の確認
```bash
# ファイルが正しく移動されたか確認
ls -la html-files/2025-06-28-ai-seo-strategy.html

# files.jsonが正しく更新されたか確認
grep "ai-seo-strategy" files.json
```

## チェックリスト
- [ ] ファイルの内容を確認し、適切な内容説明を考えた
- [ ] ファイルの作成日または更新日を取得した
- [ ] 命名規則に従った新しいファイル名を決定した
- [ ] ファイル名を変更した
- [ ] files.jsonに新しいエントリを追加した
- [ ] idは内容説明部分と一致している
- [ ] タグは適切に設定されている
- [ ] 変更後のファイルとJSONエントリを確認した

## よくある間違い
1. **日付形式の誤り**: 必ず`YYYY-MM-DD`形式を使用（ゼロパディング必須）
2. **内容説明の不明瞭さ**: 記事内容がわかるクリアな説明を使用
3. **アンダースコアの使用**: ハイフン（-）を使用し、アンダースコア（_）は使わない
4. **日本語の使用**: ファイル名には英語またはローマ字のみを使用
5. **files.jsonの更新忘れ**: ファイル名変更後は必ずfiles.jsonも更新する

## 関連ドキュメント
- [命名規則ガイド](naming-convention.md)
- [ブログシステム概要](blog-system-overview.md)