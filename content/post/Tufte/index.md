+++
title = "Tufte-LaTeXとtufte-org-modeについて"
author = ["taipapa"]
date = 2018-11-14
lastmod = 2018-11-16T22:13:15+09:00
tags = ["tufte", "latex", "org", "mode"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/GrandPlace2.jpg"
  caption = "Grand Place (daytime)"
+++

以前の記事（[Emacsのorg-modeで論文を書く（その1：pdfとhtmlへの出力）](../org-mode_paper_1)）にも書いたが，Tufte−LaTeXなるものを愛用している．今回はこれについてもう少し詳しく書いてみたい．

{{% toc %}}

## [Tufte-LaTeX](https://tufte-latex.github.io/tufte-latex/) {#tufte-latex}

-   [Edward R. Tufte](https://www.edwardtufte.com/tufte/index)によって作られたページレイアウトのためのlatex packageである．典型的には文章が左側に配置され，右側には広いマージンがありノート，文献，表，図などが配置されるスタイルである．
-   さて，このスタイルが何の役に立つかというと，報告書の作成の際に図を入れたりするときに，latexのfloatを使うと案外思うところに挿入されないことがある．このスタイルだと，図は少し小さくなるが，きちんと横に納まってくれるのがよい．
-   [Tufte-LaTeX](https://tufte-latex.github.io/tufte-latex/)のサイトには，"the style of Edward R. Tufte and Richard Feynman"と書かれている．ん，と思って調べてみると，やはり，あの物理学者のファインマンのことであった．興味のある方は，The Feynman-Tufte Principleでググってみると面白いかもしれない．
-   以前の記事（[LaTeXをインストールし，texファイルが変更されると，自動的にcompileしてskimでのpdfも自動で更新されるようにする（2018年9月1日追記）](../latexmk)）に書いたようにtexliveをインストールしてあれば，tufte-latexは一緒にインストールされているので，新たにインストールする必要はない．


## [tufte-org-mode](https://github.com/tsdye/tufte-org-mode) {#tufte-org-mode}

-   [tufte-org-mode](https://github.com/tsdye/tufte-org-mode/blob/master/README.md)は，上述のlatexのtufte classをorg-modeから使えるようにした有り難いパッケージである．このおかげでlatexの記法を意識することなく，org-modeで普通に文章を書いていき，最後に後述する如く，オマジナイを唱えればTufte styleのpdfができあがる．


## 設定 {#設定}

-   下記の2つの設定で使えるようになる．init.orgでの設定の順番はどちらが先でも動く．


### tufte-org-modeのインストールと設定 {#tufte-org-modeのインストールと設定}

-   以下のようにinit.orgに書き込んで設定する．
-   ox-tufte-latex.elは上記の [tufte-org-mode](https://github.com/tsdye/tufte-org-mode)からダウンロードしてローカルに置いてインストールしている．パスは各自の環境に合わせて変更していただきたい．

    ```lisp
    #+begin_src emacs-lisp
    (quelpa
     '(ox-tufte-latex :fetcher file :path "/path/to/ox-tufte-latex.el")
     )
    (use-package ox-tufte-latex)
    #+end_src
    ```
-   quelpaは，use-packageでうまくインストール出来ないときに重宝する．
-   quelpaについては以下を参照
    -   [Quelpa](https://github.com/quelpa/quelpa)
    -   [quelpa.el : 【本邦初公開】MELPAを改善した新しいパッケージ管理システム](http://emacs.rubikitch.com/quelpa/)
    -   [CaskからQuelpaに移行する](https://qiita.com/tadsan/items/99bd9a5bbdb36def13e2)


### org-modeでtufte-latexの設定 {#org-modeでtufte-latexの設定}

-   ox-latexの設定などは以前の記事（[Emacsのorg-modeで論文を書く（その1：pdfとhtmlへの出力）](../org-mode_paper_1)）に書いたようにinit.orgに記述しておく．
-   以下のようにinit.orgに書き込んで設定する．これは以前の記事（[Emacsのorg-modeで論文を書く（その1：pdfとhtmlへの出力）](../org-mode_paper_1)）と重複するが，念のためにここにも書いておく．

    ```lisp
    #+begin_src emacs-lisp
    ;; tufte-handout class for writing classy handouts and papers
    (add-to-list 'org-latex-classes
                 '("tufte-handout"
                   "\\documentclass[twoside,nobib]{tufte-handout}
                                       [NO-DEFAULT-PACKAGES]
                                                                        \\usepackage{zxjatype}
                                                                        \\usepackage[hiragino-dx]{zxjafont}"
                   ("\\section{%s}" . "\\section*{%s}")
                   ("\\subsection{%s}" . "\\subsection*{%s}")))
    ;; tufte-book class
    (add-to-list 'org-latex-classes
                 '("tufte-book"
                   "\\documentclass[twoside,nobib]{tufte-book}
                                      [NO-DEFAULT-PACKAGES]
                                                                       \\usepackage{zxjatype}
                                                                        \\usepackage[hiragino-dx]{zxjafont}"
                   ("\\part{%s}" . "\\part*{%s}")
                   ("\\chapter{%s}" . "\\chapter*{%s}")
                   ("\\section{%s}" . "\\section*{%s}")
                   ("\\subsection{%s}" . "\\subsection*{%s}")
                   ("\\paragraph{%s}" . "\\paragraph*{%s}")))
    #+end_src
    ```
-   ここまでEmacsを設定した上で，orgで原稿を書き，C-c C-eと打てば，以下のような画面になる．

    {{< figure src="/img/org-C-c-C-e.jpg" width="100%" target="_self" >}}

-   ここで，l oと打つと普通（Tufte styleではない）のpdfがオープンされてしまう．
-   Tufte styleのpdfを得るためには，T pと打って，Export to Tufte LaTeX の中からAs PDF file and openを選択しなければならない．これで，原稿が得られ，skimでオープンされる．


## tufteの使用の実例 {#tufteの使用の実例}

-   それでは実例を示してみる．以下のような書類を作成し，Tufte\_test.orgとして保存する．hoge\_fuga.jpgなどの画像は全てTufte\_test.orgファイルと同じdirectoryにあるものとする．

    -   前半の＃で始まる行が続く部分はorg-modeの設定であり，latexのこのパッケージを使うぞ，とか，org-modeのヘッダーをどの深さまで表示するかなどを決めている．詳細はググればすぐに分かるので略.....(^^;;;
    -   :offset -8inによって，図の位置を上にずらしてバランスをとるようにしているのにご注意いただきたい．

    ```lisp
    #+LaTeX_CLASS: tufte-handout
    #+LaTeX_CLASS_OPTIONS: [12pt]
    #+LATEX_HEADER: \usepackage{geometry}
    #+LATEX_HEADER: \geometry{left=0.6in,top=1in,bottom=1in}
    #+LaTeX_HEADER: \usepackage[sort,compress,super,comma]{natbib}
    #+STARTUP:  overview
    #+STARTUP:  hidestars
    #+OPTIONS:   H:4 num:nil toc:nil \n:nil @:t ::t |:t ^:t -:t f:t *:t TeX:t LaTeX:t skip:nil d:nil todo:t pri:nil tags:not-in-toc
    #+LINK_UP:
    #+LINK_HOME:
    #+OPTIONS: author:nil date:nil

    #+begin_fullwidth
    \centering
    #+LATEX: \huge{\textbf{hoge/fugaによる相補的な治療における高難度症例の治療と成績}}
    \vspace{0.5cm}\\
    #+LATEX: \normalsize{taipapa, 織田信長, 豊臣秀吉, 徳川家康}\\
    \vspace{0.5cm}\\
    #+LATEX: \normalsize{hogefuga大学大学院 hogefuga研究科 hogefuga分野}
    #+end_fullwidth

    * *背景と目的*
    hogeとfugaを比較し，治療成績を比較する．
    ​* *結果*
    まず，hogefugaの軽症例の画像を呈示する (*Fig. [[hoge_fuga2]]*)．

    \vspace{0.25cm}

    そのころわたくしは、モリーオ市の博物局に勤めて居りました。

    十八等官でしたから役所のなかでも、ずうっと下の方でしたし俸給もほんのわずかでしたが、受持ちが標本の採集や整理で生れ付き好きなことでしたから、わたくしは毎日ずいぶん愉快にはたらきました。殊にそのころ、モリーオ市では競馬場を植物園に拵え直すというので、その景色のいいまわりにアカシヤを植え込んだ広い地面が、切符売場や信号所の建物のついたまま、わたくしどもの役所の方へまわって来たものですから、わたくしはすぐ宿直という名前で月賦で買った小さな蓄音器と二十枚ばかりのレコードをもって、その番小屋にひとり住むことになりました。わたくしはそこの馬を置く場所に板で小さなしきいをつけて一疋の山羊を飼いました。毎朝その乳をしぼってつめたいパンをひたしてたべ、それから黒い革のかばんへすこしの書類や雑誌を入れ、靴もきれいにみがき、並木のポプラの影法師を大股にわたって市の役所へ出て行くのでした。

    あのイーハトーヴォのすきとおった風、夏でも底に冷たさをもつ青いそら、うつくしい森で飾られたモリーオ市、郊外のぎらぎらひかる草の波。

    またそのなかでいっしょになったたくさんのひとたち、ファゼーロとロザーロ、羊飼のミーロや、顔の赤いこどもたち、地主のテーモ、山猫博士のボーガント・デストゥパーゴなど、いまこの暗い巨きな石の建物のなかで考えていると、みんなむかし風のなつかしい青い幻燈のように思われます。では、わたくしはいつかの小さなみだしをつけながら、しずかにあの年のイーハトーヴォの五月から十月までを書きつけましょう。

    \vspace{0.25cm}

    ついで，hogefugaの重症例の画像を呈示する (*Fig. [[hoge_fuga]]*)．

    #+NAME: hoge_fuga2
    #+caption: hoge-fuga（軽症例である）
    #+attr_latex: :float margin :width 2.8in :offset -8in
    #+attr_latex: :vertical-alignment t
    [[./hoge_fuga2.jpg]]

    #+NAME: hoge_fuga
    #+caption: hoge-fuga（重症例である）
    #+attr_latex: :float margin :width 2.8in :offset -2in
    #+attr_latex: :vertical-alignment t
    [[./hoge_fuga.jpg]]


    * *結論*
    hogefugaによる治療は有効である．
    ```

    -   ついで，前述のごとく，Emacsでこの文書を開いた状態で，C-c C-e T pと打てば，以下のようなpdfがskimで開かれる．

        {{< figure src="/img/Tufte_test_v5.jpg" width="100%" target="_self" >}}

    -   上述のように，:offset の部分で図の位置を調整している．これなしだと，かなり下の方に位置してしまう．

    -   なかなか良い感じになっている．(^o^)

    -   今回は，Tufte styleの紹介であった．実は，このスタイルを手術所見を書くのに使用している．
