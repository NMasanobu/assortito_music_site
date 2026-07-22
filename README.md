# assortito_music_site
Assortito GitHub Pages

## 目的

このリポジトリは、**Assortito の自作曲を紹介・公開するための GitHub Pages サイト**を管理する。

主な目的は以下の通り。

1. **作品情報の公開**
   - 曲名
   - 編成
   - 難易度
   - 演奏時間
   - 曲の説明

2. **試聴導線の提供**
   - YouTube 埋め込みによる試聴

3. **楽譜販売ページへの導線**
   - 外部サービス（Piascore）へのリンク

4. **SEO（検索流入）の確保**
   - 曲ごとの個別ページ
   - 適切な title / description
   - 構造化されたページ設計

---

## フレームワーク

本サイトは **Jekyll** を使用して構築する。

- Static Site Generator
- GitHub Pages 標準対応
- Markdown ベース
- テンプレートによる共通化
- 曲ページ追加が容易

公式サイト：

https://jekyllrb.com/

---

## 基本方針

- **1曲 = 1ページ**
- 曲ページは Markdown で管理
- トップページに作品一覧を表示
- 共通レイアウトは `_layouts/` に配置
- CSS は `assets/css/` に配置
- GitHub Pages で自動デプロイ

---

## フォルダ構成

```text
assortito_music_site/
├── README.md
├── _config.yml
├── index.md
├── _layouts/
│   ├── default.html
│   └── piece.html
├── _music/
│   ├── piece1.md
│   ├── piece2.md
│   ├── ....
├── assets/
│   └── css/
│       └── style.css
└── private/
    ├── piece_template.md
    ├── ...
```

---

## 各ファイルの説明と実装例

以下のコードは**実装例**であり、必要に応じて変更してよい。

---

### `_config.yml`

Jekyll の全体設定ファイル。

用途：

- サイト名
- URL
- GitHub Pages 設定
- Collection 定義
- 除外ファイル設定

実装例：

```yaml
title: "Assortito Music"
description: "自作曲公開サイト"
url: "https://YOUR_USERNAME.github.io"
baseurl: "/assortito_music_site"

lang: ja

markdown: kramdown

collections:
  music:
    output: true

defaults:
  - scope:
      path: ""
      type: "music"
    values:
      layout: piece

exclude:
  - private
```

---

### `index.md`

サイトのトップページ。

用途：

- サイト説明
- 曲一覧表示
- 自動リンク生成

実装例：

```markdown
---
layout: default
title: 自作曲一覧
---

# Assortito Music

クラリネットアンサンブル作品を中心に公開しています。

## 作品一覧

{% raw %}
{% for piece in site.music %}
- [{{ piece.title }}]({{ piece.url | relative_url }})
{% endfor %}
{% endraw %}
```

---

### `_layouts/default.html`

全ページ共通レイアウト。

用途：

- HTML骨格
- head 要素
- SEO metadata
- 共通ヘッダー／フッター

実装例：

```html
<!doctype html>
<html lang="ja">
<head>
  <meta charset="utf-8">

  <title>{{ page.title }} | {{ site.title }}</title>

  <meta
    name="description"
    content="{{ page.description | default: site.description }}"
  >

  <link
    rel="stylesheet"
    href="{{ '/assets/css/style.css' | relative_url }}"
  >
</head>
<body>

<header>
  <h1>
    <a href="{{ '/' | relative_url }}">
      {{ site.title }}
    </a>
  </h1>
</header>

<main>
  {{ content }}
</main>

<footer>
  <p>© Assortito</p>
</footer>

</body>
</html>
```

---

### `_layouts/piece.html`

各曲ページ専用レイアウト。

用途：

- 曲情報表示
- YouTube 埋め込み
- Piascore リンク表示

実装例：

```html
---
layout: default
---

<h1>{{ page.title }}</h1>

<p>編成: {{ page.instrumentation }}</p>
<p>難易度: {{ page.level }}</p>
<p>演奏時間: {{ page.duration }}</p>

{{ content }}

<h2>試聴</h2>
{{ page.youtube_embed }}

<h2>楽譜購入</h2>
<p>
  <a href="{{ page.piascore_url }}">
    Piascoreで購入する
  </a>
</p>
```

---

### `music/clarinet-quartet-no1.md`

各曲ページ。

用途：

- 曲ごとのメタデータ管理
- 曲説明
- SEO対象ページ

実装例：

```markdown
---
title: クラリネット四重奏曲 第1番
description: >
  オリジナルのクラリネット四重奏作品。

instrumentation: Bb Clarinet ×4
level: 中級
duration: 約4分

piascore_url: "https://store.piascore.com/XXXXXXXX"

youtube_embed: |
  <iframe
    width="560"
    height="315"
    src="https://www.youtube.com/embed/VIDEO_ID"
    frameborder="0"
    allowfullscreen>
  </iframe>
---

静かな導入から始まり、
後半に向けて広がる作品。
```

---

### `assets/css/style.css`

サイト全体のデザイン。

用途：

- レイアウト
- 色
- タイポグラフィ
- レスポンシブ調整

実装例：

```css
body {
  max-width: 900px;
  margin: 0 auto;
  padding: 2rem;
  font-family: system-ui, sans-serif;
  line-height: 1.7;
  background: #fafafa;
  color: #222;
}

a {
  color: #0a66c2;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}
```

---

### `private/`

公開しないメモ置き場。

用途：

- 作曲メモ
- TODO
- 下書き
- 作品ページのMarkdownひな形

注意：

`_config.yml` の `exclude` に含めることで、
GitHub Pages の公開対象から除外する。

例：

```text
private/
├── piece_template.md
├── notes.md
└── drafts.md
```

---

## GitHub Pages 公開手順

1. GitHub リポジトリを作成
2. この構成でファイルを配置
3. `main` ブランチへ push
4. GitHub Settings → Pages
5. `Deploy from a branch`
6. `main` / `(root)` を選択

公開 URL 例：

```text
https://YOUR_USERNAME.github.io/assortito_music_site/
```

---

## LLM エージェントへの期待

この README を仕様書として参照し、LLM エージェントは以下を行う。

- 新しい曲ページの生成
- メタデータ入力
- トップページ更新
- CSS 改善
- SEO metadata 改善
- レイアウト改善

生成時は以下を守ること。

- 既存構造を維持する
- Jekyll の Front Matter を壊さない
- `relative_url` を使う
- GitHub Pages 互換を保つ
- 1曲1ファイルを維持する

---

# 動作確認
事前準備
- rubyインストール (3.1～3.3)
- gem install bundler jekyll
- bundle init
- bundle add github-pages --group jekyll_plugins

ローカルで起動
- bundle exec jekyll serve
- http://localhost:4000