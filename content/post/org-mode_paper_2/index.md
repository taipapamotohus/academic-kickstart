+++
title = "Emacsのorg-modeで論文を書く（その2：BibDeskによる論文収集と整理）"
author = ["taipapa"]
date = 2018-09-12
lastmod = 2018-09-16T21:44:45+09:00
tags = ["reference", "citation", "bibdesk", "bibtex", "pdf", "pubmed"]
type = "post"
draft = false
weight = 1
[image]
  placement = 3
  caption = "Musée du Louvre"
+++

さて，前回（[Emacsのorg-modeで論文を書く（その1：pdfとhtmlへの出力）](../org-mode_paper_1)）はorg-modeによる論文本体の執筆に付いてまとめたわけだが，今回は論文引用の方法についてまとめる．と思ったのだが，論文を引用するためには，当然のことながら論文を収集しなければならない．そのうち膨大な数の論文の海に溺れることになる．そこで，収集した論文，つまり，pdfの整理をするソフトが必要になってくる．その引用も，書式や番号付を雑誌の規定に合わせて自動化してくれれば言うことはない．ということで，そのようなソフトについて書くことにする．有料ソフトの定番としては，EndoNoteがあるし，フリーソフトでは，[zotero](https://www.zotero.org)や[Mendeley](https://www.mendeley.com/?switchedFrom=)が有名である．私自身は，LaTeXを利用することが多い関係上，[BibDesk](https://bibdesk.sourceforge.io)というこれまた老舗のソフトをもっぱら利用している．ネット上でもzoteroやmendeleyについての情報は豊富だが，BibDeskについてはそれほど見られないので，まとめておくことは多少の意味があるであろうと考え，BibDeskによる論文収集を説明することにした．




## [BibDesk](https://bibdesk.sourceforge.io) {#bibdesk}

-   texliveをインストールすればその中に入っているが，最新版はリンク先にあるので，そちらを落とすほうが良い．
-   文献をbibtexのファイルとして管理する．pdfとの関連付けができるので，書誌事項とpdfが一体化して管理できる．
-   [bibtex](http://www.bibtex.org)に関しては，[BiBTeXとは](https://qiita.com/SUZUKI%5FMasaya/items/14f9727845e020f8e7e9) を参照
-   bibtexファイルなので，当然，latexの機能を用いて，文献の引用，引用スタイルの設定，文献リストの作成，文献リストのスタイルの設定などをすべて自動化できる．これが便利！
-   見た目はこんな感じ

    {{< figure src="/img/bibdesk.jpg" width="100%" target="_self" >}}

-   医学系の論文となると，やはり，[PubMed](https://www.ncbi.nlm.nih.gov/pubmed)などネットでの連携が重要である．下図のごとく，BibDeskではSearches menuからPubMedを選ぶことにより，BibDeskの中からPubMedを検索できる．

{{< figure src="/img/bibdesk-pubmed.jpg" width="100%" target="_self" >}}

-   検索欄に，例えば，"heat shock protein"と打つと，下図のように検索結果が50編ずつ並ぶが，50編以上ある場合は，Searchを繰り返しクリックすることにより，100編と150編とどんどんリストに取り込まれていく．

    {{< figure src="/img/bibdesk-pubmed2.jpg" width="100%" target="_self" >}}

-   上図のごとく，検索結果欄の左端に"Import" ボタンがあるが，これをクリックするとその論文の書誌事項が取り込まれる．その際に，自分の文献リストの名前を，"hogefuga-reference.bib" など適当に決めれば良い．以後はそのリストに追加していくことになる．

-   また，取り込まれる際にcite-keyをBibDeskが自動的に決めてくれる．このcite-keyは次回の記事で述べる「引用の自動化」の際にreftexに使用される．

-   なお，PubMedの番号，つまり，Pmidが分かっていれば，それを打ち込めば一発で書誌事項を検索できる．

-   リストの中から興味のある論文をクリックして選択し，グレーにハイライトさせると，下図のごとく右側のサイドパネルに，その論文のあるサイトを示すアイコンが表示される．これをクリックすれば，ブラウザーが開いてそのサイトに飛ぶ．もし，その論文がオープンアクセスであれば，あるいは，所属する組織が出版社と契約を交わしていれば，その論文のpdfを落とせる．落としたpdfをドラッグしてその論文に該当するリストのラインにドロップすれば，その書誌事項とpdfはリンクし，以降はその論文のサイドパネルにpdfのアイコンが表示され，ダブルクリックによりオープンするようになる．さらに言えば，pdfではなく，パワポやワードのファイルとして文献が存在することもある．同じようにドラッグ＆ドロップすれば，これまたリンクする．しかも一つの文献にいくつものpdfやその他のファイルをリンクできる．非常に便利である．

    {{< figure src="/img/bibdesk-pubmed3.jpg" width="100%" target="_self" >}}

-   また，下図のごとく，左のサイドパネルの一番上の方にある"Web BibDesk Web Group"をクリックして選択すれば，更にいろいろな文献ソースが表示される．医学系では，Google Scholarが有用なので，これをクリックすれば，BibDeskの中からGoogle Scholarを検索できるし，書誌事項も取り込める．pdfのリンクが存在すればBibDeskの中でpdfを落とすこともできる．

    {{< figure src="/img/bibdesk-pubmed4.jpg" width="100%" target="_self" >}}

-   収集した文献の書誌事項はhogefuga-reference.bibにbibtex fileとしてまとめられているが，その中身は以下のような情報の集積である（下の例ではabstractなどは省略している）．

    ```tex
    @article{Rothwell:2018aa,

      Author = {Rothwell, Peter M and Cook, Nancy R and Gaziano, J Michael and Price, Jacqueline F and Belch, Jill F F and Roncaglioni, Maria Carla and Morimoto, Takeshi and Mehta, Ziyah},
      Date-Added = {2018-08-03 22:46:26 +0900},
      Date-Modified = {2018-08-03 22:46:26 +0900},
      Doi = {10.1016/S0140-6736(18)31133-4},
      Journal = {Lancet},
      Journal-Full = {Lancet (London, England)},
      Month = {Jul},
      Pmid = {30017552},
      Pst = {aheadofprint},
      Title = {Effects of aspirin on risks of vascular events and cancer according to bodyweight and dose: analysis of individual patient data from randomised trials},
      Year = {2018},
    }
    ```

-   bibtexなので，前述のごとく，文献の引用，引用スタイルの設定，文献リストの作成，文献リストのスタイルの設定などをすべて自動化できる．

-   Emacsのorg-modeと組み合わせて，どのように文献の引用を自動化するかについては次回の記事にまとめる．
