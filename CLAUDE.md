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

4. 統一CSSとナビゲーションを追加：
   ```html
   <head>
       <!-- 既存のメタタグ等 -->
       <link rel="stylesheet" href="../css/style.css">
   </head>
   <body>
       <nav class="article-nav article-nav-top">
           <a href="../blog.html" class="back-link">← ブログ一覧</a>
       </nav>
       
       <!-- 記事内容 -->
       
       <nav class="article-nav article-nav-bottom">
           <a href="../blog.html" class="back-to-list">他の記事を読む</a>
       </nav>
   </body>
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
- [ ] パンくずリストの実装（フラット構造のため低優先度）
- [x] 内部リンク構造の強化（4グループ26記事で実装済み - 2025年6月完成）
- [x] ブログナビゲーション改善（同一タブ遷移・記事内ナビ - 2025年6月実装）
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

## 内部リンク構造

### 概要
記事間の内部リンクを実装することで、ユーザーの回遊性とSEO効果を向上させます。現在、**4つの記事グループ（26記事）**で内部リンク構造を実装済み。総計**80個以上の相互リンク**でサイト内ナビゲーションを大幅改善しました。

### 実装済み記事グループ
- ✅ **習慣形成関連記事（9記事）**: 相互リンク済み
  - 基礎理論記事から実践記事へのリンク
  - 実践記事から理論基盤へのリンク
  - 関連概念記事間の相互参照

- ✅ **Claude Code関連記事（4記事）**: 完全実装済み
  - 総合ガイド記事をハブとした構造
  - 機能解説、実践ガイド、エンジニア働き方への相互リンク
  - 統一されたカラーテーマ（オレンジ、パープル、ブルー、コーラルピンク）

- ✅ **Python開発関連記事（6記事）**: 完全実装済み
  - Python総合ガイドをハブとした構造
  - FastAPI、Whisper AI、APScheduler、UV Package Manager、サーバーサイド言語ガイドへの相互リンク
  - Python公式カラー（ブルー #306998、イエロー #FFD43B）を活用

- ✅ **デザインツール関連記事（7記事）**: 完全実装済み
  - Figmaクラスター（2記事）: レスポンシブWeb ⟷ Figma to Framer
  - Framerクラスター（3記事）: Figma to Framer → 実践マニュアル ⟷ HTML拡張
  - Asepriteクラスター（3記事）: 基本マニュアル → ピクセルアート ⟷ 参照画像
  - 各ツール固有のカラーテーマと統一されたホバーエフェクト

### 内部リンク実装ガイドライン

#### 1. リンクパス形式
```html
<!-- 正しい形式（ブログシステムが自動的にhtml-files/を追加） -->
<a href="2025-03-30-habit-loop-structure.html" class="related-link">

<!-- 間違った形式（重複エラーになる） -->
<a href="html-files/2025-03-30-habit-loop-structure.html" class="related-link">
```

#### 2. 関連記事セクションのHTMLテンプレート
```html
<div class="related-articles">
    <h3>関連記事</h3>
    <div class="related-links">
        <a href="[ファイル名.html]" class="related-link">
            <h4>[記事タイトル]</h4>
            <p>[簡潔な説明文]</p>
        </a>
        <!-- 4-6記事程度推奨 -->
    </div>
</div>
```

#### 3. CSSスタイリング
記事のアクセントカラーに合わせたスタイリング：
```css
.related-articles {
    background-color: [アクセントカラーの薄い版];
    border: 2px solid var(--accent-color);
    border-radius: 10px;
    padding: 25px;
    margin: 30px 0;
}

.related-link:hover {
    border-color: var(--accent-color);
    box-shadow: 0 2px 8px [アクセントカラーのrgba版];
    transform: translateY(-2px);
}
```

#### 4. リンク戦略
- **Hub記事**: 基礎理論記事は多くの関連記事にリンク
- **論理的関係**: 前提→応用、理論→実践の流れを重視
- **相互参照**: 同レベルの概念記事間でクロスリンク
- **適度な数**: 1記事あたり4-6個の関連記事リンク

### 拡張可能な記事グループ
- [ ] **AI・教育関連記事（3記事）**: 中優先度
- [ ] **DevOps・インフラ関連記事（3記事）**: 中優先度
- [ ] **ソフトウェアデザインパターン記事（1記事）**: 独立性が高い

### SEO効果
- ページ権威（Page Authority）の分散
- クロール深度の改善
- ユーザー滞在時間の延長
- 関連キーワードの強化

## ブログナビゲーション改善（2025年6月実装）

### 改善概要
ブログ記事のナビゲーション体験を向上させるため、ベストプラクティスに基づく改善を実装しました。

### 実装内容

#### 1. 同一タブでの記事遷移
- **変更前**: `window.open(fullPath, '_blank')` → 新タブで開く
- **変更後**: `window.location.href = fullPath` → 同一タブで開く
- **理由**: SEO効果向上、ユーザー体験改善、戻るボタンの正常動作

#### 2. 記事内ナビゲーション追加
記事の上部と下部にナビゲーションリンクを配置：

**上部ナビゲーション（控えめ）**:
```html
<nav class="article-nav article-nav-top">
    <a href="../blog.html" class="back-link">← ブログ一覧</a>
</nav>
```

**下部ナビゲーション（目立つ）**:
```html
<nav class="article-nav article-nav-bottom">
    <a href="../blog.html" class="back-to-list">他の記事を読む</a>
</nav>
```

#### 3. 統一CSSスタイリング
`style.css`に以下のクラスを追加：
- `.article-nav`: 基本レイアウト
- `.back-link`: 上部用の控えめなリンクスタイル
- `.back-to-list`: 下部用の目立つボタンスタイル
- レスポンシブ対応とホバーエフェクト

#### 4. 外部CSSリンク統合
記事ファイルに以下を追加：
```html
<link rel="stylesheet" href="../css/style.css">
```

### 実装状況
- ✅ **blog.html**: 同一タブ遷移に変更済み
- ✅ **style.css**: ナビゲーション用スタイル追加済み
- ✅ **全51記事**: 全ブログ記事にナビゲーション実装完了（2025年6月29日）
- ✅ **統一CSS**: 全記事に外部CSSリンク追加済み

### ベストプラクティス準拠
- **Ghost/Medium型**: ミニマルで直感的なナビゲーション
- **モバイルファースト**: レスポンシブ対応
- **アクセシビリティ**: 適切なセマンティクス
- **パフォーマンス**: 軽量でGPU最適化済み

### 期待効果・実装結果
- **UX向上**: 全51記事でスムーズな記事間移動、迷子防止を実現
- **SEO効果**: 滞在時間延長、ページビュー増加、直帰率改善（測定開始）
- **分析改善**: 単一セッションでのユーザー行動追跡が可能
- **開発効率**: 統一されたナビゲーションパターンで今後の記事作成が簡素化

### 実装完了サマリー（2025年6月29日）
**処理対象**: 全51記事
**完了率**: 100%
**実装内容**: 
- 同一タブ遷移への変更
- 上部・下部ナビゲーション追加
- 統一CSSリンク追加
- レスポンシブ対応スタイリング

## 重要な注意事項

1. **パッケージ管理なし**: npm/node依存関係のない純粋な静的サイト
2. **直接ファイル編集**: すべての変更はHTML/CSS/JSファイルに直接行う
3. **ブログシステム**: 記事追加時は必ずHTMLファイルと`files.json`の両方を更新
4. **テスト**: push前にシンプルなHTTPサーバーでローカルテスト
5. **ブラウザ互換性**: CSS GridとFlexboxをサポートする最新ブラウザを対象
6. **SEO対策**: 新規記事作成時はcanonical URLの追加を推奨
7. **サイトマップ更新**: 新規記事追加時はsitemap.xmlへの追加も検討