# CLAUDE.md

このファイルは、このリポジトリでコードを扱う際のClaude Code（claude.ai/code）へのガイダンスを提供します。

## プロジェクト概要

infohirokiWebsiteは、vanilla HTML/CSS/JavaScriptで構築された静的ウェブサイトで、GitHub Pagesでinfohiroki.comとしてホストされています。ブログシステムを統合した個人/プロフェッショナルサイトです。

## アーキテクチャ

### コア構造
- **静的サイト**: バックエンドなし、ビルドプロセスなし、フレームワークなし
- **ブログシステム**: カスタムJavaScriptベースの記事管理
  - 記事は`html-files/`ディレクトリに保存
  - メタデータは`files.json`で管理
  - `auto-scan.js`による動的読み込み
- **レスポンシブデザイン**: 専用モバイルナビゲーション付きのモバイルファーストアプローチ

### 主要ファイル
- `files.json`: 全ブログ記事のメタデータ中央リポジトリ
- `js/main.js`: コア機能（ナビゲーション、クリップボード、モバイルメニュー）
- `js/auto-scan.js`: ブログ記事の読み込みと検索機能
- `css/style.css`: CSS変数を使用した統一デザインシステム

## コマンド

### 開発
```bash
# ローカル開発サーバーの起動（いずれか選択）
python -m http.server 8000
# または
python3 -m http.server 8000
# または
npx http-server

# ビルド、リント、テストコマンドなし - 静的サイトのため
```

### Git操作
```bash
# GitHub Pagesへのデプロイ（mainへのpushで自動）
git add .
git commit -m "コミットメッセージ"
git push origin main
```

## ブログ記事管理

### 新規記事の追加
1. 命名規則に従って`html-files/`にHTMLファイルを作成：
   ```
   YYYY-MM-DD-category-description.html
   ```
   カテゴリ: `system`, `edu`, `tech`, `design`, `biz`

2. `files.json`に記事メタデータを追加：
   ```json
   {
     "id": "ファイル名の説明部分",
     "path": "YYYY-MM-DD-category-description.html",
     "title": "記事タイトル",
     "description": "簡潔な説明（50-100文字）",
     "tags": ["タグ1", "タグ2"],
     "created": "YYYY-MM-DD"
   }
   ```

### ファイル命名規則
- 日付形式: `YYYY-MM-DD`（ゼロパディング必須）
- カテゴリ: 事前定義されたカテゴリのみ使用
- 説明: 英語/ローマ字、ハイフン区切り、20文字以内推奨
- 例: `2025-06-28-tech-ai-seo-strategy.html`

## CSS変数とテーマ設定

サイトは一貫したテーマのためにCSSカスタムプロパティを使用：
```css
--color-primary: #2c3e50;
--color-accent: #3498db;
--color-text: #333;
--color-bg: #f4f6f8;
```

## モバイルレスポンシブ

- ブレークポイント: 768px
- モバイルメニューはハンバーガーパターンを使用
- サイドバーはモバイルでオーバーレイに変換

## SEOとパフォーマンス

- 各ページにメタタグ、OGP、構造化データを含む
- 画像はレイアウトの安定性のため明示的なwidth/heightを使用
- CSSは適切な場所でGPUアクセラレーションを使用

## GitHub Pagesデプロイメント

- ドメイン: infohiroki.com（CNAMEファイルで設定）
- デプロイ: mainブランチへのpushで自動
- CI/CDパイプライン不要

## 重要な注意事項

1. **パッケージ管理なし**: npm/node依存関係のない純粋な静的サイト
2. **直接ファイル編集**: すべての変更はHTML/CSS/JSファイルに直接行う
3. **ブログシステム**: 記事追加時は必ずHTMLファイルと`files.json`の両方を更新
4. **テスト**: push前にシンプルなHTTPサーバーでローカルテスト
5. **ブラウザ互換性**: CSS GridとFlexboxをサポートする最新ブラウザを対象