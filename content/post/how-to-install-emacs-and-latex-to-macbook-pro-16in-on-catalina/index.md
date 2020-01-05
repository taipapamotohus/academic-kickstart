+++
title = "How to install Emacs & LaTeX to MacBook Pro 16-inch on Catalina"
author = ["taipapa"]
date = 2019-12-31
lastmod = 2020-01-05T22:56:58+09:00
tags = ["macbookpro16", "Emacs", "LaTeX", "Catalina", "setup"]
type = "post"
draft = false
weight = 1
subtitle = "Emacs, LaTeXなどのMacBook Pro 16-inch, Catalinaへのインストール"
[image]
  placement = 3
  caption = "Desktop of MacBook Pro 16 inch (Catalina)"
+++

1ヶ月ほど前にMacBook Pro late 2016のバッテリーが逝かれてしまい，laptopのはずがコンセントに繋がないと動かないdesktopになってしまった．すると待っていたかのようにMacBook Pro 16-inch 2019が発売となり，速攻で注文してしまった．これまでは常に半年ぐらい様子を見てから新機を購入していたのだが，今回は止むを得ず発売直後のものを購入せざるを得なかった．新しい計算機やOSへのEmacsやLaTeXのインストールは色々な問題に遭遇することが多い．幸いなことに今回の年末年始の休みは長いので，こちらにまとめておくことにした．基本的には，[Upgrade to Mojave and upgrade to Emacs 26.2 by homebrew](../mojave)でまとめたことをやれば良くて，大きな問題はなかった．トップの画像で示しているように各種ソフトがスムーズに動いている．

<!--more-->

**Table of contents**

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [Dealing with read-only system volume in Catalina](#dealing-with-read-only-system-volume-in-catalina)
- [Preparation for Emacs installation](#preparation-for-emacs-installation)
- [How to install Emacs 26.3 into MacBook Pro 16 inch (Catalina)](#how-to-install-emacs-26-dot-3-into-macbook-pro-16-inch--catalina)
- [How to install  LaTeX into MacBook Pro 16 inch (Catalina)](#how-to-install-latex-into-macbook-pro-16-inch--catalina)
    - [References](#references)
    - [フォントマップの確認とpdfへのフォントの埋め込みの確認](#フォントマップの確認とpdfへのフォントの埋め込みの確認)
- [How to setup org-mode in MacBook Pro 16 inch (Catalina)](#how-to-setup-org-mode-in-macbook-pro-16-inch--catalina)
- [How to setup full text search of pdf files on Catalina](#how-to-setup-full-text-search-of-pdf-files-on-catalina)
- [Encountered problems: 遭遇した問題点](#encountered-problems-遭遇した問題点)
    - [zxjafontのプリセットの変更](#zxjafontのプリセットの変更)
    - [org-modeからhtml exportの際のthemeを読み込まない（パスを読まない）](#org-modeからhtml-exportの際のthemeを読み込まない-パスを読まない)
    - [Location of pdf files in BibDesk on Catalina](#location-of-pdf-files-in-bibdesk-on-catalina)
- [まとめ](#まとめ)

</div>
<!--endtoc-->


## Dealing with read-only system volume in Catalina {#dealing-with-read-only-system-volume-in-catalina}

-   参照サイト : [Catalinaでファイルシステムがこう変わる](https://news.mynavi.jp/article/osxhack-242/)

Catalinaのファイルシステムではファイルアクセスが厳格化されており，APFS (Apple File System)でフォーマットされたルートボリューム(起動ディスク)はシステム領域とデータ領域に2分割され，そのうちシステム領域は完全にリードオンリーとなる（上記の参照サイトが詳細かつわかりやすく解説しているので参照されたい）．

私はこれまでは，root directoryにData directoryを作成し，そこにほぼ全てのデータを入れ，また，各種ソフトウェアのソースなどは，root directoryにSources directoryを作成し保存してきた．しかし，Catalinaでは上述の変化に伴い，これが不可能となった．実際，管理者権限でdirectoryをrootに作成しようとしても，

```sh
$ cd /
$ sudo mkdir test
Password:
mkdir: test: Read-only file system
```

と言う具合に撥ねられてしまう．

そこで，Data directoryやSources directoryを自分のhome directory，即ち，/Users/taipapa/の下に入れることにした．つまり，

```sh
/
├── Data
└── Sources
```

から

```sh
taipapa
├── Data
└── Sources
```

に変更した．これに伴い，各ファイル内のパスも修正した．基本的にはファイルパスの最初に，/Users/taipapa/もしくは **~** をつければ良いだけであるが，これでうまくいかないものは個別に対応するしかない．例えば，文献管理ソフトの **BibDesk** がそうであった．これらの問題は別稿にまとめることにする．


## Preparation for Emacs installation {#preparation-for-emacs-installation}

つい先日やったばかりなのに，もはや記憶が定かではないのだが，要は，[Upgrade to Mojave and upgrade to Emacs 26.2 by homebrewのUpgrade to Xcode 10.2.1](../mojave//#upgrade-to-xcode-10-dot-2-dot-1)にまとめたことを行った．tmp directoryのpermissionの問題があったかどうかは覚えていない（笑）．homebrew自体のインストールにも特に問題はなかった．要するに，ほとんど何も覚えていないくらいスムーズにことは運んだと言うことである．


## How to install Emacs 26.3 into MacBook Pro 16 inch (Catalina) {#how-to-install-emacs-26-dot-3-into-macbook-pro-16-inch--catalina}

Emacsのインストールも，[Upgrade to Mojave and upgrade to Emacs 26.2 by homebrew の Upgrade to Emacs 26.2 from 26.1](../mojave/#upgrade-to-emacs-26-dot-2-from-26-dot-1) にまとめたことを行っただけである．

```sh
$ brew tap railwaycat/emacsmacport
$ brew install emacs-mac --with-modern-icon --with-imagemagick
$ ln -s /usr/local/opt/emacs-mac/Emacs.app /Applications
```

これで， **/usr/local/Cellar/emacs-mac/emacs-26.3-z-mac-7.8 (4,010 files, 129.7MB)** がインストールされた．

あとは，以前に書いた以下の記事に従ってセットアップした．

[Emacsの設定（その1）Preludeの導入（2018年10月9日修正）](../prelude_install)

[Emacsの設定（その2）設定ファイル（init.el）をorg-modeで管理する](../init_org)

[Emacsの設定（その3）ようやくinit.orgの記述: 日本語の設定，inline-patchの設定など](../japanese_setup)

実際には，preludeを導入した後に，前のMacBook Pro 2016 lateの/Users/taipapa/.emacs.d/personal/init.orgをコピペしただけである．もちろん，多少の微調整は必要であったが，特に問題なく設定も終了した．diredがらみの微調整はCatalinaに特有の問題ではないようだが，後日に別途まとめるつもりである．


## How to install  LaTeX into MacBook Pro 16 inch (Catalina) {#how-to-install-latex-into-macbook-pro-16-inch--catalina}


### References {#references}

-   [MacTeX 2019 のインストール＆日本語環境構築法](https://doratex.hatenablog.jp/entry/20190502/1556775026)
-   [LaTeXをインストールし，texファイルが変更されると，自動的にcompileしてskimでのpdfも自動で更新されるようにする（2018年9月1日追記）](../latexmk)
-   [Upgrade to Mojave and upgrade to Emacs 26.2 by homebrew](../mojave)

最初の[MacTeX 2019 のインストール＆日本語環境構築法](https://doratex.hatenablog.jp/entry/20190502/1556775026)の通りにすれば良い．これにより， **texlive2019** がインストールされる．下の2つのサイトは当ブログの以前の記事であるが，これをもとに補足作業を行なった．なお，[macOS Catalina / macOS Mojave / macOS High Sierra / macOS Sierra / OS X El Capitan に付属するヒラギノフォントのセットアップ](https://texwiki.texjp.org/?%E3%83%92%E3%83%A9%E3%82%AE%E3%83%8E%E3%83%95%E3%82%A9%E3%83%B3%E3%83%88#macos-hiragino-setup) にCatalinaでのtexlive2019のインストールの仕方が記載されているが，私は前述の通りにやった後にこのサイトに気が付いたので，こちらのやり方は行っていない．


### フォントマップの確認とpdfへのフォントの埋め込みの確認 {#フォントマップの確認とpdfへのフォントの埋め込みの確認}

[いまさらMacTeXの更新](https://qiita.com/potato%5Fomom/items/88b5964bb057bbddf2c3) を参考に，フォントマップを見ると，

```sh
$ kanji-config-updmap-sys status
Cannot find ptex-fontmaps-macos-data.dat, skipping!
CURRENT family for ja: hiragino-highsierra-pron
Standby family : ipa
Standby family : ipaex
```

とりあえず， **CURRENT family for ja: hiragino-highsierra-pron** になっているのでよしとする．latexで生成したhogehoge.pdfのフォントの埋め込みを，pdffontを用いて確認してみると，

```sh
$ pdffonts SMC-Ab.pdf
name                                 type              encoding         emb sub uni object ID
------------------------------------ ----------------- ---------------- --- --- --- ---------
RWTTMJ+LMSans10-Bold                 Type 1C           Custom           yes yes yes      4  0
CZOKCD+CMMI12                        Type 1C           Builtin          yes yes yes      5  0
NBDCJR+HiraKakuProN-W6-Identity-H    CID Type 0C       Identity-H       yes yes no       7  0
TYTYWC+LMRoman17-Regular             Type 1C           Custom           yes yes yes      8  0
GWABID+LMRoman12-Bold                Type 1C           Custom           yes yes yes      9  0
AJNMNY+LMRoman12-Regular             Type 1C           Custom           yes yes yes     10  0
HCOYDP+HiraMinProN-W3-Identity-H     CID Type 0C       Identity-H       yes yes no      16  0
RMUEJU+LMRoman8-Regular              Type 1C           Custom           yes yes yes     21  0
XXHLXJ+LMRoman7-Regular              Type 1C           Custom           yes yes yes     23  0
PPVQFL+LMMono10-Regular              Type 1C           Custom           yes yes yes     24  0
```

とヒラギノフォントも含めて全てのフォントはemb = yesとなっており，確かに埋め込まれている．

なお，文献管理ソフトであるBibDeskのpdfの管理に問題が生じたが，これについては解決策とともに別稿でまとめる．


## How to setup org-mode in MacBook Pro 16 inch (Catalina) {#how-to-setup-org-mode-in-macbook-pro-16-inch--catalina}

最もよく使うorg-modeのセットアップは以下の以前の記事の通りに作業した．

-   [Emacsのorg-modeで論文を書く（その1：pdfとhtmlへの出力）](../org-mode_paper_1)
-   [Emacsのorg-modeで論文を書く（その2：BibDeskによる論文収集と整理）](../org-mode_paper_2)
-   [Emacsのorg-modeで論文を書く（その3：org-modeとbibtexとreftexの連携による文献引用の自動化](../org-mode_paper_3)
-   [Emacsのorg-modeで論文を書く（その4：pandocを利用してorg-modeからword [docx]を文献付きでexportする）](../org-mode_paper_4)

といっても実際にやったのは，以前の/Users/taipapa/.emacs.d/personal/init.orgをコピペしたことがほぼ全てである．


## How to setup full text search of pdf files on Catalina {#how-to-setup-full-text-search-of-pdf-files-on-catalina}

以前の記事で全文検索についてまとめたが（[Full text search of PDF archives with hyperestraier on maos (mojave) — Hyper Estraierでpdfの全文検索を行う](../fulltextsearch)），Catalinaでもこの記事と同じ設定でpdf fileの全文検索が可能となった．Mojaveでもhome directoryでセットアップしたので，大きな問題はなかったのであろう．ただし，Catalinaでは，pdf fileの置き場所がhome directoryの下のData directory以下に変更されているため，/Users/taipapa/Sites/cgi-bin/est/estseek.confの設定は以下のように変更した．

```sh
1 #indexname: casket
2 indexname: /Users/taipapa/Sites/pdf/casket
3
4 tmplfile: estseek.tmpl
5
6 topfile: estseek.top
7
8 helpfile: estseek.help
9
10 lockindex: true
11
12 pseudoindex:
13
14 #replace: ^file:///home/mikio/public_html/{{!}}http://localhost/
15 #replace: /index\.html?${{!}}/
16
17 #replace: ^file:///Data/{{!}}http://localhost/~taipapa/pdf/PDFs/
18
19 replace: ^file:///Users/taipapa/Data/{{!}}http://localhost/~taipapa/pdf/PDFs/
20
```

これでブラウザによるpdf fileの全文検索が可能となった．なお，MacBook Pro late 2016では，document数が11734個，語数が1351563のindex作成に要した時間は約40分強であったが，今回，MacBook Pro 2019 16 inch では，document数が11782個，語数が1362430のindex作成に要した時間は約31分強であった．


## Encountered problems: 遭遇した問題点 {#encountered-problems-遭遇した問題点}

ここからは，上述のセットアップの際に遭遇した問題点についてまとめておく．


### zxjafontのプリセットの変更 {#zxjafontのプリセットの変更}

-   参照サイト：[そういえば ZXjafont が新しくなった（v0.4）](https://zrbabbler.hatenablog.com/entry/20180721/1532187775)
-   zxjafontとは，「和文フォントのプリセット設定」を XeLaTeX + zxjatype の環境で行うためのもの

上記サイト（[Emacsのorg-modeで論文を書く（その1：pdfとhtmlへの出力）](../org-mode_paper_1)）にあるorg fileをコピペして，org fileからpdfをexportしようとすると，

```sh
! Package zxjafont Error: The old preset 'hiragino-dx' is *abolished*.

See the zxjafont package documentation for explanation.
Type  H <return>  for immediate help.
```

と言うようなエラーになる．要するに，texlive2019をインストールしてzxjafontが新しくなったのに伴い，"hiragino-dx"と言うpresetは古くてもう廃止されたと言われているのである．指示に従ってドキュメントを読むと， **hiragino-pron** を使えとあるので，init.orgの中のhiragino-dxを全てhiragino-pronに変更するとうまくいくようになった．


### org-modeからhtml exportの際のthemeを読み込まない（パスを読まない） {#org-modeからhtml-exportの際のthemeを読み込まない-パスを読まない}

以前の記事（[Org-modeでhtml exportの際のthemeについて](../org-html-export-theme)）で，好きなテーマとしてあげた **org-spec** であるが，org fileの冒頭の部分は以下のようになっている．

```lisp
#+HTML_HEAD: <link href="http://fonts.googleapis.com/css?family=Roboto+Slab:400,700|Inconsolata:400,700" rel="stylesheet" type="text/css" />
#+HTML_HEAD: <link href="/Hoge/Fuga/org-spec-master/css/style.css" rel="stylesheet" type="text/css" />
```

これを，Catalinaではパスの変更に合わせて以下のように設定した．

```lisp
#+HTML_HEAD: <link href="http://fonts.googleapis.com/css?family=Roboto+Slab:400,700|Inconsolata:400,700" rel="stylesheet" type="text/css" />
#+HTML_HEAD: <link href="/Users/taipapa/Hoge/Fuga/org-spec-master/css/style.css" rel="stylesheet" type="text/css" />
```

しかし，上記の2行目を，Catalinaではうまく読めなくなり，設定が読み込まれなくなった．スッピンのhtml とでも言うか，単なるテキストに近いものとしてexportされてしまう．しかし，org-specのcss/style.cssをorg fileと同じdirectoryに置いて，

```lisp
#+HTML_HEAD: <link href="css/style.css" rel="stylesheet" type="text/css" />
```

と冒頭の部分を書き直すとうまくいく．前述の1行目はなくてもよかった．


### Location of pdf files in BibDesk on Catalina {#location-of-pdf-files-in-bibdesk-on-catalina}

以前の記事でBibDeskについてまとめた（[Emacsのorg-modeで論文を書く（その2：BibDeskによる論文収集と整理）](../org-mode_paper_2)）が，その中で，自分の文献リストに“hogefuga-reference.bib”と名前をつけて保存していることを記載している．ただ，保存場所については記述していなかった．実はこのリストは，/Users/taipapa/Documentsに置いており，iCloudを利用して，仕事場のiMacでも同じ文献リストが使えるようにしている．BibDeskでは，その論文のpdfも一緒に管理できるのだが，上述のファイルシステム構造の変化により，全て，pdfが行方不明になってしまった．

BibDeskにおけるpdfのlocationを修正しようとしてhogefuga-reference.bibの中を見てみると，

```tex
Bdsk-File-1 = {YnBsaXN0MDDSAQIDBFxyZWxhdGl2ZVBhdGhZYWxpYXNEYXRhXxAhLi4vLi4vLi4vRGF0YS9OUy1wZGYvMjVfM18zMTMucGRmTxEBQgAAAAABQgACAAAKSGllcm9ueW11cwAAAAAAAAAAAAAAAAAAAAAAAAAAAEJEAAH/////DDI1XzNfMzEzLnBkZgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP////8AAAAAAAAAAAAAAAAAAwADAAAKIGN1AAAAAAAAAAAAAAAAAAZOUy1wZGYAAgApLzpVc2Vyczprb2hraWNoaTpEYXRhOk5TLXBkZjoyNV8zXzMxMy5wZGYAAA4AGgAMADIANQBfADMAXwAzADEAMwAuAHAAZABmAA8AFgAKAEgAaQBlAHIAbwBuAHkAbQB1AHMAEgAnVXNlcnMva29oa2ljaGkvRGF0YS9OUy1wZGYvMjVfM18zMTMucGRmAAATAAEvAAAVAAIAD///AAAACAANABoAJABIAAAAAAAAAgEAAAAAAAAABQAAAAAAAAAAAAAAAAAAAY4=}}
```

と言うふうに，pdfの場所は暗号化？されており，修正などできないことが判明した．1万数千のpdf fileをもう一度手作業でBibDeskに認識させないといけないのかと気が遠くなりかけた．しかし，よく考えてみると，BibDeskは相対的な場所やパスを認識している．以前のMacBook Pro late 2016では，/Data/hoge-pdf/のように，/Dataの下にdirectoryを作成して，そこにpdfを保存していた．Catalinaでは，Data directoryはhome directoryである/Users/taipapaの下に置かれるようになった．つまり，相対的には2レベル下のdirectoryに文献リストであるhogefuga-References.bibを置けば良いのである．

```sh
$ cd /Users
$ tree -L 5
.
└── taipapa
     └── Documents
          └── taipapa2
               └── Documents
                    └── hogefuga-References.bib
```

上図のように，Documentsの下にtaipapa2 directoryを作成し，さらにその下にDocuments directoryを作成し，そこにhogefuga-References.bibを置くようにしたところ，BibDeskがpdfの場所を認識するようになった．ただ，iCloudを介してMojaveがインストールされた他のマック（まだmojave）からもこの文献リストを共有するためには，/Users/taipapa/Documents/にhogefuga-References.bibをコピーしておかなければならない．symbolic linkではオリジナルの方を認識してしまいうまくいかないし，hard linkではiCloudを経由するとうまくいかない．


## まとめ {#まとめ}

今回はかなり苦労すると予想していたのだが，意外とスムーズに行ってしまった（笑）．問題になったのは細かい点が多く，ファイルシステムの変更に伴う問題が一番厄介であった．そのほかでは，GIMPも特に問題なく動き，動画ファイルも問題なかった．お陰で仕事ができる環境を素早く確立することができた．今回も，「案ずるより産むが易し」であった．😄
