# 残りのnote記事作成タスク

XMLファイルに61記事あり、現在8記事作成済み。残り53記事の作成が必要です。

## 進行状況
- 作成済み：11記事
- 残り：50記事
- 進捗率：18%

## 優先度別タスクリスト

### 高優先度（最新記事・AI関連）
1. **ChatGPTから2年以上。実務に生き残ったAIツール4選**
   - 公開日：2024-11-20
   - カテゴリ：AI・ツール活用

2. **AI駆動開発と呼ぶらしい、開発スタイルまとめてみた**
   - 公開日：2024-12-09
   - カテゴリ：AI・開発

3. **【Cody】AI駆動開発でChrome拡張機能をつくった話**
   - 公開日：2025-01-09
   - カテゴリ：AI・開発・Chrome拡張

### 中優先度（Notion・AIシリーズ）
4. **🤖📚AIとNotionはセットでつかう？_01**
   - 公開日：2024-06-03
   - カテゴリ：AI・Notion・ワークフロー

5. **Notionってなんだ？_02**
   - 公開日：2024-06-04
   - カテゴリ：Notion・入門

6. **ChatGPTとNotionシリーズ_0**
   - 公開日：2024-06-03
   - カテゴリ：AI・Notion・日記

### 実装時の注意事項

#### 技術要件
- [ ] XMLからコンテンツを正確に抽出
- [ ] 画像パスを`/assets/`から`/images/note/`に変換
- [ ] canonical URLを各記事に追加
- [ ] レスポンシブデザインを適用

#### デザイン統一
- [ ] 記事カテゴリに応じたカラーテーマ選択
  - AI関連：青・紫グラデーション
  - Notion関連：グレー・青グラデーション
  - 開発関連：緑・青グラデーション
  - 日記・雑記：温かい色調

#### SEO対策
- [ ] メタディスクリプション（50-100文字）
- [ ] 適切なキーワード設定
- [ ] 構造化データの追加

#### ファイル管理
- [ ] ファイル名形式：`YYYY-MM-DD-description.html`
- [ ] `files.json`への記事メタデータ追加
- [ ] 画像ファイルの存在確認

### 作業フロー
1. XMLから記事データ抽出
2. HTMLテンプレート適用
3. 画像パス調整
4. デザイン・レスポンシブ対応
5. SEO最適化
6. `files.json`更新
7. 動作確認

### 推定作業時間
- 1記事あたり：10-15分
- 残り53記事：8-13時間
- バッチ処理による効率化可能

### 次のアクション
最優先として上記3記事（ChatGPTツール、AI駆動開発、Cody Chrome拡張）の作成から開始し、その後Notion関連シリーズを継続する。

## 重要な実装詳細

### ソースファイル
- **XMLファイル**: `/Users/hirokitakamura/Documents/Dev/infohirokiWebsite/note/note-hirokitakamura-1.xml`
- **画像フォルダ**: `/Users/hirokitakamura/Documents/Dev/infohirokiWebsite/images/note/`
- **メタデータ**: `/Users/hirokitakamura/Documents/Dev/infohirokiWebsite/files.json`

### 既存テンプレート参考
- **note記事テンプレート**: `2024-05-20-gas-terminology-collection.html`
- **修正済み記事**: `2024-03-28-notion-subscription-management.html`

### コマンド例
```bash
# XMLから記事タイトル検索
grep -n "タイトル" /Users/hirokitakamura/Documents/Dev/infohirokiWebsite/note/note-hirokitakamura-1.xml

# 画像ファイル確認
ls /Users/hirokitakamura/Documents/Dev/infohirokiWebsite/images/note/
```

### HTMLテンプレート構造
```html
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>[記事タイトル]</title>
    <link rel="canonical" href="https://infohiroki.com/html-files/[ファイル名]">
    <style>
        /* カテゴリ別カラーテーマ */
        background: linear-gradient(135deg, [色1], [色2]);
    </style>
</head>
<body>
    <div class="container">
        <header>[アイコン] [タイトル]</header>
        <div class="content">[本文]</div>
        <div class="meta">
            <div>公開日: [日付]</div>
            <div class="tags">[タグ]</div>
        </div>
        <footer>note記事アーカイブ - infohiroki.com</footer>
    </div>
</body>
</html>
```

### 完了済み記事リスト
1. ✅ 2024-03-27-notion-save-to-notion-extension.html
2. ✅ 2024-03-28-notion-subscription-management.html (修正済み)
3. ✅ 2024-03-29-chatgpt-reskilling-guide.html
4. ✅ 2024-04-08-book-output-reading-technique-memo.html
5. ✅ 2024-05-17-book-world-class-engineer-thinking-memo.html
6. ✅ 2024-05-19-prompt-design-guide.html
7. ✅ 2024-05-20-programming-terminology-collection.html
8. ✅ 2024-05-20-gas-terminology-collection.html

### 新規作成記事（2025年7月作成）
9. ✅ 2025-06-26-gemini-cli-vs-claude-code-review.html
   - タイトル: 「Gemini CLI」がきた。Claude CodeをMaxプランで毎日使ってる人の感想
   - カテゴリ: AI開発ツール比較・レビュー
   - 作成日: 2025年7月1日
   
10. ✅ 2025-06-07-git-github-complete-cheatsheet.html
    - タイトル: Git & GitHub 完全チートシート
    - カテゴリ: 開発ツール・リファレンス
    - 作成日: 2025年7月1日

11. ✅ 2025-06-06-cursor-1-0-new-features.html
    - タイトル: 🎉 Cursor 1.0 の主要新機能
    - カテゴリ: AI開発ツール・新機能解説
    - 作成日: 2025年7月1日
    - 特徴: 技術解説+創作小説の組み合わせ

12. ✅ 2024-12-09-ai-driven-development-summary.html
    - タイトル: AI駆動開発と呼ぶらしい、開発スタイルまとめてみた
    - カテゴリ: AI開発手法・ワークフロー解説
    - 作成日: 2025年7月1日
    - 特徴: Cursor、Claude、Dify等のツール使い分け戦略