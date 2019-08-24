+++
title = "How to automatically embed R plot in blog created by Hugo via ox-hugo"
author = ["taipapa"]
date = 2019-03-25
lastmod = 2019-03-30T21:54:40+09:00
tags = ["R", "plot", "embed", "Hugo", "ox", "hugo", "blog", "emacs", "org-mode"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/Hotel_de_Ville_de_Bruxelles.jpg"
  caption = "Hôtel de Ville de Bruxelles"
+++

（承前）前回（[How to automatically embed R plot into html exported by org-mode with org-babel](../ExportRplot)）はorg-babelを設定して，Rで描いたグラフを自動でhtmlやpdfに挿入するところまでまとめた．繰り返しになるが，本サイトは，ox-hugoで書いてHugo用のMarkdownをexportすることにより作成している．前々回の記事（[How to plot survival curve of competing risk analysis with censoring mark and number at risk (at risk table)](../prodlim)）を書いている際に，Rでplotしたgraphをブログ記事の中に自動ではめ込むよう設定するのに苦労した．前回でorg-babelの設定は終わっているので，今回は，Hugoやox-hugoの設定に関してまとめ，ブログ記事へのR plotの自動挿入ができるようにする．

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [References](#references)
- [Configuration of Hugo section](#configuration-of-hugo-section)
- [Setup of HUGO\_SECTION & HUGO\_BASE\_DIR in ox-hugo](#setup-of-hugo-section-and-hugo-base-dir-in-ox-hugo)
- [References to files outside the static directory](#references-to-files-outside-the-static-directory)

</div>
<!--endtoc-->


## References {#references}

-   [HUGO](https://gohugo.io)  <br />
    Hugo is one of the most popular open-source static site generators. With its amazing speed and flexibility, Hugo makes building websites fun again. Hugoのsetupについてはネットに山のように情報があるので，そちらを参照（手抜き）(^^;;;
-   [ox-hugo](https://ox-hugo.scripter.co)  <br />
    ox-hugo is an Org exporter backend that exports Org to Hugo-compatible Markdown (Blackfriday) and also generates the front-matter (in TOML or YAML format).

    要するに，Markdownを直接書くのではなく，org-modeで書いてしまおうというもので，私のようなorg-mode maniacにピッタリのパッケージである．ox-hugoのsetupについてもネットに山のように情報があるので，そちらを参照（手抜き）(^^;;;


## Configuration of Hugo section {#configuration-of-hugo-section}

**Ref:** [Org-mode で記事を書いて Hugo 向け markdown を ox-hugo で自動生成する話](https://sfus.net/blog/2018/12/org-mode-with-ox-hugo/)

そもそも，まず，Hugoのディレクトリ・ファイルの構成を把握する必要があった．本サイトは，/hogehoge/hogeblog/hogefugablog/hogefugablog.org に書き込んでおり，directory/file構成は以下の通りである．上記参考サイトと同じく，/hogehoge/hogeblog/hogefugablog/，つまり，Hugo の content/ と同じ階層に hogefugablog.org ファイルを置いている．なお，themeは [**academic**](https://themes.gohugo.io/academic/) を使用している．また，ox-hugoのdirectoryは今回の作業により新たに作成されたものであり，当初はなかった．

```shell
$ tree -L 2
.
├── config.toml
├── content
│   ├── home
│   ├── post
│   └── privacy.md
├── data
│   └── 6F
├── hogefugablog.org
├── layouts
│   ├── js
│   ├── partials
│   └── search
├── static
│   ├── css
│   ├── files
│   ├── img
│   └── ox-hugo
└── themes
    └── academic
```


## Setup of HUGO\_SECTION & HUGO\_BASE\_DIR in ox-hugo {#setup-of-hugo-section-and-hugo-base-dir-in-ox-hugo}

**Ref:** [Before you export](https://ox-hugo.scripter.co/doc/usage/#before-you-export)

本サイトでは，HUGO\_SECTIONは特に設定しておらず，C-h v org-hugo-default-section-directoryの値は default valueであるpostsになっている．

また，hogefugablog.orgの文頭に以下のように記述して，HUGO\_BASE\_DIRを設定している．

```lisp
#+HUGO_BASE_DIR: ./
```

ここまでで，ox-hugoからのexportの準備が整った．


## References to files outside the static directory {#references-to-files-outside-the-static-directory}

**Ref:** [References to files outside the static directory](https://ox-hugo.scripter.co/doc/image-links/#references-to-files-outside-the-static-directory)

Hugoのstatic directory以外の場所にあるファイルへのreferenceを作成し，かつ，そのファイルが **org-hugo-external-file-extensions-allowed-for-copying** のリストに挙げられている拡張子を有している場合は，そのファイルはox-hugoによりstatic directoryにコピーされる．ちなみに，C-h v org-hugo-external-file-extensions-allowed-for-copyingとすると，以下のような値を得る．

```lisp
org-hugo-external-file-extensions-allowed-for-copying is a variable defined in ‘ox-hugo.el’.
Its value is
("jpg" "jpeg" "tiff" "png" "svg" "gif" "pdf" "odt" "doc" "ppt" "xls" "docx" "pptx" "xlsx")
```

[Source path does not contain `/static/`](https://ox-hugo.scripter.co/doc/image-links/#source-path-does-not-contain-static)    <br />
このサイトの **Table 2: Where files get copied to if their path does not contain static/** が本サイトに当てはまる．これが分かるまでに時間を要した．本サイトは，/hogehoge/hogeblog/hogefugablog/hogefugablog.orgに書き込んでいる．この環境で，postの中にorg-babelを使ってRのcode blockを評価すると，Rにより作成されるplot（foo.png）は，

```lisp
/hogehoge/hogeblog/hogefugablog/foo.png
```

に作成される．そして，このファイルは，最終的に，

```lisp
/hogehoge/hogeblog/hogefugablog/static/ox-hugo/foo.png
```

にコピーされ，ブログ記事に挿入されるということになる．なお，ox-hugo directoryはこの時に自動的に作成される．

つまり，前回の記事（[How to automatically embed R plot into html exported by org-mode with org-babel](../ExportRplot)）のように，R plotのcode blockを含むorg ファイルを作成し，それをexportして，R plotが自動で組み込まれるようなら，そのorg-babelのcode blockをそのままox-hugoで書いたブログ記事のorg ファイルにコピペすれば，あとはox-hugoが良きにはからってくれるはずである．

実は，できたグラフの画像を自分でいろいろな場所にコピーしては失敗していた．Hugoのroot directory，つまり，/hogehoge/hogeblog/hogefugablog/でRを動かして，できたグラフ画像に対して何もせずに放置しておけば，ox-hugoが全て面倒を見てくれるということに気がつかず，余計なことをしていたわけである．

まとめとして，前回記事のcode blockをこの記事に挿入して試してみる．

```lisp
#+begin_src R :session *R* :results output graphics :file test1.png :exports both
boxplot(islands)
#+end_src
```

{{< figure src="/ox-hugo/test1.png" >}}

```lisp
#+begin_src R :session *R* :results output graphics :file test2.png :exports both
library("ggplot2")
ggplot(iris, aes(x = Sepal.Width, y = Sepal.Length, color = Species)) +
geom_point()
#+END_SRC
```

{{< figure src="/ox-hugo/test2.png" >}}

ちゃんとグラフが自動的に挿入されている．

org-babelとRの組み合わせは強力で，ox-hugoも便利と改めて痛感．
