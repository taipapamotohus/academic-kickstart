+++
title = "How to automatically embed R plot into html exported by org-mode with org-babel"
author = ["taipapa"]
date = 2019-03-25
lastmod = 2019-03-30T12:01:01+09:00
tags = ["org-babel", "emacs", "export", "R", "plot", "graph", "org-mode"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/Wikiki-2.jpg"
  caption = "Wikiki"
+++

本サイトは，ox-hugoで書いてHugo用のMarkdownをexportすることにより作成しているが，前回の記事（[How to plot survival curve of competing risk analysis with censoring mark and number at risk (at risk table)](../prodlim)）を書いている際に，Rでplotしたgraphを記事の中に自動ではめ込むよう設定するのに苦労したので，これも忘れないうちにまとめておく．まず，今回はorg-babelの設定について書き，次回にHugoでの設定をまとめる．

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [Org-babel setup](#org-babel-setup)
- [How to use org-babel](#how-to-use-org-babel)
- [Org-babel evaluation of R code block](#org-babel-evaluation-of-r-code-block)

</div>
<!--endtoc-->


## Org-babel setup {#org-babel-setup}

org-babelとは，う～～～ん，なにもの？　ものすごく端折って言うと，Code blockを評価して結果を表示するorg-modeの拡張，といったところだろうか．．．実例を見たほうが早いと思う．今回，org-babelによる R code の評価について書こうとして，ふと，ブログを見直してみると，なんとorg-babelの設定をまとめた記事を投稿してない .....(^^;;;

ということで，org-babelの設定を改めて記しておく．例によって，init.orgに以下のように書き込んでおけばよい．

**Ref:** [Org-babel Setup](http://doc.norang.ca/org-mode.html#OrgBabel)　ここからコピペ  (^^;;;

```lisp
#+begin_src emacs-lisp
(org-babel-do-load-languages
 (quote org-babel-load-languages)
 (quote ((emacs-lisp . t)
         (dot . t)
         (ditaa . t)
         (R . t)
         (python . t)
         (ruby . t)
         (gnuplot . t)
         (clojure . t)
         (shell . t)
         (ledger . t)
         (org . t)
         (plantuml . t)
         (latex . t))))
#+end_src
```


## How to use org-babel {#how-to-use-org-babel}

以下のサイトを参考にした．

**Ref 1:** Official manual [14 Working with Source Code](https://orgmode.org/manual/Working-with-Source-Code.html#Working-with-Source-Code)

**Ref 2:** [org-modeのコードブロック(Babel)の使い方](http://misohena.jp/blog/2017-10-26-how-to-use-code-block-of-emacs-org-mode.html)   <br />
このサイトが分かりやすい．特に， **ヘッダー引数** と **言語毎の書き方** の **R** の項は必読．


## Org-babel evaluation of R code block {#org-babel-evaluation-of-r-code-block}

[R and Emacs with org mode](http://blogs.neuwirth.priv.at/software/2012/03/28/r-and-emacs-with-org-mode/)   <br />
org-babelによるR codeの評価とhtmlへのgraph plotの自動埋め込みは，このサイトが分かりやすい．ここに有る”Using org mode with R”というサンプルを参考に，以下のようなorgファイルを/Data/hogehoge/hogefugaに作成する．

```lisp
#+TITLE: R-test
#+AUTHOR: taipapa

* Test

  #+begin_src R :session *R* :results output graphics :file test1.png :exports both
  boxplot(islands)
  #+end_src

  #+begin_src R :session *R* :results output graphics :file test2.png :exports both
  library("ggplot2")
  ggplot(iris, aes(x = Sepal.Width, y = Sepal.Length, color = Species)) +
  geom_point()
  #+END_SRC
```

C-c C-e h oとしてhtmlにexportすると，以下のように簡単にグラフがプロットされたhtmlが作成される．いちいちできたグラフ画像を挿入する必要はなく，自動で挿入される．便利である．

{{< figure src="/img/R-test-html.png" width="100%" target="_self" >}}

　　注意点としては，C-c C-e hoとしたときに， **R starting project directory？** と尋ねられるはずで，defaultの値として　/Data/hogehoge/hogefuga/ が既に表示されているはずである．これをそのままリターンすれば同じdirectoryにグラフが作成されて良きにはからってくれる．この際に異なるdirectoryを選んだりするとうまくいかないので注意．

また，C-c C-e loとすると，自動でR plotの挿入されたpdfが作成されオープンする．

これで準備が整った．次回はHugoで作成したブログにR plotを自動で差し込む方法をまとめる予定である．
