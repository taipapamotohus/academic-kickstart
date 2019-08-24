+++
title = "Emacsとskimで，latexのソースとpdfの間を行ったり来たり"
author = ["taipapa"]
date = 2018-10-07
lastmod = 2018-10-08T17:21:48+09:00
tags = ["latex", "pdf", "synchronization", "tex", "emacs", "emacsclient"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/Pharaoh.jpg"
  caption = "Pharaoh"
+++

以前の記事（[beamerでスライド原稿用pdfを作成する（その１）](../beamer)）で，Emacsでbeamerを用いてlatexのソースを書いてコンパイルし，スライド原稿としてpdfを出力する方法をまとめた．この際に，pdfの特定の箇所がlatexのソースでどこに当たるのかがわかったり，逆に，latexのソースの特定の箇所がpdf上のどこに当たるのかがわかったりすると便利である．今回はそれについてまとめる．なお，auctexの全般的な設定については，TeXWikiの[macOS での設定例](https://texwiki.texjp.org/?AUCTeX#h32722ec) を参照していただきたい．

{{% toc %}}

-   参照：[Mac OS X El Capitan の AUCTeX の設定](https://ryogan.org/blog/2015/12/30/mac-os-x-el-capitan-の-auctex-の設定/)
-   参照：[skimとの連携](https://texwiki.texjp.org/?Emacs#e9c08b3d)
-   参照：[AUCTeX の設定と便利な機能](https://skalldan.wordpress.com/2011/07/20/auctex-の設定と便利な機能/)
-   参照：[TeX\_and\_PDF\_Synchronization](https://sourceforge.net/p/skim-app/wiki/TeX%5Fand%5FPDF%5FSynchronization/)


## backward search {#backward-search}

-   こちらのほうが便利なので最初に説明する．
-   skimで表示されたpdf上の特定の箇所に該当するLaTeX文書の箇所を探して示してくれる．
-   この機能を可能にするには，skimの環境設定を開いて「同期する」の初期値を「カスタム」とし，コマンドのところに以下のように打ち込む．

    ```shell
    $ /usr/local/Cellar/emacs-mac/emacs-26.1-z-mac-7.1/bin/emacsclient
    ```
-   defaultでは初期値に「Emacs」となっており，本来ならこれで動くはずだが，homebrewで最新のEmacsをinstallしたために，もともと入っているemacsとはversionが異なる．つまり，サーバーとして起動しているEmacsと、使用するEmacsClientのバージョンが異なることになり，このままでは動かない．そこで，homebrewでインストールした方をfull pathで明示的に指示する必要がある．
-   引数のところには，以下のように打ち込む．

    ```shell
    $ --no-wait +%line "%file"
    ```
-   つまりこうなる．

    {{< figure src="/img/skim-1.jpg" width="100%" target="_self" >}}
-   一方，Emacsのinit.orgには以下のように記述して，Emacs serverを立ち上げておく．

    ```lisp
    #+begin_src emacs-lisp
    ;; Starts the Emacs server
    (server-start)
    #+end_src
    ```
-   これで，pdf上の任意の箇所で，Shift-Command-Clickすると，該当するlatex documentの箇所に飛ぶ．~~もし，Emacsが立ち上がっていなければ，Emacsを立ち上げるところからやってくれる．素晴らしい！~~ これは確かめてみると勘違いであった．Emacsは立ち上げておかないといけない．
-   この機能は知ってしまうと，無くてはならないほど便利に感じる機能である．pdfで間違いを見つけたときに，それがlatexソースのどこに相当するかを同定するのは結構面倒であるが，この機能により一発で同定することができる．
-   該当箇所が少しずれることがあるのが欠点であるが，それでも十分に役に立つ．


## forward search {#forward-search}

-   こちらも，backward searchほどではないが，役に立つ．
-   Emacs上のlatex document上の特定の箇所に該当するpdfの箇所を探して示してくれる．
-   この機能を可能にするには，init.orgに以下のように記述する．

    ```lisp
    #+begin_src emacs-lisp
    (add-hook 'LaTeX-mode-hook
              (function (lambda ()
                          (add-to-list 'TeX-command-list
                                       '("displayline"
                                         "/Applications/Skim.app/Contents/SharedSupport/displayline %n %s.pdf \"%b\""
                                         TeX-run-discard-or-function t t :help "Forward search with Skim"))
                          )))
    #+end_src
    ```
-   これで，C-c C-c displayline により，Emacsのlatex document上の特定の箇所に該当するpdfの箇所に飛んでくれる．pdfの該当するところが赤丸で示される（数秒で消える）．
-   ただし，該当する箇所が結構ずれてしまうことが多い．最近は，beamerでしか使わないので，もしかすると，通常のlatex 文書だと狂いなく示すのかもしれない．まぁ，backward searchと違って， なくても困らない機能である．
