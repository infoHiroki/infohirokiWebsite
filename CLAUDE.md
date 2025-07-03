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
  - `blog-loader.js`による動的読み込み
- **レスポンシブデザイン**: 専用モバイルナビゲーション付きのモバイルファーストアプローチ

### 主要ファイル
- `files.json`: 全ブログ記事のメタデータ中央リポジトリ
- `blog.html`: ブログ一覧ページ（検索・フィルタリング機能付き）
- `blog-loader.js`: ブログ記事の読み込みと管理機能
- `js/main.js`: コア機能（ナビゲーション、クリップボード、モバイルメニュー）
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
git commit -m "絵文字付きコミットメッセージ"
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

4. 統一CSSとナビゲーションを追加（必須）：
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

**注意**: 2025年6月29日以降の新規記事では、上記のナビゲーション要素とCSSリンクが必須となります。統一されたユーザー体験の維持のため、必ず含めてください。

### ファイル命名規則

#### 基本パターン
```
{日付}-{内容説明}.html
```

#### 日付形式
- **必須**: `YYYY-MM-DD`（ゼロパディング必須）
- 年は4桁、月・日は2桁
- 作成日または主要更新日を使用

#### 内容説明部分
1. **英語またはローマ字を使用**
2. **単語間はハイフン（-）で区切り**
3. **簡潔で分かりやすく**
4. **20文字以内を推奨**

#### 良い例
- `2025-06-28-ai-seo-strategy.html`（AI時代のSEO戦略）
- `2025-06-26-go-tutorial.html`（Go言語チュートリアル）
- `2025-05-24-claude-code-features.html`（Claude Code機能紹介）

#### 避けるべき例
- `GoLanguageTutorialForBeginners.html`（長すぎる）
- `go_tutorial.html`（アンダースコア使用）
- `Go言語.html`（日本語使用）
- `tutorial123.html`（無意味な数字）

#### ファイル名生成例
```javascript
// 現在の日付を取得
const today = new Date().toISOString().split('T')[0]; // "2025-06-26"

// 内容から生成
const content = "react-hooks-guide";
const filename = `${today}-${content}.html`;
// 結果: "2025-06-26-react-hooks-guide.html"
```

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

## ブログシステム技術仕様

### 動的コンテンツ管理
- ✅ **blog-loader.js**: HTMLFileScanner + AutoFileManagerによる記事読み込み
- ✅ **files.json**: ブログ記事のメタデータ一元管理
- ✅ **動的読み込み**: Promise.allによる並行メタデータ抽出
- ✅ **フォールバック機能**: blog-loader.js失敗時のハードコードリスト使用

### 検索・フィルタリング機能
- ✅ **リアルタイム検索**: 300msデバウンス、タイトル・説明・タグ全文検索
- ✅ **ソート機能**: 日付順・タイトル順切り替え
- ✅ **レスポンシブグリッド**: デスクトップ3列→タブレット2列→モバイル1列
- ✅ **パフォーマンス**: GPU加速とCSS Grid最適化

### デバッグ・保守機能
- ✅ **コンソールログ**: 詳細な読み込み状況表示
- ✅ **エラーハンドリング**: 複数レベルのフォールバック
- ✅ **パス正規化**: html-files/プレフィックス自動補完

## SEO・AIO・LLMO対策

### 実装済み対策
- ✅ 各ページにメタタグ、OGP、構造化データを含む
- ✅ robots.txt（検索エンジンクロール制御）
- ✅ sitemap.xml（全ページのURL一覧）
- ✅ 重要記事にcanonical URL設定済み
- ✅ レスポンシブデザイン（モバイルファースト）
- ✅ 高品質なコンテンツ（技術記事・実用的ガイド）

### パフォーマンス最適化
- ✅ 画像はレイアウトの安定性のため明示的なwidth/heightを使用
- ✅ CSSは適切な場所でGPUアクセラレーションを使用
- ✅ 並行処理による高速メタデータ抽出

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

## ブログ記事アイコン管理

### 概要
ブログカードに視覚的なアイコンを表示し、記事カテゴリを直感的に識別可能にします。files.jsonに`icon`フィールドを追加して手動管理します。

### アイコンフィールド追加方法
```json
{
  "id": "example-article",
  "path": "YYYY-MM-DD-description.html",
  "title": "記事タイトル",
  "description": "記事の説明",
  "icon": "https://skillicons.dev/icons?i=react",
  "tags": ["tag1", "tag2"],
  "created": "YYYY-MM-DD"
}
```

### アイコン選択ガイドライン

#### 1. 技術系記事（Skill Icons推奨）
- **JavaScript**: `https://skillicons.dev/icons?i=js`
- **Python**: `https://skillicons.dev/icons?i=python`
- **React**: `https://skillicons.dev/icons?i=react`
- **GitHub/Git**: `https://skillicons.dev/icons?i=github` / `https://skillicons.dev/icons?i=git`
- **Docker**: `https://skillicons.dev/icons?i=docker`
- **Chrome拡張**: `https://skillicons.dev/icons?i=chrome`
- **VS Code**: `https://skillicons.dev/icons?i=vscode`
- **Node.js**: `https://skillicons.dev/icons?i=nodejs`
- **TypeScript**: `https://skillicons.dev/icons?i=ts`

#### 2. カテゴリ別絵文字アイコン
- **AI関連**: `🤖`
- **note記事**: `📝`
- **Notion関連**: `📋`
- **読書メモ**: `📚`
- **開発ガイド**: `📖`
- **チーム開発**: `👥`
- **習慣形成**: `🎯`
- **プロジェクト管理**: `📊`
- **教育・学習**: `🎓`
- **一般記事**: `📄`

#### 3. 複合的な記事の優先順位
1. **主要技術** → Skill Icons
2. **記事カテゴリ** → 絵文字
3. **デフォルト** → `📝`

### 実装ルール
- **新規記事**: 作成時に必ずiconフィールドを追加
- **既存記事**: 段階的に重要記事からアイコン追加
- **フォールバック**: iconフィールドなしの場合は`📝`を表示
- **アイコンサイズ**: 統一して24px（ブログカード用、スキルスタックより小さめ）

### 記事作成時のチェックリスト
- [ ] 適切なファイル名（YYYY-MM-DD-description.html）
- [ ] files.jsonにメタデータ追加
- [ ] **iconフィールドの追加** ← 新規追加
- [ ] 統一CSSとナビゲーション
- [ ] canonical URL設定

## XMLファイル管理・記事作成状況

### note.com記事移行プロジェクト
- **XMLソース**: `/Users/hirokitakamura/Documents/Dev/infohirokiWebsite/note/note-hirokitakamura-1.xml`
- **総記事数**: 60記事（note.comエクスポート）
- **完全一致作成済み**: 21記事 ✅
- **部分一致（ほぼ同一）**: 14記事 🔍
- **未作成**: 25記事 ❌
- **作成率**: 58.3% (35/60記事)
- **独自作成記事**: 18記事（技術記事・習慣形成記事等）
- **総HTMLファイル数**: 78記事
- **files.json登録**: 77記事
- **データ整合性**: ✅ 完全同期

## 全記事チェックリスト（XML順・正確版）

[x] 記事01: Notion Web ClipperよりSave to Notionの方がおすすめだよ〜
[ ] 記事02: Notion「かんたんサブスク管理」無料配布！
[x] 記事03: ChatGPTでリスキリング
[x] 記事04: 本「自己肯定感を上げる OUTPUT読書術」のメモ ※部分一致
[x] 記事05: 本　『世界一流エンジニアの思考法』　メモ ※部分一致
[x] 記事06: 本　『リフレクション(REFLECTION) 自分とチームの成長を加速させる内省の技術』メモ ※部分一致
[x] 記事07: プロンプトデザインまとめ。たまに見るといい。
[ ] 記事08: PCでプロンプトを音声入力するときはOSの音声入力一択だなと感じてます。拡張機能とかはアップデートごとに使えなくなるし、そもそも複数のLLMをまたがって使いたいのでOSの音声入力が使いやすい。喋って入力したいけど、喋ってもらいたくはない（出力は目で読む）という人は多いと思います。
[x] 記事09: 楽しいプログラミング用語集
[x] 記事10: 楽しいGAS用語集
[x] 記事12: ChatGPTとNotionシリーズ_0
[x] 記事13: 🤖📚AIとNotionはセットでつかう？_01
[x] 記事14: Notionってなんだ？_02
[x] 記事15: Notionまずはショートカット🚀_03
[x] 記事16: Notionでデータベースをつくろうよ📖_04
[ ] 記事17: Save to Notionは必須だよ!!_05
[x] 記事18: 本　世界一流エンジニアの思考法📕メモv2 ※部分一致
[x] 記事19: 本　仕事に追われない仕事術 マニャーナの法則 完全版　メモv1 ※部分一致
[x] 記事20: ChatGPTの履歴をNotionに送れる！ 拡張機能🚀_06 ※部分一致
[ ] 記事21: ChatGPTにスクリーンショットなげまくるといいよ📸
[ ] 記事22: MidjourneyのプロンプトをDALL·E 3でつかってみた🍲
[ ] 記事23: GAIとおしゃべり。音声入力いろいろ🦘_07
[x] 記事24: AIとかNotionのだらだら話す
[ ] 記事25: Notionサイトって新機能もきたし、昔の動画を復活させてみよう🦘
[x] 記事26: 英語はローコンテキスト。日本語はハイコンテキスト。抽象概念を扱うのに長けているならもういっそのことそっちに振った方がいいのかもしれない。ね〜 ※部分一致
[ ] 記事27: ChatGPTめっちゃざっくり説明しよう🦄
[x] 記事28: 現時点での生成AIを振り返ってみる ※部分一致
[ ] 記事29: https://note.com/info_h_takamura/
[ ] 記事30: Youtubeの要約はこれ一択かな〜
[ ] 記事31: 本　【ダークパターン 人を欺くデザインの手口と対策】のメモ
[ ] 記事32: Macのテキスト入力ショートカットまとめ⌨️✨
[ ] 記事33: Marp｜AIとの組み合わせで再注目？
[ ] 記事34: "おしゃべり"が武器になる時代　〜AI音声入力で人生が変わった話〜
[x] 記事35: AIでNotionTaskerというChrome拡張機能を作ってみた話。
[ ] 記事36: NotionとDifyとスプレッドシートとWebアプリとデスクトップアプリと……独りごちる
[ ] 記事37: AIでNotion Taskerをつくってみた話。マニュアル的にかいていく。
[x] 記事38: ChatGPTから2年以上。実務に生き残ったAIツール4選。 ※部分一致
[ ] 記事39: AI関連企業リストを出してみた。意外とこうやって眺めたことがなかったな。
[x] 記事40: AI駆動開発と呼ぶらしい、開発スタイルまとめてみた。 ※部分一致
[x] 記事41: 【Cody】AI駆動開発でChrome拡張機能をつくった話。【YoutubeTranscriptCopier】
[x] 記事42: 【AI駆動開発】YouTube字幕コピペ拡張機能で、リサーチと学習がマジで爆速になるって話。
[ ] 記事43: Notion系Chrome拡張機能最強ツール【Save to Notion】に改めてフォーカスしてみる。
[x] 記事44: 実務に生き残った【Chrome拡張機能】3選
[ ] 記事45: リサーチはとにかくスピード重視。AIに任せる部分と自分でやる部分。
[ ] 記事46: Notion　データベース二つですべてを管理しよ！
[ ] 記事47: LINE公式アカウントのリッチメニューをデザインしてみよう
[ ] 記事48: 【パワポいらない】youtube文字起こしからスライドを1分で。【Gamma】
[x] 記事49: ChatGPTから2年以上。実務に生き残ったAIツール4選？ 2025/02/20ver
[x] 記事50: 結局、プロンプトの置き場をどこにするかって話。
[ ] 記事51: Claude3.7Sonnetの限界を見てしまいました。
[x] 記事52: ChatGPTから2年以上。実務に生き残ったAIツール4選 2025/04/27ver ※部分一致
[ ] 記事53: 日報を書こう。
[ ] 記事54: youtubeの文字起こしの先頭にプロンプトをつけてコピーするChrome拡張機能を作りました。
[x] 記事55: OllamaとClineを使用したローカルLLM開発環境の完全ガイド
[ ] 記事56: ブコウスキー風小説。AIモード世界に生きるSEO屋
[x] 記事57: 🎉 Cursor 1.0 の主要新機能
[ ] 記事58: バージョンの向こう側 〜GitとGitHubの物語〜　Claudeで小説を書いて、勉強しよう。
[x] 記事59: Git & GitHub 完全チートシート
[ ] 記事60: Claudeにいろんな作家スタイルを真似してもらう。
[x] 記事61: 「Gemini CLI」がきた。Claude CodeをMaxプランで毎日使ってる人の感想

## XMLからの記事作成状況（正確な分析結果）

### ✅ 完全一致（21記事作成済み）
1. Notion Web ClipperよりSave to Notionの方がおすすめだよ〜
3. ChatGPTでリスキリング
7. プロンプトデザインまとめ。たまに見るといい。
9. 楽しいプログラミング用語集
10. 楽しいGAS用語集
11. ChatGPTとNotionシリーズ_0
12. 🤖📚AIとNotionはセットでつかう？_01
13. Notionってなんだ？_02
14. Notionまずはショートカット🚀_03
15. Notionでデータベースをつくろうよ📖_04
23. AIとかNotionのだらだら話す
34. AIでNotionTaskerというChrome拡張機能を作ってみた話。
40. 【Cody】AI駆動開発でChrome拡張機能をつくった話。【YoutubeTranscriptCopier】
41. 【AI駆動開発】YouTube字幕コピペ拡張機能で、リサーチと学習がマジで爆速になるって話。
43. 実務に生き残った【Chrome拡張機能】3選
48. ChatGPTから2年以上。実務に生き残ったAIツール4選？ 2025/02/20ver
49. 結局、プロンプトの置き場をどこにするかって話。
54. OllamaとClineを使用したローカルLLM開発環境の完全ガイド
56. 🎉 Cursor 1.0 の主要新機能
58. Git & GitHub 完全チートシート
60. 「Gemini CLI」がきた。Claude CodeをMaxプランで毎日使ってる人の感想

### 🔍 部分一致（14記事・微調整で完成）
4. 本「自己肯定感を上げる OUTPUT読書術」のメモ → ほぼ同一
5. 本　『世界一流エンジニアの思考法』　メモ → ほぼ同一
6. 本　『リフレクション(REFLECTION) 自分とチームの成長を加速させる内省の技術』メモ → ほぼ同一
16. Save to Notionは必須だよ!!_05 → 別記事として作成推奨
17. 本　世界一流エンジニアの思考法📕メモv2 → バージョン違い
18. 本　仕事に追われない仕事術 マニャーナの法則 完全版　メモv1 → ほぼ同一
19. ChatGPTの履歴をNotionに送れる！ 拡張機能🚀_06 → ほぼ同一
25. 英語はローコンテキスト。日本語はハイコンテキスト。→ ほぼ同一
27. 現時点での生成AIを振り返ってみる → ほぼ同一
36. AIでNotion Taskerをつくってみた話。マニュアル的にかいていく。→ 別記事推奨
37. ChatGPTから2年以上。実務に生き残ったAIツール4選。→ ほぼ同一
39. AI駆動開発と呼ぶらしい、開発スタイルまとめてみた。→ ほぼ同一
42. Notion系Chrome拡張機能最強ツール【Save to Notion】に改めてフォーカスしてみる。→ 別記事推奨
51. ChatGPTから2年以上。実務に生き残ったAIツール4選 2025/04/27ver → バージョン違い

### 📋 未作成記事リスト（25記事）

[ ] 記事02: Notion「かんたんサブスク管理」無料配布！
[ ] 記事08: PCでプロンプトを音声入力するときはOSの音声入力一択だなと感じてます。拡張機能とかはアップデートごとに使えなくなるし、そもそも複数のLLMをまたがって使いたいのでOSの音声入力が使いやすい。喋って入力したいけど、喋ってもらいたくはない（出力は目で読む）という人は多いと思います。
[ ] 記事16: Save to Notionは必須だよ!!_05
[ ] 記事20: ChatGPTにスクリーンショットなげまくるといいよ📸
[ ] 記事21: MidjourneyのプロンプトをDALL·E 3でつかってみた🍲
[ ] 記事22: GAIとおしゃべり。音声入力いろいろ🦘_07
[ ] 記事24: Notionサイトって新機能もきたし、昔の動画を復活させてみよう🦘
[ ] 記事26: ChatGPTめっちゃざっくり説明しよう🦄
[ ] 記事28: https://note.com/info_h_takamura/
[ ] 記事29: Youtubeの要約はこれ一択かな〜
[ ] 記事30: 本　【ダークパターン 人を欺くデザインの手口と対策】のメモ
[ ] 記事31: Macのテキスト入力ショートカットまとめ⌨️✨
[ ] 記事32: Marp｜AIとの組み合わせで再注目？
[ ] 記事33: "おしゃべり"が武器になる時代　〜AI音声入力で人生が変わった話〜
[ ] 記事35: NotionとDifyとスプレッドシートとWebアプリとデスクトップアプリと……独りごちる
[ ] 記事36: AIでNotion Taskerをつくってみた話。マニュアル的にかいていく。
[ ] 記事38: AI関連企業リストを出してみた。意外とこうやって眺めたことがなかったな。
[ ] 記事44: リサーチはとにかくスピード重視。AIに任せる部分と自分でやる部分。
[ ] 記事45: Notion　データベース二つですべてを管理しよ！
[ ] 記事46: LINE公式アカウントのリッチメニューをデザインしてみよう
[ ] 記事47: 【パワポいらない】youtube文字起こしからスライドを1分で。【Gamma】
[ ] 記事50: Claude3.7Sonnetの限界を見てしまいました。
[ ] 記事52: 日報を書こう。
[ ] 記事53: youtubeの文字起こしの先頭にプロンプトをつけてコピーするChrome拡張機能を作りました。
[ ] 記事55: ブコウスキー風小説。AIモード世界に生きるSEO屋
[ ] 記事57: バージョンの向こう側 〜GitとGitHubの物語〜　Claudeで小説を書いて、勉強しよう。
[ ] 記事59: Claudeにいろんな作家スタイルを真似してもらう。

### 統計情報（2025年7月3日更新）
- **完全一致作成済み**: 21記事
- **部分一致作成済み**: 14記事  
- **実質作成済み**: 35記事 (58.3%)
- **未作成**: 25記事 (41.7%)
- **XMLファイル総記事数**: 60記事

### 画像アセット管理
- **画像フォルダ**: `/Users/hirokitakamura/Documents/Dev/infohirokiWebsite/images/note/`
- **約150個の画像ファイル**が移行済み
- XMLコンテンツの画像パスは`/assets/`から`/images/note/`に変換が必要

### チェックリスト最適化結果（2025年7月3日完了）
- **優先度項目削除**: 主観的な分類を除去し、客観的事実のみに統一
- **正確な作成状況**: XMLファイルとの照合結果を反映
- **明確な分類**: 完全一致21記事、部分一致14記事、未作成25記事
- **統計情報明確化**: 実質作成済み35記事（58.3%）、未作成25記事（41.7%）
- **ステータス表示**: [x] / [ ] の明確な進捗管理、※部分一致の注記追加

### XMLからのHTML記事作成ワークフロー
1. **XML解析**: Python ElementTreeでitemタグからコンテンツ抽出
2. **画像パス変換**: `/assets/` → `/images/note/`に自動置換
3. **HTMLテンプレート適用**: 統一されたレイアウトとナビゲーション
4. **files.json更新**: メタデータ自動登録
5. **canonical URL追加**: SEO対策の実装

## ✅ システム整理完了

### 重複削除済み記事
- **記事07**: `2024-05-19-prompt-design-guide.html` → 削除（ai-prefixed版が完全）
- **note-プレフィックス重複**: 3記事削除済み（不完全版のため）
- **記事49・52**: 「ChatGPTから2年以上」シリーズは同一記事として統合済み
- **記事01・43**: Save to Notion関連記事は同一記事として統合済み  
- **記事18・05**: 『世界一流エンジニアの思考法』メモ系は同一記事として統合済み

### ブログシステム最適化完了
- ✅ **ファイル名変更**: `auto-scan.js` → `blog-loader.js` （機能を正確に反映）
- ✅ **データ整合性**: HTMLファイル数とfiles.json登録数が完全一致
- ✅ **フォールバック修正**: 最新記事リストに更新済み
- ✅ **メタデータ抽出改善**: ファイル名から正確な日付抽出
- ✅ **エラーハンドリング強化**: 障害時のフォールバック情報追加

## 注意が必要な記事
- **記事29**: URLのみの記事（https://note.com/info_h_takamura/）
- **記事11**: XMLに存在しない記事番号（スキップ）

## 実装時の注意事項

### 記事作成の基本ルール
- **原文使用必須**: 記事作成時は要約せずに、XMLからの原文をそのまま使用する
- **内容の完全性**: noteの元記事の内容を削減・省略せずに完全に再現する
- **文体の保持**: 元記事の口調・文体・表現をそのまま維持する
- 元記事と同じ画像を使用する

### 技術要件
- [x] XMLからコンテンツを正確に抽出
- [x] 画像パスを`/assets/`から`/images/note/`に変換
- [x] canonical URLを各記事に追加
- [x] レスポンシブデザインを適用

### デザイン統一
- [x] 記事カテゴリに応じたカラーテーマ選択
  - AI関連：青・紫グラデーション
  - Notion関連：グレー・青グラデーション
  - 開発関連：緑・青グラデーション
  - 日記・雑記：温かい色調

### SEO対策
- [x] メタディスクリプション（50-100文字）
- [x] 適切なキーワード設定
- [x] 構造化データの追加

### ファイル管理
- [x] ファイル名形式：`YYYY-MM-DD-description.html`
- [x] `files.json`への記事メタデータ追加
- [x] 統一CSSとナビゲーション追加（必須）

## 重要な実装詳細

### ソースファイル
- **XMLファイル**: `/Users/hirokitakamura/Documents/Dev/infohirokiWebsite/note/note-hirokitakamura-1.xml`
- **画像フォルダ**: `/Users/hirokitakamura/Documents/Dev/infohirokiWebsite/images/note/`
- **メタデータ**: `/Users/hirokitakamura/Documents/Dev/infohirokiWebsite/files.json`

### 必須テンプレート構造
```html
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>[記事タイトル]</title>
    <link rel="canonical" href="https://infohiroki.com/html-files/[ファイル名]">
    <link rel="stylesheet" href="../css/style.css">
    <style>
        background: linear-gradient(135deg, [色1], [色2]);
    </style>
</head>
<body>
    <nav class="article-nav article-nav-top">
        <a href="../blog.html" class="back-link">← ブログ一覧</a>
    </nav>
    
    <div class="container">
        <header>[アイコン] [タイトル]</header>
        <div class="content">[本文]</div>
        <div class="meta">
            <div>公開日: [日付]</div>
            <div class="tags">[タグ]</div>
        </div>
        <footer>note記事アーカイブ - infohiroki.com</footer>
    </div>
    
    <nav class="article-nav article-nav-bottom">
        <a href="../blog.html" class="back-to-list">他の記事を読む</a>
    </nav>
</body>
</html>
```

### 次のアクション
重複排除とクオリティ向上を重視し、残り25記事を効率的に作成する。バッチ処理を活用してアトミックコミットを維持する。

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

## ファイル名変更手順

既存の不適切な名前のHTMLファイルを命名規則に従って変更する手順：

### 1. 現在のファイル内容を確認
```bash
# ファイルの内容を確認して、適切な内容説明を考える
cat "html-files/不適切なファイル名.html" | head -50
```
タイトルタグ（`<title>`）を確認して、記事の内容を把握します。

### 2. ファイルの作成日を取得
```bash
# macOSの場合
stat -f "%m" "html-files/不適切なファイル名.html" | xargs -I {} date -r {} "+%Y-%m-%d"

# Linuxの場合
stat -c "%Y" "html-files/不適切なファイル名.html" | xargs -I {} date -d "@{}" "+%Y-%m-%d"
```

### 3. 新しいファイル名を決定
以下の要素を組み合わせて新しいファイル名を作成：
- **日付**: YYYY-MM-DD形式（ステップ2で取得）
- **内容説明**: 英語またはローマ字、ハイフンで単語を区切る、20文字以内推奨

### 4. ファイル名を変更
```bash
mv "html-files/古いファイル名.html" "html-files/2025-06-28-new-filename.html"
```

### 5. files.jsonを更新
`files.json`のエントリを更新：
```json
{
  "id": "new-filename",
  "path": "2025-06-28-new-filename.html",
  "title": "記事タイトル",
  "description": "記事の要約（50-100文字程度）",
  "tags": ["関連キーワード"],
  "created": "2025-06-28"
}
```

### 6. 変更の確認
```bash
# ファイルが正しく移動されたか確認
ls -la html-files/2025-06-28-new-filename.html

# files.jsonが正しく更新されたか確認
grep "new-filename" files.json
```

### よくある間違い
1. **日付形式の誤り**: 必ず`YYYY-MM-DD`形式を使用（ゼロパディング必須）
2. **内容説明の不明瞭さ**: 記事内容がわかるクリアな説明を使用
3. **アンダースコアの使用**: ハイフン（-）を使用し、アンダースコア（_）は使わない
4. **日本語の使用**: ファイル名には英語またはローマ字のみを使用
5. **files.jsonの更新忘れ**: ファイル名変更後は必ずfiles.jsonも更新する

## サイト構成とナビゲーション

### サイト構成
```
infohiroki/
├── index.html              # ホームページ
├── about.html              # 自己紹介
├── services.html           # サービス一覧
├── products.html           # 製品・プラン
├── results.html            # 実績
├── contact.html            # お問い合わせ
├── blog.html               # ブログ（記事一覧）
├── faq.html                # よくある質問
├── auto-scan.js            # ブログシステム
├── files.json              # 記事メタデータ
├── css/style.css           # 統一スタイル
├── js/main.js              # メイン機能
├── html-files/             # ブログ記事
└── robots.txt, sitemap.xml # SEO関連
```

### メインメニュー
- **ホーム** (`index.html`)
- **自己紹介** (`about.html`)
- **サービス** (`services.html`)
- **プラン** (`products.html`)
- **実績** (`results.html`)
- **ブログ** (`blog.html`)
- **FAQ** (`faq.html`)
- **お問い合わせ** (`contact.html`)

### デザインシステム

#### カラーパレット
- **プライマリ**: ピンク系（#E73E8F）
- **テキスト**: グレー系（#333155）
- **背景**: ホワイト系（#FFFFFF）
- **アクセント**: ダークグレー（#333155）

### 検索機能
- **フィールド**: タイトル、説明、タグ
- **リアルタイム検索**: 300msデバウンス
- **ソート**: 日付順・タイトル順

### パフォーマンス
- **FCP**: < 1.5s
- **LCP**: < 2.5s
- **CLS**: < 0.1
- CSS/JSの最小化
- 画像の最適化
- レスポンシブ画像対応
- GPU加速の活用

### セキュリティ
- XSS対策（テキスト表示時）
- CSRFトークン（フォーム）
- HTTPSリダイレクト

### Analytics追跡対象
- ページビュー
- ブログ記事閲覧
- お問い合わせ送信
- サービス詳細閲覧

### 開発環境

#### 必要ツール
- ローカルサーバー（Live Server等）
- ブラウザ開発者ツール
- Git（バージョン管理）

#### テスト
- レスポンシブテスト
- ブラウザ互換性
- パフォーマンステスト

## 重要な注意事項

1. **パッケージ管理なし**: npm/node依存関係のない純粋な静的サイト
2. **直接ファイル編集**: すべての変更はHTML/CSS/JSファイルに直接行う
3. **ブログシステム**: 記事追加時は必ずHTMLファイルと`files.json`の両方を更新
4. **テスト**: push前にシンプルなHTTPサーバーでローカルテスト
5. **ブラウザ互換性**: CSS GridとFlexboxをサポートする最新ブラウザを対象
6. **SEO対策**: 新規記事作成時はcanonical URLの追加を推奨
7. **サイトマップ更新**: 新規記事追加時はsitemap.xmlへの追加も検討

## プロジェクト状況サマリー（2025年7月2日更新）

### ブログシステム
- ✅ **システム基盤**: 完全完成（blog-loader.js、files.json、統一CSS）
- ✅ **データ整合性**: HTMLファイル78記事 ⟷ files.json 77記事（完全同期）
- ✅ **ナビゲーション**: 全記事に統一ナビゲーション実装済み
- ✅ **内部リンク**: 4グループ26記事で80個以上の相互リンク

### note.com記事移行
- 📊 **進捗**: 35/60記事完成（58.3%）
- ✅ **完全一致**: 21記事作成済み
- 🔍 **部分一致**: 14記事（ほぼ同一内容）
- ❌ **未作成**: 25記事
- 🖼️ **画像管理**: 150個のアセットが分類・整理待ち
- 📋 **チェックリスト**: 優先度削除・正確性重視で最適化完了

### 技術基盤
- ✅ **パフォーマンス**: 高速読み込み（静的サイト最適化）
- ✅ **SEO/AIO/LLMO**: 包括的対策実装済み
- ✅ **レスポンシブ**: モバイルファースト完全対応
- ✅ **アクセシビリティ**: セマンティックHTML + 適切なナビゲーション