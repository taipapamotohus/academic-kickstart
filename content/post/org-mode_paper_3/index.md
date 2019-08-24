+++
title = "Emacsのorg-modeで論文を書く（その3：org-modeとbibtexとreftexの連携による文献引用の自動化）"
author = ["taipapa"]
date = 2018-09-15
lastmod = 2018-09-17T16:39:07+09:00
tags = ["reference", "citation", "bibtex", "reftex", "latex", "org", "mode", "automation"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/Paris-4.jpg"
  caption = "Arc de triomphe de l'Étoile"
+++

前回（[Emacsのorg-modeで論文を書く（その2：BibDeskによる論文収集と整理）](../org-mode_paper_2)）は，BibDeskを用いて文献情報をbibtex ファイルとして収集する方法についてまとめた．今回は，この文献情報を利用した引用をEmacsのorg-modeでどのように自動化するかについてまとめる．早い話が，org-modeからlatexのbibtexとreftexの機能を利用するということになる．

{{% toc %}}


## org-mode-reftex-setup {#org-mode-reftex-setup}

-   参照サイト：[Using Emacs Org-mode to Draft Papers](http://www.mfasold.net/blog/2009/02/using-emacs-org-mode-to-draft-papers/)
-   RefTex-ModeというものがEmacsには含まれている．文献や引用の管理のためのパッケージである．詳細はC-h iでマニュアルを見れば分かる，と言いたいところだが，このマニュアルが膨大である．そこで，RefTeX in a Nutshellという2ページほどの要約を読めば，使うのには十分であるとマニュアル自体に書いてある（笑）．実は私はそれすらろくに読んでいないが，以下のように設定すれば，十分に使える．設定方法は，以前の記事（[Emacsの設定（その2）設定ファイル（init.el）をorg-modeで管理する](../init_org)）に記載したとおり，init.orgに書き込めば良い．

    ```lisp
    #+begin_src emacs-lisp
    (defun org-mode-reftex-setup ()
      (load-library "reftex")
      (and (buffer-file-name)
           (file-exists-p (buffer-file-name))
           (reftex-parse-all))
      (define-key org-mode-map (kbd "C-c )") 'reftex-citation)
      )
    (add-hook 'org-mode-hook 'org-mode-reftex-setup)
    #+end_src
    ```
-   上記の設定により，参照サイトの説明のように，org-modeの中でreftex-citationの機能が働くようになる．


### org-mode-reftex-setupの使用方法 {#org-mode-reftex-setupの使用方法}

-   org-modeで文書を書いている最中に文献を引用したい箇所で，C-c ) と打つ
-   すると，まず，どの文献リストを使うかを聞いてくるので，hogefuga-reference.bibなど使いたいリストの名前を打つ．前回説明した方法で収集した文献のbib ファイルが有るはずである．
-   次に，文献を絞り込むためにキーワードを打つように催促されるので，それを打つ．すると，そのキーワードを有する文献のリストがずらずらと並ぶ．下図の例では，stetler と著者名を打ったときの結果が下のバッファに表示されている．該当する文献にカーソルを持ってくるか，クリックして選択し，リターンを押せば決定され，本文の該当箇所にその論文の cite-key，つまり，\cite{Stetler:2012jt} が入力される．

    {{< figure src="/img/reftex.jpg" width="100%" target="_self" >}}

-   上図の下のバッファ内でも，C-sの検索機能は使えるので，さらに絞り込みが必要な際は便利である．以前の記事（[Emacsの設定（その1）Preludeの導入](../prelude_install)）で述べたようにpreludeを導入して，かつ，helmを有効にしておけば，下図のようにC-sでswiperが使えて更に便利である．（なお，下図では，まず，heat shock proteinと打ち，ズラッと並んだ真ん中のバッファでC-sとやってstetlerと打ったところである．一番下のバッファにstetlerで絞り込まれた候補が並んでいる）

    {{< figure src="/img/reftex-2.jpg" width="100%" target="_self" >}}


## [ox-bibtex.el](https://code.orgmode.org/bzg/org-mode/raw/master/contrib/lisp/ox-bibtex.el) {#ox-bibtex-dot-el}

-   参考サイト：[Org and Bibtex](https://aliquote.org/post/org-and-bibtex/)
-   上述の作業で，文献を本文中にcite-keyとして引用することはできた．次に必要なのはorg-modeからpdfやhtmlにexportする際に，cite-keyをもとに，文献が雑誌の規定の様式で引用され，最後に文献リストが規定の様式で記述されるようにすることである．この面倒をみてくれるのが，[ox-bibtex.el](https://code.orgmode.org/bzg/org-mode/raw/master/contrib/lisp/ox-bibtex.el) である．
-   ox-bibtex.elは，org-plus-contrib packageの中に含まれているので，まず，org-plus-contribをインストールする．これは，[package.el](http://emacs-jp.github.io/packages/package-management/package-el.html) を使えば簡単である．
-   次いで，use-packageを使って，ox-bibtexを設定する．と言っても以下のようにinit.orgに書くだけである．

    ```lisp
    (use-package ox-bibtex)
    ```
-   なお，use-packageを使ってox-bibtexを設定する際に，defer t にすると，htmlへの文献のexportができなくなるので注意！
-   use-packageは非常に便利．emacsの新しいパッケージの導入と管理はこれでいいと思う．
    -   参照サイト1：[use-package](https://github.com/jwiegley/use-package)
    -   参照サイト2：[use-packageで可読性の高いinit.elを書く](https://qiita.com/kai2nenobu/items/5dfae3767514584f5220)
-   ox-bibtex.elはbibtexをLaTeX, html, asciiにexportしてくれる．HTMLへのexportには，[bibtex2html](https://www.lri.fr/~filliatr/bibtex2html/) が使われる．そこで，bibtex2htmlをインストールしておく．homebrewがインストールされていれば以下のようにすれば良い．

    ```shell
    brew install bibtex2html
    ```
-   ox-bibtexの使い方はソースの最初に書いてあるとおりである．すなわち，文献をexportするためには，org-mode文書の冒頭に例えば以下を追加し，

    ```lisp
    #+LaTeX_HEADER: \usepackage[sort,compress,super,comma]{natbib}
    ```

    最後に，

    ```lisp
    #+BIBLIOGRAPHY: /Users/taipapa/Documents/hogefuga-references.bib Stroke_3-authors_alphabetical.bst option:-a limit:t
    ```

    を追加する．
-   \#+LaTeX\_HEADER: の行の最後の[natbib](https://ctan.org/pkg/natbib) は，texliveに含まれる文献サポートのパッケージであり，1, 2, 3,....というような番号付タイプの文献引用や author-yearタイプの文献引用の両方に（それ以外にも）対応している．その手前はnatbibのオプションである．
-   \#+BIBLIOGRAPHY: のあとにfoo.bibを書くわけだが，この部分はフルパスで書いて良い．その後にはスタイルを書く．上記のStroke\_3-authors\_alphabetical.bstは自作だが，これは投稿ジャーナルの規定に合わせて作成する．ジャーナルによってはbst ファイルを用意してあるかもしれない．
-   option: -foobar はbibtex2htmlに 'foobar' を渡す．つまり

    ```lisp
    option:-d    sort by date
    option:-a    sort as BibTeX (usually by author) *default*
    option:-u    unsorted i.e. same order as in .bib file
    option:-r    reverse the sort
    ```
-   複数のオプションを使用することも可能

    ```lisp
    option:-d option:-r
    ```
-   上述のように， **limit:t** とすることにより，引用された文献のみのリストになる．これをしないと bib ファイルの中のすべての論文がリストになってしまう．


## bst ファイルについて {#bst-ファイルについて}

-   bibtexにおいて引用のスタイルを決めているファイルであり，これを目指すジャーナルの投稿規定に合わせる．既にそのようなbst ファイルがあれば極楽だが，ない場合は大変である．この辺は以下のサイトを参照．
    -   [LaTeXで参考文献の形式を変更する方法（bstファイルの編集）](http://www.ketsuago.com/entry/2015/03/16/231806)
    -   [BibTeXのドキュメント](http://www.med.osaka-u.ac.jp/pub/anes/www/html/manual/bibtex.html)


### bst ファイルの置き場所 {#bst-ファイルの置き場所}

-   これにはかなり悩まされたが，なんのことはないMacTeXのFAQサイトに書いてあった．
-   [The Most Frequently Asked Questions (FAQ)](http://www.tug.org/mactex/2013/faq/#qm05)

    **QM.06 :** Why can't the latest MacTeX find my local BibTeX files? Earlier versions of MacTeX worked correctly.  <br />
    <br />
     **AM.06 :** TeX Live is slightly pickier about placement of these files. ".bib" files go in <br />
     **~/Library/texmf/bibtex/bib** <br />
     or subfolders of this directory, and ".bst" files go in <br />
     **~/Library/texmf/bibtex/bst** <br />
     or subfolders of this directory.
-   ここにおいておけば，パスを指定することなく，どこからでもbstファイルを指定してスタイルを決められる．


## 文献を引用したorg-modeからのexportの実例 {#文献を引用したorg-modeからのexportの実例}

-   ようやく，これで準備が整ったので，実例を示す．以下のファイルを作成し，hogefuga.orgとして保存する．

```lisp
#+LaTeX_CLASS: koma-jarticle
#+LaTeX_CLASS_OPTIONS: [12pt]
#+LATEX_HEADER: \usepackage{geometry}
#+LATEX_HEADER: \geometry{left=1in,right=1in,top=1in,bottom=1in}
#+LaTeX_HEADER: \usepackage[sort,compress,super,comma]{natbib}
#+STARTUP:  overview
#+STARTUP:  hidestars
#+OPTIONS:   H:4 num:nil toc:nil \n:nil @:t ::t |:t ^:t -:t f:t *:t TeX:t LaTeX:t skip:nil d:nil todo:t pri:nil tags:not-in-toc
#+OPTIONS: date:nil
#+LINK_UP:
#+LINK_HOME:

#+TITLE: hoge/fugaによる相補的な治療における高難度症例の治療と成績
#+AUTHOR: taipapa, 織田信長, 豊臣秀吉, 徳川家康
\vspace*{-1.5cm}

\hspace{2.5cm} hogefuga大学大学院 hogefuga研究科 hogefuga分野

* 背景と目的
hogeとfugaを比較してみると，一方で難易度の高い症例でも他方では容易に行える場合も多い．\cite{Stetler:2012jt}当施設では，一方に片寄ることなく，hogeとfugaを相補的に用いることにより合併症の減少を目指す方針をとっている．そこで，自験例から高難度のhogefuga症例についての方針と成績を主にhogefuga surgeonの立場から検討した．
* 結果
hogefuga症例の画像である (*Fig. [[hoge_fuga]]*)．\cite{Cosentino:2011dn}

#+NAME: hoge_fuga
#+caption: hoge-fuga（重症例である）
#+attr_latex: :float t :width 3in  :align center
#+ATTR_HTML: :width 500  :float: wrap :align center
[[./hoge_fuga.jpg]]

* 結論
hogefugaによる治療は有効である．

#+BIBLIOGRAPHY: /Users/taipapa/Documents/hogefuga-References.bib Stroke_3-authors_alphabetical.bst option:-a limit:t
```

-   hogefuga-References.bibの部分やbstの部分は，それぞれ該当するファイルに置き換えていただきたい．
-   \cite{Stetler:2012jt,Cosentino:2011dn} の部分は私のbibファイルにおけるcite-keyである．
-   C-c C-e l o で，文献が番号付きで引用されたpdfが作成され，skimで開く．
-   下図のように，文献リストも付いているし，本文中の番号をクリックすれば文献リストの該当論文にジャンプするリンク付きである．また，このbstでは著者名のアルファベット順を指定しているので，最初にでてきた文献が2に，二番目にでてきた文献が1になっていることに注意してほしい．さらに，著者名は3人までは全員記載し，4人以上の論文では4人目以降はet alになっている．bibtexの活用により，これらのことが自動的になされている．

    {{< figure src="/img/ref-pdf.jpg" width="100%" target="_self" >}}
-   ついで，htmlである．C-c C-e h o で，文献が番号付きで引用されたhtmlが作成され，browserで開く．
-   下図のように，文献リストも付いているし，本文中の番号をクリックすれば文献リストの該当論文にジャンプするリンク付きである．その他もpdfと同様であるが，文献リストにはabstractやDOIも追加される．投稿する際はpdfか，別記事のようにwordにしてしまうので，html出力の設定はこれ以上触っていない．

    {{< figure src="/img/ref-html.jpg" width="100%" target="_self" >}}
-   ようやく，文献付きの原稿の出力の設定にまでたどり着くことができた．次回はpandocを利用して，org-modeからword ファイルを出力する方法をまとめる．
