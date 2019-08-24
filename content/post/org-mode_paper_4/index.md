+++
title = "Emacsのorg-modeで論文を書く（その4：pandocを利用してorg-modeからword [docx]を文献付きでexportする）"
author = ["taipapa"]
date = 2018-09-17
lastmod = 2018-09-29T12:37:33+09:00
tags = ["org", "mode", "word", "export", "reference", "citation", "ox", "pandoc"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/BlueMosque.jpg"
  caption = "Sultanahmet Camii"
+++

医学系の多くのジャーナルでは，論文投稿の際のフォーマットを Microsoft Word と指定しているところが多い．．．．．

いま，一瞬，憤りのあまり意識を失いかけたが，気を取り直して続ける．実際のところ，仕事でもしばしばword文書を要求される．イチからwordで文書を書くのはやりたくないわけで，ふと，org-modeからexportできないかと調べてみた．やはり，同じようなことを考える人はいるもので，エライ人はそれを実現させるべく色々な方法を開発していた．それらの中から，自分で試してみてうまく行った方法をまとめておく．使用するのは， **pandoc** とそれをorg−modeから利用するための **ox-pandoc** である．

{{% toc %}}

## ox-pandoc {#ox-pandoc}

-   参照サイト1：本家　[ox-pandoc](https://github.com/kawabata/ox-pandoc)
-   参照サイト2：[ox-pandoc - org-mode + org-ref to docx with bibliographies](http://kitchingroup.cheme.cmu.edu/blog/2015/06/11/ox-pandoc-org-mode-+-org-ref-to-docx-with-bibliographies/)
-   参照サイト3：もっと本家　[Pandoc   a universal document converter](https://pandoc.org)
-   pandoc自体の説明は略．ご存知，フォーマット変換のスイスアーミーナイフ．
-   ox-pandocは，pandocを介してorg-mode fileを様々なフォーマットに変換する新しいexporterであり，40種ものフォーマットに変換できる．
-   pandoc 2.0 (or later version)と，bibliography featureを使うならpandoc-citeproc 0.3 (or later)が必要なので，homebrewでインストールする．

    ```shell
    $  brew install pandoc
    $  brew install pandoc-citeproc
    ```
-   init.orgには以下のように書き込んで，ox-pandocをインストールし，設定する．use-packageを使うと両方がいっぺんにできて便利である．

    ```lisp
    #+begin_src emacs-lisp
    (use-package ox-pandoc
      :ensure t
      :config
      ;; default options for all output formats
      (setq org-pandoc-options '((standalone . t)))
      ;; cancel above settings only for 'docx' format
      (setq org-pandoc-options-for-docx '((standalone . nil)))
      ;; special settings for beamer-pdf and latex-pdf exporters
      (setq org-pandoc-options-for-beamer-pdf '((pdf-engine . "xelatex")))
      (setq org-pandoc-options-for-latex-pdf '((pdf-engine . "xelatex")))
      )
    #+end_src
    ```
-   latex engineにxelatex以外を使っている場合は，上記の設定をそちらに変更する．
-   以上でorg-mode自体の基本的な設定は終了である．


## 文書の中で実際に引用された論文のみからなる文献リストを生成する方法 {#文書の中で実際に引用された論文のみからなる文献リストを生成する方法}

-   このリスト（bib ファイル）を作成しておかないと，Wordをexportする際にうまくいかない．
-   reftex-create-bibtex-fileとbibexportの２つの方法がある．


### reftex-create-bibtex-file {#reftex-create-bibtex-file}

-   参照サイト：[reftex-create-bibtex-file](https://www.gnu.org/software/emacs/manual/html%5Fnode/reftex/BibTeX-Database-Subsets.html)
-   Emacsの中に最初から入っているコマンドである．
-   前回の記事（[Emacsのorg-modeで論文を書く（その3：org-modeとbibtexとreftexの連携による文献引用の自動化](../org-mode_paper_3)）の際に作成したhogefuga.orgからpdfをexportした際に同じdirectory内にhogefuga.texも保存されているはずである．これをEmacsでオープンし, **M-x reftex-create-bibtex-file** と打つ．すると，実際に引用された論文のみからなる文献リストを生成してくれる．この際に名前をどうするかを聞かれるので，適当につける．今回は，extract.bibとした．
-   しかし，たまに，reftex-create-bibtex-fileで引用された論文の一部が欠けてしまうことがある．そのようなときは，第２の方法であるbibexportが役に立つ．


### bibexport {#bibexport}

-   参考サイト1：[bibexport – Extract a BibTEX file based on a .aux file](https://ctan.org/pkg/bibexport)
-   参考サイト2：[Creating .bib file containing only the cited references of a bigger .bib file](https://tex.stackexchange.com/questions/41821/creating-bib-file-containing-only-the-cited-references-of-a-bigger-bib-file)
-   texliveに含まれているshell script
-   文書の中の **\cite** で引用された文献のみを抽出する．上記のreftex-create-bibtex-fileはtex ファイルが有れば抽出できたが，こちらはaux ファイルを必要とする．したがって，まず，org-modeからpdfをexportし，tex ファイルを作成し，次いで，tex ファイルをlatex でコンパイルしてaux ファイルを作成し，そのaux ファイルに対してbibexportを用いるというややこしいことをする必要がある．
-   しかし，reftex-create-bibtex-fileでうまく抽出できないときも，bibexportならうまくいくことが多いので，知っておいて損はない．
-   使い方は以下を参考

    ```sh
    bibexport --help
    ```

    ```sh
    bibexport: a tool to extract BibTeX entries out of .bib files.
    usage: bibexport [-h|v|n|c|a|d|s|t] [-b|e|es|ec|o|r file] file...

    Basic options:
    --------------
     -a, --all                  export the entire .bib files
     -o bib, --output-file bib  write output to file       [default: bibexport.bib]
     -t, --terse                operate silently
     -h, --help                 print this message and exit
     -v, --version              print version number and exit

    Advanced options:
    -----------------
     -b bst, --bst bst          specifies the .bst style file [default: export.bst]
     -c, --crossref             preserve crossref field               [default: no]
     -n, --no-crossref          remove crossref'd entries             [default: no]
     -e bib, --extra bib        extra .bib file to be used (crossrefs and strings)
     -es bib, --extras bib      extra .bib file to be used (for strings)
     -ec bib, --extrac bib      extra .bib file to be used (for crossrefs)
     -p, --preamble             write a preamble at beginning of output
     -r bib, --replace bib      replace .bib file(s) in the .aux file
     -d, --debug                create intermediate files but don't run BibTeX
    ```

-   例えばこんなふうにshellで打つ

    ```sh
    $ bibexport -o extract2.bib hogefuga_English.aux
    ```


## pandocのための設定 {#pandocのための設定}

-   word fileとして出力しても，スタイルが気に入らない可能性は高い．そこで，予めスタイルファイルを自分好みにしておく．
-   参考サイト1：[Defining custom DOCX styles in LibreOffice (and Word)](https://github.com/jgm/pandoc/wiki/Defining-custom-DOCX-styles-in-LibreOffice-(and-Word))
-   参考サイト2：[Customize styles in Word for Mac](https://support.office.com/en-us/article/Customize-styles-in-Word-for-Mac-1ef7d8e1-1506-4b21-9e81-adc5f698f86a)
-   参考サイト3：[ドキュメント変換ツールPandoc：ユーザーズガイドを熟読して分かったマニアックな使い方](https://qiita.com/sky%5Fy/items/5fd5c9568ea550b1d7af)
-   上記のサイトを参考にスタイルファイルを作成し，~/.pandocにword用に **reference.docx** として置く．このテンプレートのフォーマットに従ってword fileが出力される．
-   しかし，実は，これが結構面倒くさいのである．念のため自作のものを[ここ](/files/reference.docx)に置いておく．


## Citation Style Language (CSL)の設定 {#citation-style-language--csl--の設定}

-   参考サイト1：[Citation Style Language](https://citationstyles.org)  ご本家
-   参考サイト2：[citation-style-language/styles](https://github.com/citation-style-language/styles) スタイルの在り処
-   参考サイト3：[citation-style-language/styles/stroke.csl](https://github.com/citation-style-language/styles/blob/master/stroke.csl)  今回使用するスタイル
-   CSLは学術出版の引用と文献スタイルの書式自動化を促進することを目的としたオープンソースプロジェクト．ありがたく使わせていただく．
-   上記の[参考サイト3](https://github.com/citation-style-language/styles/blob/master/stroke.csl)からstroke.cslをダウンロードする．
-   stroke.cslをexportの対象のorg fileと同じdirectoryに置いておく．
-   これで，[Stroke](https://www.ahajournals.org/journal/str) という雑誌の引用書式に従ったスタイルになってword fileがexportされる．


## 英語論文の場合のorg fileの設定 {#英語論文の場合のorg-fileの設定}

-   ここからは，個々のorg-mode file側の設定である．
-   英語の場合は殆どなんの問題もなくexportされる．
-   早速実例を見てみる．まず下のorg fileをhogefuga\_English.orgとして保存する．

    ```lisp
    #+LaTeX_CLASS: koma-article
    #+LaTeX_CLASS_OPTIONS: [12pt]
    #+LATEX_HEADER: \usepackage{times}
    #+LATEX_HEADER: \usepackage{geometry}
    #+LATEX_HEADER: \geometry{left=1in,right=1in,top=1in,bottom=1in}
    #+LaTeX_HEADER: \usepackage[sort,compress,super,comma]{natbib}
    #+STARTUP:  overview
    #+STARTUP:  hidestars
    #+OPTIONS:   H:3 num:nil toc:nil \n:nil @:t ::t |:t ^:t -:t f:t *:t TeX:t LaTeX:t skip:nil d:nil todo:t pri:nil tags:not-in-toc DATE:nil
    #+LINK_UP:
    #+LINK_HOME:

    #+TITLE: Hogefuga profiling to identify distinct changes associated with hogefuga events in hogefuga disease
    #+AUTHOR: taipapa, Nobunaga Oda, Hideyoshi Toyotomi, Ieyasu Tokugawa

     \vspace*{-1.5cm}

          \hspace{2.5cm} Department of Hogefuga, Hogefuga University

    ,* Introduction

    Stroke is estimated to be ranked as the second leading cause of death and the third most common cause of permanent disability around the world.\cite{Donnan:2008ax} The proportion of ischemic stroke is more than 90% in all stroke. The underlying metabolomic pathophysiology of ischemic stroke, however, remains poorly understood.

    Recently, metabolome analysis using “omics” method has developed. Mass spectrometry (MS) and nuclear magnetic resonance (NMR) spectroscopy have garnered the most use for profiling a large number of metabolites simultaneously.\cite{Lewis:2008uq} These technologies offer comprehensive information about thousands of low-molecular mass compounds (less than 2kDa) including lipids, amino acids, peptides, nucleic acids, organic acids, vitamins, thiols and carbohydrates. Metabolomics renders the metabolic profile of a system, the end points of biological events, and reflect the state of a cell or group of cells at a given time.\cite{Gerszten:2008uq} Gas-chromatography/mass-spectrometry (GC/MS) is one of the wide-spread techniques, which enables researchers to determine analyte masses with such high precision and accuracy that peptides and metabolites can be identified unambiguously even in complex fluids.\cite{Lewis:2010oq}

    The profiling of low molecular weight biochemicals that serve as substrates and products in metabolic pathways is particularly relevant to cardiovascular diseases.\cite{Lewis:2008uq} At present, however, very few studies have been reported on metabolic profiling of stroke. Unlike myocardial infarction, metabolomic changes in the brain are not sufficiently reflected by blood biomarkers due to the presence of the blood-brain barrier and dilution by peripheral blood.\cite{kim2013biomarkers} In addition, most of the studies focused on acute stage of stroke.\cite{Jiang:2011uq,Jung:2011fk,Kimberly:2013mq

    #+BIBLIOGRAPHY: /Users/taipapa/Documents/hogefuga-References.bib Stroke_6-authors.bst option:-a limit:t

    ```
-   何故か \*Introductionの前に２つコンマを打たないとうまくhugoでブログにexportされない（理由は不明，ご教示を乞う）．このために画面上 \*Introduction の前にコンマが一つ残っているが，もしコピーして試して見るなら，この余分なコンマは除かないとうまくいかないので注意していただきたい．
-   Emacsで上記のhogefuga\_English.orgを開いた状態で，C-c C-e l oとすると，pdfが作成され，下図のようにskimで開かれる．

{{< figure src="/img/hogefuga_English-pdf.jpg" width="100%" target="_self" >}}

-   本文中に文献番号はついているし，文献リストもStrokeというジャーナルの投稿規定通り6人までの著者名は提示し，それ以上はet al. になっている．
-   何故pdfを作成するかというと， **文書の中で実際に引用された論文のみからなる文献リストを生成する** ためである．pdfと一緒にできたtex ファイルで，reftex-create-bibtex-file もしくは bibexportを使って抽出された文献リストであるextract.bibを作成する．
-   実際に行った手順は以下の通りである．
    1.  pdfのexportの際に一緒に生成された **hogefuga\_English.tex** をEmacsで開く．

    2.  **M-x reftex-create-bibtex-file** する

    3.  抽出された文献ファイルをextract.bibと命名し保存

    4.  しかし，extract.bibは何故か引用された8つの文献のうちの6つしか含まれていなかった．

    5.  そこで，上述のように **bibexport** を利用することにした．texファイルをxelatexでコンパイルし，できたaux ファイルにbibexportを適用した．[LaTeXをインストールし，texファイルが変更されると，自動的にcompileしてskimでのpdfも自動で更新されるようにする（2018年9月1日追記）](../latexmk)のlatexmkの項を参照のこと．

        ```sh
        $ latexmk -pvc -pdf -view=none hogefuga_English.tex
        $ bibexport -o extract2.bib hogefuga_English.aux
        ```

    6.  これで，８つの文献をすべて含むextract2.bibが生成された．


## 英語論文のWord fileのexport {#英語論文のword-fileのexport}

-   ようやくWord fileへexportできる段階となった．
-   上記で作成したhogefuga\_Engolish.orgをEmacsでオープンし，冒頭に以下の3行を追加する．1行目は引用のスタイルファイルを指定し，2行目はWordのスタイルファイルを指定し，3行目は文書の中で実際に引用された文献のみのリストを指定している．この文献リストはorg-modeと同じdirectoryに置いておく．多分パスも効くが，この原稿専用のリストなので，同じdirectoryの方が混乱することがないであろう．

    ```lisp
    #+PANDOC_OPTIONS: csl:/Data/hoge/fuga/stroke.csl
    #+PANDOC_OPTIONS: reference-doc:~/.pandoc/reference.docx
    #+BIBLIOGRAPHY: extract2.bib
    ```

-   さらに，最後の文献についての以下の部分は削除する．

    ```lisp
    #+BIBLIOGRAPHY: /Users/taipapa/Documents/hogefuga-References.bib Stroke_3-authors_alphabetical.bst option:-a limit:t
    ```

-   以上で，下図のようになるので，hogefuga\_English\_WORD.org として保存する．

    ```lisp
    #+LaTeX_CLASS: koma-article
    #+LaTeX_CLASS_OPTIONS: [12pt]
    #+LATEX_HEADER: \usepackage{times}
    #+LATEX_HEADER: \usepackage{geometry}
    #+LATEX_HEADER: \geometry{left=1in,right=1in,top=1in,bottom=1in}
    #+LaTeX_HEADER: \usepackage[sort,compress,super,comma]{natbib}
    #+STARTUP:  overview
    #+STARTUP:  hidestars
    #+OPTIONS:   H:3 num:nil toc:nil \n:nil @:t ::t |:t ^:t -:t f:t *:t TeX:t LaTeX:t skip:nil d:nil todo:t pri:nil tags:not-in-toc DATE:nil
    #+PANDOC_OPTIONS: csl:/Data/Stroke2018/Survival_CEA_CAS-MN/stroke.csl
    #+PANDOC_OPTIONS: reference-doc:~/.pandoc/reference.docx
    #+BIBLIOGRAPHY: extract2.bib
    #+LINK_UP:
    #+LINK_HOME:

    #+TITLE: Hogefuga profiling to identify distinct changes associated with hogefuga events in hogefuga disease
    #+AUTHOR: taipapa, Nobunaga Oda, Hideyoshi Toyotomi, Ieyasu Tokugawa.

             \vspace*{-1.5cm}

                  \hspace{3cm} Department of Hogefuga, Hogefuga University

    ,* Introduction
      Stroke is estimated to be ranked as the second leading cause of
      death and the third most common cause of permanent disability
      around the world.\cite{Donnan:2008ax} The proportion of ischemic
      stroke is more than 90% in all stroke. The underlying metabolomic
      pathophysiology of ischemic stroke, however, remains poorly
      understood.

      Recently, metabolome analysis using “omics” method has
      developed. Mass spectrometry (MS) and nuclear magnetic resonance
      (NMR) spectroscopy have garnered the most use for profiling a large
      number of metabolites simultaneously.\cite{Lewis:2008uq} These
      technologies offer comprehensive information about thousands of
      low-molecular mass compounds (less than 2kDa) including lipids,
      amino acids, peptides, nucleic acids, organic acids, vitamins,
      thiols and carbohydrates. Metabolomics renders the metabolic profile
      of a system, the end points of biological events, and reflect the
      state of a cell or group of cells at a given
      time.\cite{Gerszten:2008uq} Gas-chromatography/mass-spectrometry
      (GC/MS) is one of the wide-spread techniques, which enables
      researchers to determine analyte masses with such high precision and
      accuracy that peptides and metabolites can be identified
      unambiguously even in complex fluids.\cite{Lewis:2010oq}

      The profiling of low molecular weight biochemicals that serve as
      substrates and products in metabolic pathways is particularly
      relevant to cardiovascular diseases.\cite{Lewis:2008uq} At present,
      however, very few studies have been reported on metabolic profiling
      of stroke. Unlike myocardial infarction, metabolomic changes in the
      brain are not sufficiently reflected by blood biomarkers due to the
      presence of the blood-brain barrier and dilution by peripheral
      blood.\cite{kim2013biomarkers} In addition, most of the studies
      focused on acute stage of
      stroke.\cite{Jiang:2011uq, Jung:2011fk, Kimberly:2013mq}
    ```

-   \*Introductionの前のコンマについては前述のとおりである．


### org-modeからWord fileへのexportの方法 {#org-modeからword-fileへのexportの方法}

-   ここで，C-c C-e とすると，exportのバッファが表示される．C-nで下の方まで下がると，下図のように, **export via pandoc** のメニューが見える．そこで，p xとして，export via pandoc ---> to docx and openを選択する．

    {{< figure src="/img/org-C-c-C-e.jpg" width="100%" target="_self" >}}

-   暫く待つと，下図のようにWordが立ち上がって，docx file（ **hogefuga\_English\_WORD.docx** ）が開かれる．

    {{< figure src="/img/hogefuga_English-WORD.jpg" width="100%" target="_self" >}}

    -   全体的なスタイルはまずまずである．

    -   本文中に文献番号はついているし，文献リストもStrokeというジャーナルの投稿規定通り6人までの著者名は提示し，それ以上はet al. になっている．

    -   文献リストの体裁はインデントに問題ありだが，これは手作業でやっても苦痛でないレベルである．

    -   org-modeのオプションが見えてしまっているが，この程度であれば僅かな手作業で消去できる．

    -   **英語に関しては，pdfと比べると多少見劣りがするが，まず問題ないレベルのWord fileが出力できた．**


## 日本語論文の場合のorg fileの設定 {#日本語論文の場合のorg-fileの設定}

-   前回の記事（[Emacsのorg-modeで論文を書く（その3：org-modeとbibtexとreftexの連携による文献引用の自動化](../org-mode_paper_3)）の際に作成したhogefuga.orgをEmacsでオープンし，冒頭に以下の3行を追加する．1行目は引用のスタイルファイルを指定し，2行目はWordのスタイルファイルを指定し，3行目は文書の中で実際に引用された文献のみのリストを指定している．この文献リストはorg-modeと同じdirectoryに置いておく．

    ```lisp
    #+PANDOC_OPTIONS: csl:/Data/hoge/fuga/stroke.csl
    #+PANDOC_OPTIONS: reference-doc:~/.pandoc/reference.docx
    #+BIBLIOGRAPHY: extract.bib
    ```

-   さらに，最後の文献についての以下の部分は削除する．

    ```lisp
    #+BIBLIOGRAPHY: /Users/taipapa/Documents/hogefuga-References.bib Stroke_3-authors_alphabetical.bst option:-a limit:t
    ```

-   以上で，下図のようになるので，hogefuga\_WORD.org として保存する．

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
#+PANDOC_OPTIONS: csl:/Data/hoge/fuga/stroke.csl
#+PANDOC_OPTIONS: reference-doc:~/.pandoc/reference.docx
#+BIBLIOGRAPHY: extract.bib
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

```


## org-modeからWord fileへのexportの方法 {#org-modeからword-fileへのexportの方法}

-   ここで，C-c C-e とすると，exportのバッファが表示される．C-nで下の方まで下がると，下図のように, **export via pandoc** のメニューが見える．そこで，p xとして，export via pandoc ---> to docx and openを選択する．

    {{< figure src="/img/org-C-c-C-e.jpg" width="100%" target="_self" >}}

-   暫く待つと，下図のようにWordが立ち上がって，docx file（ **hogefuga\_WORD.docx** ）が開かれる．

    {{< figure src="/img/word-1.jpg" width="100%" target="_self" >}}

    -   本文中に文献番号はついているし，文献リストもStrokeというジャーナルの投稿規定通り6人までの著者名は提示し，それ以上はet al. になっている．

    -   Figureのキャプションが消えているが，通常，論文投稿時には，本文と画像は別々になるので，画像自体を本文から削除できるため，問題無しとする．

    -   しかし，英語の場合には見られなかった大きな問題が発覚した！　本文が,  **濁点分離** してしまっている．


## Word file の濁点分離を修正する方法 {#word-file-の濁点分離を修正する方法}

-   **濁点分離** したままでは使いものにならないので，修正する必要がある．しかし，Word fileの内容を点検して，いちいち手作業をやっていては堪らない．そこで，一気に濁点分離を修正する方法はないものかといろいろ探ってみた．
-   参考サイト1：[Wordで文書内の文字をUnicode NFC正規化する方法](https://ja.stackoverflow.com/questions/36762/wordで文書内の文字をunicode-nfc正規化する方法)
-   参考サイト2：[あらゆる文字に濁点と半濁点を付けてみよう](http://labs.timedia.co.jp/2018/04/post-57.html)
-   参考サイト3：[Macの濁点問題を解決するPython unicodedataモジュール](http://ikeikeikeike.hatenablog.com/entry/2013/11/20/121930)
-   参考サイト4：[濁点問題](http://emasaka.blog65.fc2.com/blog-entry-1407.html)
-   参考サイト5：[濁点の話](https://www.slideshare.net/emasaka/ss-82692529)
-   参考サイト6：[docx-normarize-nfc](https://github.com/emasaka/docx-normarize-nfc)
-   上記の参考サイト4, 5, 6では，emasaka氏により，pythonを用いた方法が報告されており，[docx-normarize-nfc](https://github.com/emasaka/docx-normarize-nfc) としてGithubにアップされている．これはpython scriptであり，.docxファイルをZIPアーカイブとして開き、文書本体のXMLテキストを開いてNFC正規化し、ZIPアーカイブに書き戻すというものであり，これを使わせてもらうことにした．


### Pythonの導入 {#pythonの導入}

-   参考サイト：[Welcome to Python.org](https://www.python.org) （本家）ご存知いま一番アツい言語．それしか知らなくても下記のようにして使える（笑）
-   まず下準備としてpythonを入れる．
-   homebrew でpython3をインストール

    ```shell
    $ brew install python3
    ```


### docx-normarize-nfcの導入 {#docx-normarize-nfcの導入}

-   [docx-normarize-nfc](https://github.com/emasaka/docx-normarize-nfc) からダウンロードして，/usr/local/bin/ にコピーする．（/usr/local/binにパスが通っているものとする）


### 濁点分離の修正 {#濁点分離の修正}

-   これでWord fileに対して上記のスクリプトを使用すれば良い．
-   念のために，Word fileの名前を，hogefuga\_WORD\_濁点分離修正済み.docxに変更し新規保存しておく．
-   そのうえで，shellで以下の操作を行う

    ```shell
    $ docx-normarize-nfc hogefuga_WORD_濁点分離修正済み.docx
    ```
-   一瞬で修正は終わるので，ファイルをオープンして確かめてみると，下図のように修正されている．素晴らしい．

    {{< figure src="/img/word-fixed.jpg" width="100%" target="_self" >}}
-   ようやく，使い物になる日本語のWord fileを作成することができた．
-   これで，英語でも日本語でも，pdfからWordにコピペして修正するという難行苦行から解放される．
-   しかし，co-authorとのすり合わせやrevisionの際は，まだ，Wordでの作業が必要とされる．苦行は続くのである．．．．．
