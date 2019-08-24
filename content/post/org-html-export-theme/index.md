+++
title = "Org-modeでhtml exportの際のthemeについて"
author = ["taipapa"]
date = 2018-12-16
lastmod = 2018-12-22T12:13:19+09:00
tags = ["emacs", "orgmode", "html", "export", "css", "theme"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/Topkapi_Palace.jpg"
  caption = "Topkapi Palace"
+++

org-modeで文書を書いてhtmlにexportすると，素のままでは，なんの愛想もない．特にいくつかの項目をまとめた要約などを並べていくときは，side columnなどがあって，すぐに行きたいところに飛べるようになっていると嬉しい．ということで，今回はorg-modeをhtmlにexportするときのthemeがテーマである．．．．．

とにかく，たくさんのthemesが存在する．まずは以下のサイトをチェック，というか以下を読めばこのブログは読まなくても良いような．．．(^^;;;

1.  [org-modeのHTMLテーマ](https://qiita.com/sambatriste/items/2dc9f81cbf1e82d7429a)
2.  [org-modeのHTMLテーマ第2弾](https://qiita.com/sambatriste/items/c8e70368ee5fd092096b)
3.  [How to export Org mode files into awesome HTML in 2 minutes](https://github.com/fniessen/org-html-themes#how-to-export-org-mode-files-into-awesome-html-in-2-minutes)
4.  [org-spec](https://github.com/thi-ng/org-spec)

私のお気に入りは，ReadTheOrg（上記の1, 3にある）とorg-spec（上記の4）である．

{{% toc %}}

## ReadTheOrg {#readtheorg}

これは[Read the Docs](https://docs.readthedocs.io/en/latest/)で使われているthemeのcloneである．一番簡単な使い方は，3にあるようにsetup fileをorg fileのpreambleに書いておくことである．

```lisp
#+SETUPFILE: https://fniessen.github.io/org-html-themes/setup/theme-readtheorg.setup
```

あるいは，3からOrg-HTML themes projectをダウンロードしてきて解凍しローカルの"setup file"へのパスを書き込めば，ネットの接続に依存せずにexportできるようになる．たとえば，/Hoge/Fuga/org-html-themes-master/setup/theme-readtheorg.setupに設定ファイルがあるとすると以下のようにすれば良い．

```lisp
#+SETUPFILE: /Hoge/Fuga/org-html-themes-master/setup/theme-readtheorg.setup
```

以下に研究会で発表するスライド原稿を作る下準備として，実際に10個の論文をまとめたorg fileの一部を掲示しておく．左に論文のタイトルが並び，見ている論文の小見出しが自動的に展開される．subheadの色も設定されており，読みやすい．デザインもプロっぽい印象である．書いた内容にかかわらず，なんとなく賢くなったような気分になれる（笑）．

{{< figure src="/img/ReadTheOrg.jpg" width="100%" target="_self" >}}


## [org-spec](https://github.com/thi-ng/org-spec) {#org-spec}

An Org-mode template for technical specification documents and HTML publishing. とのことで，技術よりのthemeである．特徴としては，Ditaa, Graphviz & PlantUMLなどによりテキストベースで図が書ける．表に対応，自動的にアップデートするフィールド，PDF生成にも対応，コードブロックの基本的なsyntax highlightingなどがある．実際の例として[https://demo.thi.ng/org-spec/](https://demo.thi.ng/org-spec/)がある．

こちらの使い方は少しだけ面倒である．リンク先からorg-specをダウンロードして解凍する．ここで，style.cssが/Hoge/Fuga/org-spec-master/css/style.cssに保存されたとする．ダウンロードして来たファイルに含まれているindex.orgに全て書いてあるので，それを真似てorg fileのpreambleに次のように書いておく．

```lisp
#+HTML_HEAD: <link href="http://fonts.googleapis.com/css?family=Roboto+Slab:400,700|Inconsolata:400,700" rel="stylesheet" type="text/css" />
#+HTML_HEAD: <link href="/Hoge/Fuga/org-spec-master/css/style.css" rel="stylesheet" type="text/css" />
#+AUTHOR: taipapa
#+EMAIL: your@mail.address

#+HTML: <div class="outline-2" id="meta">
| *Author* | {{{author}}} ({{{email}}})    |
| *Date*   | {{{time(%Y-%m-%d %H:%M:%S)}}} |
#+HTML: </div>

#+TOC: headlines 2
```

以下に前述の論文のまとめをこのcssでexportしたものを掲示しておく．印象がかなり変わると思う．subheadなどは最初から展開されている．ReadTheOrgよりもビジネスライクな感じであるが，よりスマートな気もする．その日の気分によって，この2つを使い分けている．

{{< figure src="/img/org-spec.png" >}}

以上あげた2つ以外にも無数のthemeが存在する．また，自分でthemeを作ってしまう剛の者もいらっしゃるので，あちこちを探してみるのも一興．．．(^o^)
