+++
title = "Emacsのorg-modeで論文を書く（その5：htmlへのexportの際のフォントの色の変更，ハイライトなど）（2018年12月19日追記）（2020年1月12日追記）"
author = ["taipapa"]
date = 2018-12-10
lastmod = 2020-04-26T23:14:39+09:00
tags = ["emacs", "orgmode", "html", "export", "css", "color"]
type = "post"
draft = false
weight = 1
subtitle = "Hugo v0.60+ とGoldmarkへの対応"
[image]
  placement = 3
  caption = "Parthenon"
+++

学会発表や論文作成にあたっては，当然のことながら，その分野の他の研究者の論文を読んでまとめるなどの作業を行う．そこで，論文の要旨などをorg-modeにざっとまとめておくと，pdfにもhtmlにもtextにもexport出来て便利である．pdfは印刷に向いているが，htmlは多くの論文をいっぺんに見るのに向いており，また，compileの時間もpdfより圧倒的に速い．また，色を変えたり，ハイライトしたりするのもhtmlなら容易である．そこで，今回は，org-modeからhtmlへexportする際の有用な小技について書いてみたい．


## [Org Macros](https://github.com/fniessen/org-macros) {#org-macros}

フォントの色を変更する方法はいろいろあるが，ハイライトや背景の色の変更までできるこの [Org Macros](https://github.com/fniessen/org-macros)が一番便利である．内容は，org-modeの便利なマクロ集である．リンク先からダウンロードして，適当なところに保存し，解凍しておく．ここでは，/Users/taipapa/hoge/fuga/org-macros.setupに置くことにする．使い方は簡単で上記のwebsiteに書いてあるとおり，各org fileの先頭に以下のように記述してorg-macros.setupの場所を教えてやれば良い．

```lisp
#+INCLUDE: /Users/taipapa/hoge/fuga/org-macros.setup
```

これだけである．

-   ~~注意事項としては，このブログはox-hugoで書いているが，ox-hugoの場合は文書の先頭に上記を書いても効かない．各ポストのpropertyのあとに書いておけば効く．各ポストごとに設定するようになっているらしい．．．．．（全国15人？ぐらいの人にしか意味のない注意書きである）~~


### 上記に関しては，ox-hugoの作者であるKaushal Modi氏から以下のような指摘を頂いた．（12月19日追記） {#上記に関しては-ox-hugoの作者であるkaushal-modi氏から以下のような指摘を頂いた-12月19日追記}

<div class="panel">
  <div></div>

Can you try using #+setupfile instead of #+include? As an example, here is my blog Org source that I export using ox-hugo ( <https://gitlab.com/kaushalm>... ), and here is the setup file tha t I "include" using the more appropriate #+setupfile ( <https://gitlab.com/kaushalm>... ).

As you see, I use a lot of Org macros, and they all work in my "one post per subtree" flow.

</div>

ということで，ox-hugoの場合は，以下のように文書の先頭に書いておけば，one-post per subtreeの投稿全てにorg-macroが効くことを確認した．

```lisp
#+setupfile: /Users/taipapa/hoge/fuga/org-macros.setup
```

こんなブログにまで目を通してコメントしてくれるのには驚いた．親切な方である．日本語が読める人なのかとも思ったが，どうやらGoogleの翻訳を利用されているようだ．このページだと，[A Perfect Autumn Day](https://translate.google.com/translate?depth=1&sl=auto&sp=nmt4&tl=en&u=https://taipapamotohus.com/post/html%5Fexport/&xid=17259,1500004,15700019,15700124,15700149,15700186,15700190,15700201,15700237,15700242#comment-4245099680)に行くと翻訳版を見ることができる．その翻訳レベルにも今更ながら感心した．．．

-   残念ながら，LaTeXへのexportでは，この方法による色の変更などは（現在のところ）効かない．

いくつか使い方の例をあげておく

```lisp
{{{color(red, 赤くなるかな？)}}}
*{{{color(blue, ボールドで赤くなるかな？)}}}*
{{{highlight(yellow, 黄色にハイライトされるかな？)}}}
*{{{highlight(yellow, 黄色にハイライトされて文字はボールドになるかな？)}}}*
{{{bgcolor(cyan, 背景がシアンになるかな？)}}}
*{{{bgcolor(cyan, 背景がシアンになって文字はボールドになるかな？)}}}*
```

これが以下のように表示される．

-   <span style="color: red"> 赤くなるかな？</span>
-   **<span style="color: red"> ボールドで赤くなるかな？</span>**
-   <span style="background-color: yellow;"> 黄色にハイライトされるかな？</span>
-   **<span style="background-color: yellow;"> 黄色にハイライトされて文字はボールドになるかな？</span>**
-   <div style="background-color: cyan;"> 背景がシアンになるかな？</div>
-   **<div style="background-color: cyan;"> 背景がシアンになって文字はボールドになるかな？</div>**

上記以外にも多くのマクロが含まれており，そちらも人によっては有用かもしれない．少しだけ例をあげておく．以下はパネルの例．

```lisp
{{{begin_panel}}} Panel example This is a formatted block of text… {{{end_panel}}}
```

これが，
<div class="panel"><p> Panel example This is a formatted block of text… </p></div>
となる．マニュアルでは以下の使い方を薦めている．

```lisp
#+begin_panel
*Panel example* \\
This is a formatted block of text...
#+end_panel
```

<div class="panel">
  <div></div>

**Panel example** <br />
This is a formatted block of text...

</div>

なお，org-modeのマクロ自体に関しては，org-modeのマニュアルの **12.5 Macro replacement** を参考にしていただきたい．

{{% alert note %}}
**2020年1月12日　Hugoを使用している人のための追記**
{{% /alert %}}

2019年の年末にMacBook Pro 16 inch (Catalina)に買い替えた時，Hugoもv.０.61にupgradeした．その際に，HugoのMarkdown用のdefault libraryがGoldmarkに変更になっていることに気がついていなかった．そのためにこのページのフォントの色の変更が働かなくなっていた．数日前に気がついたので，修正した．詳細は以下のサイトを参考．

参考サイト1：[Goldmark – CommonMark compliant, GitHub flavored, fast and flexible – is the new default library for Markdown in Hugo.](https://gohugo.io/news/0.60.0-relnotes/)

参考サイト2：[Just wonder if the migration to Goldmark is going to be smooth ?](https://discourse.gohugo.io/t/ox-hugo-go-org/21254/7)

上記のサイトには，「マークダウンファイルにinline HTMLがたくさんあるのなら，unsafe modeを有効にしないといけないかもね」とあるので，config/default/config.tomlの最後に，

```nil
[markup]
  [markup.goldmark]
    [markup.goldmark.renderer]
      unsafe = true
```

を追加した．これで，Markdownがうまく働くようになり，再びフォントの色も変更されるようになった．


## [Exporting org-mode to HTML: In-place coloring](https://stackoverflow.com/questions/21340380/exporting-org-mode-to-html-in-place-coloring) {#exporting-org-mode-to-html-in-place-coloring}

フォントの色を変える別の方法である．リンク先にある通り，

```html
この文章は， *@@html:<font color = "blue">@@青のボールド@@html:</font>@@*になって欲しい！
```

これが以下のように表示される．<br />
この文章は， **<font color = "red">赤のボールド</font>** になって欲しい！

-   org-modeのマニュアルの **12.9.5 Quoting HTML tags** も参考のこと

    こちらは設定を必要としないが，やはり，最初に説明したマクロの方がいろいろ出来て便利である．

    次回は，htmlをexportする際のCSSについてまとめてみたい．
