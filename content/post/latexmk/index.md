+++
title = "LaTeXをインストールし，texファイルが変更されると，自動的にcompileしてskimでのpdfも自動で更新されるようにする（2018年9月1日追記）"
author = ["taipapa"]
date = 2018-08-19
lastmod = 2018-09-03T22:46:10+09:00
tags = ["emacs", "latex", "pdf", "skim", "beamer", "auctex"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/琵琶湖.jpg"
  caption = "琵琶湖"
+++

{{% toc %}}

## TeX Live 2018のインストール {#tex-live-2018のインストール}

-   参考サイト１：[TeX Wiki TeX Live](https://texwiki.texjp.org/?TeX%20Live)
-   参考サイト２：[TeX Wiki TeX LiveMac](https://texwiki.texjp.org/?TeX%20Live%2FMac)
-   参考サイト３：[The MacTeX-2018 Distribution](http://www.tug.org/mactex/)
-   上記サイトを参考にTeX Live 2018もしくはMacTeX-2018をインストールする．
-   これでスライド作成ソフトであるbeamerも一緒にインストールされる．
-   beamerに固有の設定は特に必要なし．


## TeX Live 2018のインストール（2018年9月1日追記） {#tex-live-2018のインストール-2018年9月1日追記}

大事なことを２つ忘れていたので追記する．


### ヒラギノフォントの埋め込みなど {#ヒラギノフォントの埋め込みなど}

-   参考サイト：[MacTeX 2018のインストール&日本語環境構築法](http://doratex.hatenablog.jp/entry/20180501/1525144736)
-   こんなところを読んでいないで，直ちに上記のサイトに飛んでいただきたい．
-   マックユーザーにとっての最大の問題点は **「macos標準添付のヒラギノフォントなど商用フォントに関する設定ファイル群が，TeX LiveからTLContriに移動になった」** ということである．この問題に対する対応方法は，すべて上記のサイトに記載されているので，そのとおりにすれば良い．実にありがたい．


### インストール後のパスの修正（symbolic linkを張る） {#インストール後のパスの修正-symbolic-linkを張る}

-   参考サイト：[TeX Wiki TeX LiveMac](https://texwiki.texjp.org/?TeX%20Live%2FMac)
-   MacTeX 2018を普通にインストールすると，/usr/local/texlive/2018/bin/x86\_64-darwin/bibtexというふうにbinの下にx86\_64-darwinというdirectoryが入って，tex関連のコマンドのパスが上手く通らなくなる．そこで，上記の[TeX Wiki TeX LiveMac](https://texwiki.texjp.org/?TeX%20Live%2FMac)に書いてあるように，/usr/local/binディレクトリの下にsymbolic linkを張る．

    ```shell
    $ sudo /usr/local/texlive/????/bin/*/tlmgr path add
    ```
-   上記でうまくいかないときは下記のように具体的なディレクトリ名を指定する．

    ```shell
    $ sudo /usr/local/texlive/2018/bin/x86_64-darwin/tlmgr path add
    ```


## auctexのインストールと設定 {#auctexのインストールと設定}

-   latex文書を扱うなら，Emacsのパッケージであるauctexが最強（according to 独断と偏見）
-   preludeをインストールした時点で，auctexもインストールされる．
-   auctexの設定などについては以下を参照（手抜き..... (^^;;; ）
    -   [AUCTEX – Sophisticated document creation](https://www.gnu.org/software/auctex/)
    -   [TeX Wiki AUCTeX](https://texwiki.texjp.org/?AUCTeX)
    -   [Emacs/AUCTeX](https://mytexpert.osdn.jp/index.php?Emacs/AUCTeX)
    -   [AUCTeX の設定と便利な機能](https://skalldan.wordpress.com/2011/07/20/auctex-の設定と便利な機能/)
-   しかし，org-modeを使うようになってからは直接latex文書を打つことはめっきり少なくなってしまった．それでも，学会発表用のスライドなどはlatexを直接打って，beamer文書を作成し，pdf原稿に変換している．


## latexmk {#latexmk}

-   Emacsでbeamerを使って，スライド原稿などを作成していると，C-c C-cするたびにtex文書がcompileされてpdf原稿が出来上がるのを待たなければならず，この待ち時間が結構辛い．そこで，tex ファイルの変更が保存されるたびに自動で裏でlatexmkがcompileしてくれるようにする．latexmkを使う．
-   参考サイト：[TeX Wiki Latexmk](https://texwiki.texjp.org/?Latexmk)
-   texlive2018, MacTeXに入っているので，これらを入れていれば別途インストールする必要なし．
-   ~/.latexmkrcの中に以下のように記述する（ちなみに私はxelatexを使用している）

    ```lisp
    $pdflatex = 'xelatex -interaction=nonstopmode -synctex=1 %O %S';
    $pdf_mode = 1;
    $postscript_mode = $dvi_mode = 0;
    $pdf_previewer  = 'open -a Skim';
    $preview_continuous_mode = 1;  # equivalent to -pvc
    ```


## 実際の作業 {#実際の作業}

-   上記のようにセットアップしていることとする．
-   ターミナルで，texファイルが有るdirectoryに移動し，

    ```shell
    latexmk -pvc -pdf -view=none document.tex
    ```
-   これで，texファイルへの変更を保存すると自動でコンパイルされて，設定にもよるが，skimで開いているpdfファイルも更新される．


## XeLaTeXについて {#xelatexについて}

-   以下を参照されたい．特に最初にあげたZRさんの文書は必読！
    -   [XeLaTeX で日本語する件について](http://zrbabbler.sp.land.to/xelatex.html)
    -   [TeXWiki XeTeX](https://texwiki.texjp.org/?XeTeX)
    -   [XeLaTeXだとかな混植が簡単にできて最高だという話](http://suzusime-log.hatenablog.jp/entry/2017/04/15/210327)
