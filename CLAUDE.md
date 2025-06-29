# CLAUDE.md

このファイルは、このリポジトリでコードを扱う際のClaude Code（claude.ai/code）へのガイダンスを提供します。

## プロジェクト概要

infohirokiWebsiteは、vanilla HTML/CSS/JavaScriptで構築された静的ウェブサイトで、Vercelでinfohiroki.comとしてホストされています。ブログシステムを統合した個人/プロフェッショナルサイトです。

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
- `robots.txt`: 検索エンジンクロール制御
- `sitemap.xml`: サイト構造をGoogleに通知

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
# Vercelへのデプロイ（mainへのpushで自動）
git add .
git commit -m "コミットメッセージ"
git push origin main
```

## ブログ記事管理

### 新規記事の追加
1. 命名規則に従って`html-files/`にHTMLファイルを作成：
   ```
   YYYY-MM-DD-description.html
   ```

2. `files.json`に記事メタデータを追加：
   ```json
   {
     "id": "ファイル名の説明部分",
     "path": "YYYY-MM-DD-description.html",
     "title": "記事タイトル",
     "description": "簡潔な説明（50-100文字）",
     "tags": ["タグ1", "タグ2"],
     "created": "YYYY-MM-DD"
   }
   ```

3. SEO対策のcanonical URLを追加（推奨）：
   ```html
   <link rel="canonical" href="https://infohiroki.com/html-files/YYYY-MM-DD-description.html">
   ```

### ファイル命名規則
- 日付形式: `YYYY-MM-DD`（ゼロパディング必須）
- 説明: 英語/ローマ字、ハイフン区切り、20文字以内推奨
- 例: `2025-06-28-ai-seo-strategy.html`

## CSS変数とテーマ設定

サイトは一貫したテーマのためにCSSカスタムプロパティを使用：
```css
--color-text: #000000;        /* 純黒 - メインテキスト */
--color-text-light: #666666;  /* ミディアムグレー - 副次テキスト */
--color-background: #FFFFFF;  /* 純白 - 背景 */
--color-border: #CCCCCC;      /* ライトグレー - ボーダー */
--color-accent: #E73E8F;      /* ピンク - アクセントカラー */
```

## モバイルレスポンシブ

- ブレークポイント: 768px
- モバイルメニューはハンバーガーパターンを使用
- サイドバーはモバイルでオーバーレイに変換

## SEO・AIO・LLMO対策

### 実装済み対策
- ✅ 各ページにメタタグ、OGP、構造化データを含む
- ✅ robots.txt（検索エンジンクロール制御）
- ✅ sitemap.xml（全ページのURL一覧）
- ✅ 重要記事にcanonical URL設定済み
- ✅ レスポンシブデザイン（モバイルファースト）
- ✅ 高品質なコンテンツ（技術記事・実用的ガイド）

### パフォーマンス最適化
- 画像はレイアウトの安定性のため明示的なwidth/heightを使用
- CSSは適切な場所でGPUアクセラレーションを使用

### 今後の改善点
- [ ] 残り記事へのcanonical URL追加
- [ ] パンくずリストの実装
- [ ] 内部リンク構造の強化
- [ ] E-A-T（専門性・権威性・信頼性）の強化

## Vercelデプロイメント

- ドメイン: infohiroki.com（Vercelダッシュボードで設定）
- デプロイ: mainブランチへのpushで自動
- SSL証明書の自動発行・更新
- CI/CDパイプライン不要

## スキルスタックのアイコン表示

about.html（スキルスタックページ）では、技術スタックのアイコンを2つの方法で表示：

### 1. Skill Icons (推奨)
```html
<img src="https://skillicons.dev/icons?i=react" class="tech-icon" alt="React">
```
利用可能なアイコン: https://skillicons.dev/
- 言語: python, js, ts, go, java, rust, cpp など
- フレームワーク: react, nextjs, vue, django, fastapi など
- ツール: docker, k8s, git, github, vscode など

### 2. 絵文字アイコン
```html
<span class="tech-icon">🤖</span>
```
抽象的な概念や、Skill Iconsにないものに使用

### スタイル定義
```css
.tech-icon {
    width: 32px;
    height: 32px;
    margin-right: 12px;
    vertical-align: middle;
}
```

## 重要な注意事項

1. **パッケージ管理なし**: npm/node依存関係のない純粋な静的サイト
2. **直接ファイル編集**: すべての変更はHTML/CSS/JSファイルに直接行う
3. **ブログシステム**: 記事追加時は必ずHTMLファイルと`files.json`の両方を更新
4. **テスト**: push前にシンプルなHTTPサーバーでローカルテスト
5. **ブラウザ互換性**: CSS GridとFlexboxをサポートする最新ブラウザを対象
6. **SEO対策**: 新規記事作成時はcanonical URLの追加を推奨
7. **サイトマップ更新**: 新規記事追加時はsitemap.xmlへの追加も検討