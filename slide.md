---
lang: ja
title: Vivliostyleの最新の進化
author: Shinyu Murakami
---

# Vivliostyleの最新の進化 {.cover}

2023-05-28 \
Shinyu Murakami \
Vivliostyle Foundation

# 目次 {.toc role="doc-toc"}

1.  [『CSS組版Vivliostyle入門』の本が出たこと🎉](#css組版vivliostyle入門の本が出たこと)
    1.  [やっとVivliostyleがここまでの完成度に](#やっとvivliostyleがここまでの完成度に)
    1.  [入門が難しいという問題がこれでだいぶ解決](#入門が難しいという問題がこれでだいぶ解決)
1.  [Vivliostyle.js最近の更新](#vivliostylejs最近の更新)
    1.  [2022秋からの半年で実装した機能](#2022秋からの半年で実装した機能)
    1.  [Vivliostyle Viewerの新機能](#vivliostyle-viewerの新機能)
1.  [Vivliostyle今後の開発](#vivliostyle今後の開発)
    1.  [Vivliostyle.js残っている課題を進める](#vivliostylejs残っている課題を進める)
    1.  [Vivliostyle Pubの開発を進める](#vivliostyle-pubの開発を進める)
    1.  [Vivliostyle CLI / VFM / Themes](#vivliostyle-cli--vfm--themes)
    1.  [開発貢献者たち、協力者の皆様に感謝！](#開発貢献者たち協力者の皆様に感謝)

# 『CSS組版Vivliostyle入門』の本が出たこと🎉

## やっとVivliostyleがここまでの完成度に

- CSS組版の基本機能がだいぶ完成し、不便なところの改善が進んだことで、より多くの人がCSS組版Vivliostyleを学び始めるのによい状態になってきたこと
- Vivliostyle開発者たちも入門本の監修者として協力。最新の更新が本に反映されている。制作過程で問題になったVivliostyleのバグをいくつか修正できた。以下のものなど：
  - ページ下にまだアキがあるのに段落や画像が次ページに追い出されてしまうことがあったのを修正
  - ページ背景画像がbleed領域に描画されなかったのを修正

## 入門が難しいという問題がこれでだいぶ解決

<img src="https://vivliostyle.org/assets/posts/2023-05-10-vivliostyle-book/fig-1.png" style="float: right; float-reference: page; width: 9em; margin-left: 0.5em;" alt="『Web技術で本が作れる CSS組版Vivliostyle入門』Vivliostyle監修　リブロワークス著">

- CSS組版・Vivliostyleが難しかった理由：入門書がなかった
- おすすめの入門書ができたことの意味はとても大きい。感謝！
- これでCSS組版のノウハウが広がって、本に限らず入門に役立つコンテンツが増えることを期待
- Vivliostyleユーザー必携！　『Web技術で本が作れる CSS組版Vivliostyle入門』Vivliostyle監修　リブロワークス著


# Vivliostyle.js最近の更新

## 2022秋からの半年で実装した機能


- [lh and rlh units](https://drafts.csswg.org/css-values-4/#lh)　⭐️v2.20 (2022-12-16)〜
  - `lh` は行高(line-height)を基準にした長さの単位
  - `rlh` はルート要素のline-heightが基準
- [margin-breakプロパティ](https://www.w3.org/TR/css-break-4/#break-margins)　⭐️v2.20 (2022-12-16)〜
  - ブロックのマージンがページの先頭になったときの処理
    - デフォルトの `margin-break: auto` は、なりゆき改ページでページの先頭になったブロックのマージンを破棄
    - `margin-break: keep` はマージンを保持
    - `margin-break: discard` はマージンを破棄（0にする）
- [`leader()` function](https://www.w3.org/TR/css-content-3/#leaders)　⭐️v2.22 (2023-01-25)〜
  - 目次の項目とページ番号をつなぐ点線の出力に便利

例：

<style>
#leader-test li > a::after {
  content: leader(".") target-counter(attr(href), page);
}
</style>

<ol id="leader-test">
  <li><a href="#leader-test">leader() のテスト</a></li>
</ol>

<br>

```html
<style>
#leader-test li > a::after {
  content: leader(".") target-counter(attr(href), page);
}
</style>

<ol id="leader-test">
  <li><a href="#leader-test">leader() のテスト</a></li>
</ol>
```

- [Running elements](https://www.w3.org/TR/css-gcpm-3/#running-elements)　⭐️v2.25 (2023-05-15)〜
  - HTML文書内の要素をページマージン領域に柱（欄外見出し）として表示できるようにする

例：

<style>
@page {
  @top-left {
    content: element(RunningElement);
  }
}
.running-element {
  font-size: 0.7rem;
  position: running(RunningElement);
}
</style>

<div class="running-element">
  <ruby>柱<rt>はしら</rt></ruby>に<em>Running element</em>の例
</div>

```html
<style>
@page {
  @top-left {
    content: element(RunningElement);
  }
}
.running-element {
  font-size: 0.7rem;
  position: running(RunningElement);
}
</style>

<div class="running-element">
  <ruby>柱<rt>はしら</rt></ruby>に<em>Running element</em>の例
</div>
```

<div class="running-element">
  <ruby>柱<rt>はしら</rt></ruby>に<em>Running element</em>の例
</div>
<div class="running-element"></div>
<br>

[Named strings（名前付き文字列）](https://www.w3.org/TR/css-gcpm-3/#named-strings)と[Running elements](https://www.w3.org/TR/css-gcpm-3/#running-elements)の比較

|       |Named strings|Running elements|
|-------|-------------|----------------|
|柱の内容|テキストのみ|スタイルと構造を含む要素|
|指定する要素|見出し要素などに指定|柱のためだけの要素が必要|
|プロパティ指定|`string-set: 名前 content()`|`position: running(名前)`|
|柱を出力する指定|`content: string(名前)`|`content: element(名前)`|


## Vivliostyle Viewerの新機能

CSS組版のプレビューにとどまらず、ブラウザで本を読むためのビュワーとしてより便利なものに ⭐️[v2.24 (2023-04-01)](https://github.com/vivliostyle/vivliostyle.js/releases/tag/v2.24.0)〜

- 設定メニューなどUIの多言語化（英語と日本語）
- 「閲覧状態を復元」の機能。これをオンにすると：
  - 読んでる途中のページなど閲覧状態を次に開いたとき復元
- マーカー注釈機能。これをオンにすると：
  - テキストを選択して「ハイライト」でマーカーを引ける
  - メモの記入ができる。「マーカーとメモ」一覧表示も

# Vivliostyle今後の開発

## Vivliostyle.js残っている課題を進める

- 最新CSS仕様サポートの課題
  - [`::marker` 箇条書きのマークを指定する擬似要素](https://github.com/vivliostyle/vivliostyle.js/issues/732)
  - [`@counter-style`　カウンタースタイルを定義](https://github.com/vivliostyle/vivliostyle.js/issues/731)
  - [CSS Nesting](https://github.com/vivliostyle/vivliostyle.js/issues/1032)、その他
- CSS組版機能の改良の課題
  - [`@footnote` 脚注エリアのスタイル](https://github.com/vivliostyle/vivliostyle.js/issues/1045)
  - [ページフロートの改良](https://github.com/vivliostyle/vivliostyle.js/issues/543)
  - [Mac以外の環境でハイフネーションが無効になる](https://github.com/vivliostyle/vivliostyle.js/issues/569)
  - [段組処理の見直し](https://github.com/vivliostyle/vivliostyle.js/issues/579)、その他

<!--
- 段組の指定がルート要素またはbody要素以外にあるとき、段組のページ分割処理がうまくいかない　([#579](https://github.com/vivliostyle/vivliostyle.js/issues/579))
- テーブル内で脚注が機能しない　([#438](https://github.com/vivliostyle/vivliostyle.js/issues/438))
- [`@layer`　カスケードレイヤー](https://developer.mozilla.org/ja/docs/Web/CSS/@layer)　([#977](https://github.com/vivliostyle/vivliostyle.js/issues/977))
- [`@container`　コンテナクエリー](https://developer.mozilla.org/en-US/docs/Web/CSS/@container)　([#1046](https://github.com/vivliostyle/vivliostyle.js/issues/1046))
-->

## Vivliostyle Pubの開発を進める

- 原稿ファイルごとのプレビューだけでなく全体プレビューも
- 原稿（Markdown/HTML）とスタイルシート（CSS）を同時に開いて編集可能に
- [Vivliostyle Base Theme](https://github.com/vivliostyle/themes/tree/main/packages/%40vivliostyle/theme-base)のしくみに対応
- その他
  - [ベータ版公開までのTo Do（開発）](https://github.com/vivliostyle/vivliostyle-pub/issues/218)

## Vivliostyle CLI / VFM / Themes

[Vivliostyle Base Theme](https://github.com/vivliostyle/themes/tree/main/packages/%40vivliostyle/theme-base)ができたことで、Vivliostyle Themesの活用が広がることを期待。

VFMがv2に。今後予定されている改良にも期待→[VFM issues](https://github.com/vivliostyle/vfm/issues) 

Vivliostyle CLIがv7に。目次生成機能など今後予定されている改良に期待→[Vivliostyle CLI issues](https://github.com/vivliostyle/vivliostyle-cli/issues)

## 開発貢献者たち、協力者の皆様に感謝！

開発貢献はもちろん、不具合報告や改良要望も歓迎します！

- コミュニティ→ https://vivliostyle.org/ja/community/
- あなたも Vivliostyle のスポンサーになりませんか<br>→ https://vivliostyle.org/ja/sponsors/

Vivliostyleのサービスやプロダクトをビジネスで利用する上での相談なども歓迎します。ぜひお問合せください。

私たちについて→ https://vivliostyle.org/ja/about-us/
