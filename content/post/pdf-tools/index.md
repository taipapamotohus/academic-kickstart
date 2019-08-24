+++
title = "Emacsでpdfを読む (pdf-tools) (2019.07.17追記)"
author = ["taipapa"]
date = 2019-07-17
lastmod = 2019-07-24T21:57:23+09:00
tags = ["emacs", "pdf", "pdf-tools", "org-mode"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/Templum_Veneris_et_Romae.jpg"
  caption = "Templum Veneris et Romae"
+++

Emacsでpdf文書を読もうとするとdefaultではDocViewで読むことになるが，解像度がイマイチで動きもややモッサリとしていて使い勝手が悪かったため，サクッと止めて，skimを使っている．ただ，pdf-toolsというのがあって，こちらは割とスグレモノらしいとの噂は聞いていた．そこで，今回はこれを試してみることにした．

<!--more-->

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [PDF Tools](#pdf-tools)
    - [インストール](#インストール)
    - [追記（2019年7月17日）](#追記-2019年7月17日)
    - [設定](#設定)
    - [使用法](#使用法)
- [org-pdfview](#org-pdfview)
    - [インストールと設定](#インストールと設定)
- [PDF Toolsと他のアプリ（skim, Previewなど）との比較](#pdf-toolsと他のアプリ-skim-previewなど-との比較)

</div>
<!--endtoc-->


## PDF Tools {#pdf-tools}

-   参考１：[pdf-tools](https://github.com/politza/pdf-tools)　ご本家
-   参考２：[emacsをPDF Viewerにしよう](http://blog.livedoor.jp/hiroaki8270/archives/22871970.html)
-   参考３：[emacs内でTeX文書の作成からpdf閲覧まで行う](https://ubutun.blogspot.com/2014/05/emacstexpdf.html)
-   参考４：[pdf-tools on macos](https://www.reddit.com/r/emacs/comments/6x9gtb/pdftools%5Fon%5Fmacos/)
-   参考５：[VIEW AND ANNOTATE PDFS IN EMACS WITH PDF-TOOLS](http://pragmaticemacs.com/emacs/view-and-annotate-pdfs-in-emacs-with-pdf-tools/)
-   参考６：[MORE PDF-TOOLS TWEAKS](http://pragmaticemacs.com/emacs/more-pdf-tools-tweaks/)
-   参考７：[EVEN MORE PDF-TOOLS TWEAKS](http://pragmaticemacs.com/emacs/even-more-pdf-tools-tweaks/)
-   参考８：[pdf-tools pretty much unusable with linum-mode enabled](https://github.com/politza/pdf-tools/issues/189)
-   参考９：[Using Emacs 44 - An Org mode and PDF-tools workflow](https://www.youtube.com/watch?v=LFO2UbzbZhA)

ご本家のイントロに書いてあるが，DocViewのようにghostscriptで予めrenderしておくのではなく，on demandでページを作成し，メモリーに貯めておく仕組みになっている．このrenderingは，popplerという名前の特別なライブラリーによって行われるが，これはepdfinfoと呼ばれるserver programの中で走っている．こいつの仕事はEmacsからの要求を連続して読んで適切な結果，すなわち，PDFのページのPNG imageを作成することである．

　　「実際のところ，PDFファイルを表示するのはPDF toolsの仕事の一部に過ぎない．popplerは文書に関する全ての情報を提供でき，かつ，それを修飾もできるので，遥かにたくさんのことができる」とイントロの最後で大見得を切って，何ができるかを示す[動画](https://www.dailymotion.com/video/x2bc1is?forcedQuality=hd720)を紹介している．


### インストール {#インストール}

OSXは公式にはサポートされていないが，コンパイルできたと報告されている，と書いてあり，実際，以下のように出来た．まず，homebrewでpopplerをインストールする．もし，まだ，automakeを入れていなければそれもhomebrewでインストールする．

```sh
$ brew install poppler automake
```

ついで，pkg-configをexportでいじるようなことが書いてあるが，特にそれはせずともよかった．ただし，pdf-toolsのインストールの際にコンパイルエラーが出た．どういうわけか， **pdf-tools          20180428.1527** ではだめだったが，幸い， **pdf-tools          20181221.1913** が出たので，参考4：[pdf-tools on macos](https://www.reddit.com/r/emacs/comments/6x9gtb/pdftools%5Fon%5Fmacos/)を頼りに，これにアップデートしたところ，あとは問題なくインストールできた．


### 追記（2019年7月17日） {#追記-2019年7月17日}

その後何度かpdf-toolをコンパイルすることがあったが，「libffiがどこにあるか分からん」というようなエラーメッセージが出て，「pkg-configでなんとかせい」と怒られるようになった．つまり，ご本家に書いてある通りになったわけである．そこで，libffiのpkgconfigを探して，それをPKG\_CONFIG\_PATHに含めるようにした．

```sh
$ mdfind -name pkgconfig | grep libffi
/usr/local/Cellar/libffi/3.2.1/lib/pkgconfig
$ export PKG_CONFIG_PATH=/usr/local/Cellar/libffi/3.2.1/lib/pkgconfig
$ /Applications/Emacs.app/Contents/MacOS/Emacs --debug-init
```

これで下記のように設定していると，以下のようにpdf-toolが無事にコンパイルされる．

```lisp
/Users/taipapa/.emacs.d/elpa/pdf-tools-20190413.2018/build/server/autobuild -i /Users/taipapa/.emacs.d/elpa/pdf-tools-20190413.2018/
---------------------------
Installing packages
---------------------------
Skipping package installation (already installed)

---------------------------
Configuring and compiling
---------------------------
./configure -q --bindir=/Users/taipapa/.emacs.d/elpa/pdf-tools-20190413.2018/ && make -s

Is case-sensitive searching enabled ?     yes
Is modifying text annotations enabled ?   yes
Is modifying markup annotations enabled ? yes


---------------------------
Installing
---------------------------
make -s install
/usr/local/bin/gmkdir -p '/Users/taipapa/.emacs.d/elpa/pdf-tools-20190413.2018'
/usr/local/bin/ginstall -c epdfinfo '/Users/taipapa/.emacs.d/elpa/pdf-tools-20190413.2018'
make[1]: Nothing to be done for `install-data-am'.

===========================
Build succeeded. :O)
===========================
```


### 設定 {#設定}

例によって，use-packagを用いて以下のように，init.orgに書けばよい．

```lisp
#+begin_src emacs-lisp
(use-package pdf-tools
  :ensure t
  :config
  ;; initialise
  (pdf-tools-install)
  ;; PDF Tools does not work well together with linum-mode
  (add-hook 'pdf-view-mode-hook (lambda() (nlinum-mode -1)))
  ;; open pdfs scaled to fit page
  ;; (setq-default pdf-view-display-size 'fit-page)
  ;; automatically annotate highlights
  (setq pdf-annot-activate-created-annotations t)
  ;; use normal isearch
  (define-key pdf-view-mode-map (kbd "C-s") 'isearch-forward)
  ;; more fine-grained zooming
  (setq pdf-view-resize-factor 1.1)
  )
#+end_src
```

以下に内容を説明する．

-   pdf-tools-installにより最初のときにepdfinfoがコンパイルされる．
-   行番号を表示するとうまく動かない．私はnlinum-modeを使っているのでpdf-view-modeの際には止めておく．
-   ハイライトした部分には自動的に注釈を加える．
-   swiperはうまく動かないので，C-sを普通のisearchに戻す
-   ＋とーで拡大，縮小だが，これを10%ずつにする．


### 使用法 {#使用法}

上記のインストールと設定を行えば，C-x C-fでも，drag & dropでも，Emacsのpdf toolsのpdf-view-modeでpdfが開くようになる．ここまでくれば，あとは色々なことができる．


#### highlight {#highlight}

マウスで文章をなぞって選択（下の画像の白黒反転した部分）したあとに，C-c C-a h もしくは，画像で示したように，PDF Tools &rarr; Add markup annotation &rarr; highlightを選択すれば，

{{< figure src="/img/pdf-tools-2.jpg" width="100%" target="_self" >}}

選択した部分がハイライトされ，下に新たなバッファが開いてそこに注釈が書けるようになる（下の画像参照）．書き終わったら，C-c C-cで注釈バッファが閉じる．なお，上に元からある黄色にハイライトされた部分は以前に選択してハイライトした部分である．

{{< figure src="/img/pdf-tools-3.jpg" width="100%" target="_self" >}}


#### Display Annotations {#display-annotations}

複数箇所をハイライトして注釈をつけたあとに，全ての注釈を一度にリストにしてみることができる．C-c C-a l もしくは，PDF Tools &rarr; Display Annotationsを選択すれば，下の画像のように，下に２つの新たなバッファが開く．真ん中のバッファに注釈のリストが表示される．arrow keyでリスト内を移動し，スペースキーを押すと上のバッファでその注釈のところに移動してブルーの枠で囲んで表示され，下のバッファに注釈の内容が表示される．qを押せば，2つのバッファは閉じる．

{{< figure src="/img/pdf-tools-4.jpg" width="100%" target="_self" >}}


#### Isearch document (C-s) {#isearch-document--c-s}

pdf-toolsはswiperとはconflictするために，C-sは本来のisearch-forwardに戻して設定しておく必要がある（前述の設定の通り）．これで，C-sとやると，minibufferに打ち込んだ語が反転して表示され，C-sとやるごとに先へ移動していく．下の画像では，"MK2"という単語を打った時の状態を示している．

{{< figure src="/img/pdf-tools-C-s.jpg" width="100%" target="_self" >}}


#### Occur document {#occur-document}

PDF Tools &rarr; Occur documentを選択すれば，minibufferに List lines matching PCRE: と表示される．そこに例えばMK2と打てば，下の画像のように，下に新たなバッファが開き，MK2のあるページとそこにある文章のリストが表示される．arrow keyでリスト内を移動し，スペースキーを押すと上のバッファでその注釈のところに移動する．qを押せば，下のバッファは閉じる．

{{< figure src="/img/pdf-tools-occur.jpg" width="100%" target="_self" >}}


## org-pdfview {#org-pdfview}

org-modeからpdf文書へのリンクを開くのをサポートするパッケージ．

-   参考１：[org-pdfview](https://github.com/markus1189/org-pdfview)
-   参考２：[How to use pdf-tools (pdf-view-mode) in emacs?](https://emacs.stackexchange.com/questions/19686/how-to-use-pdf-tools-pdf-view-mode-in-emacs)
-   参考３：[Configure org-pdfview and pdf-tools to open at page](https://emacs.stackexchange.com/questions/31895/configure-org-pdfview-and-pdf-tools-to-open-at-page)


### インストールと設定 {#インストールと設定}

例によって，use-packagを用いて以下のように，init.orgに書くだけ．

```lisp
#+begin_src emacs-lisp
(use-package org-pdfview
  :ensure t)
#+end_src
```

例えば，Emacsのpdf-toolsを用いて，hogehoge.pdfを開いて読んでいるとする．そこで，C-c lとすると， **Stored: /Data/Hoge/Fuga/hogefuga.pdf** と今読んでいるpdfへのリンクが保存される．そして，それを保存しておきたいorg文書の適当な場所で，C-c C-lとすれば，そのリンクが貼り付けられる．以前の記事（[Org-modeでhtml exportの際のthemeについて](../org-html-export-theme)）で書いたような文献のまとめを作成しているときに，元文献とリンクさせておく際などに便利である．貼り付けたリンクをクリックすれば，元文献がEmacsのpdf-toolsによって開かれるようになる（画面が分割され，下に新たなバッファが開いてそこにpdfが表示される）．割と便利である．


## PDF Toolsと他のアプリ（skim, Previewなど）との比較 {#pdf-toolsと他のアプリ-skim-previewなど-との比較}

-   注釈の一覧表示，C−s， occurなどの機能は便利である．
-   skim, Previewなどでは，長方形ツールによりお好みの領域を選択してコピーすることができるし，このコピーした領域のみをpdfとして保存できるが，pdf-toolsではできない．
-   skim, Preview, Adobe Acrobat Readerのように，全画面でプレゼンテーションするモードはない．
-   Adobe Acrobat Readerのように，動画を動かすことは出来ない．
-   上記２つの理由から，auctexを使用する際のpdf viewerとしてEmacsを使用していない．
-   最近のpdfは，本文中に示された文献もしくはその番号をクリックすると，最後の文献リストの中の該当の論文のところに飛ぶようになっているものも多くなっているが，pdf-toolsはそれには対応していないようである．より正確にはリンク先が分からないようである．これは自力では解決できない．．．
-   Outline構造にも対応しており，検出するのだが，リンク先が分からないようである．これも自力では解決できない．．．

以上のことより，学会発表用のスライドなどを作成している際は，skimなどの方がpdf viewerとして便利であるが，文献のまとめなどpdfを読み込む際には，pdf-toolsの方が向いているのではないかと考えている．
