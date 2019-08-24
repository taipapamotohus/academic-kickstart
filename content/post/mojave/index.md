+++
title = "Upgrade to Mojave and upgrade to Emacs 26.2 by homebrew"
author = ["taipapa"]
date = 2019-04-29
lastmod = 2019-04-30T23:38:39+09:00
tags = ["Mojave", "upgrade", "emacs", "26-2", "homebrew"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/Vancouver.jpg"
  caption = "Vancouver"
+++

世はゴールデンウィークまっただ中である．完全な10連休ではないが，それなりに長い休みとなるので，この機会に，ようやく Sierraから **Mojave** にupgradeすることにした．ついでにEmacsも26.1から 26.2にupgradeした．今回は，このupgradeの際に遭遇したトラブルについてまとめる．

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [Upgrade to Mojave from Sierra](#upgrade-to-mojave-from-sierra)
- [Upgrade to Xcode 10.2.1](#upgrade-to-xcode-10-dot-2-dot-1)
- [Upgrade to Emacs 26.2 from 26.1](#upgrade-to-emacs-26-dot-2-from-26-dot-1)
- [LaTeX](#latex)
    - [pdfにフォントが埋め込まれているかどうかを確認する方法](#pdfにフォントが埋め込まれているかどうかを確認する方法)
- [感想](#感想)

</div>
<!--endtoc-->


## Upgrade to Mojave from Sierra {#upgrade-to-mojave-from-sierra}

**Ref:** [macOS Mojave にアップグレードする方法](https://support.apple.com/ja-jp/HT201475)

結論から言うと，拍子抜けするぐらい簡単であった．AppStoreで適当にクリックするとすぐにMojaveがダウンロードされて，インストーラーが起動した．これをクリックしてインストールを始めると，此処から先は完全自動状態で，ひたすら待った．というか，違うことをしていた．何回も再起動していたようだが，実際には1時間ぐらいで終了したような気がする．手間いらずであった．R, Rstudio, ImageJ, Emacs, LaTeXが動いて画像編集，動画編集ができれば，とりあえず文句はないので，まずそのあたりをチェックしてみると，R, Rstudio, ImageJ, 画像編集，動画編集は問題なく動いた．EmacsとLaTeXについては以下に述べる．


## Upgrade to Xcode 10.2.1 {#upgrade-to-xcode-10-dot-2-dot-1}

早速brewでemacsをupgradeしようとしたのだが，xcodeが古いと叱られたので，まず，xcodeをApp Storeからupgradeした．そして **brew install** すると以下のようなエラーが出る．

```sh
$ brew install hogehoge
.........
xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
```

これは，「[macOS を Mojave にあげた後に Homebrew を使うとエラーが出る問題](https://gotohayato.com/content/487)」にあるようにcommand line developer toolsを再インストールすれば直る．同サイトに詳細が記載されている．

```sh
xcode-select --install
```

さぁ，これでようやくと思って，再度 **brew install** すると，またもエラーである.....(ToT)

```sh
$ brew install hogehoge
..........
Error: parent directory is world writable but not sticky
Please report this bug:
https://docs.brew.sh/Troubleshooting
```

調べてみるとpermissionの問題で，tmp directoryの状態を調べれば良いことがわかった．

**Ref 1:** [brew で \`Error: parent directory is world writable but not sticky\`](https://qiita.com/analsky/items/20755a3ba10119e9a4b6) <br />
**Ref 2:** [Error: parent directory is world writable but not sticky](https://stackoverflow.com/questions/42893700/error-parent-directory-is-world-writable-but-not-sticky) <br />
上記サイトの記載に従って，ls -ld /tmpを行うと以下のようになる．

```sh
$ ls -ld /tmp
lrwxr-xr-x@ 1 root  wheel  11  4 27 18:45 /tmp@ -> private/tmp
```

これは，tmp directoryはprivate/tmpを使用しているということであり，以下のようにしてpermissionを付与する．

```sh
$ sudo chmod +t /private/tmp
```

私の場合はこれでbrewが働くようになった．これらの操作は，今後のmajor upgradeの際にはまた必要になりそうなので，ここにまとめておく．


## Upgrade to Emacs 26.2 from 26.1 {#upgrade-to-emacs-26-dot-2-from-26-dot-1}

ようやくEmacsのupgradeである．mojaveにupgradeした直後にemacs26.1を起動してみるとキーが効かなかったような気がするが，26.2にupgradeするので，気にせず先に進んだ.....(^^;;;　以前の記事（[Emacsのインストール](../emacs_install)）に書いたようにhomebrewでemacs-macを入れればよいのだが，念の為に単なるupgradeは避けて，Emacs 26.1をuninstallし，~/.emac.dも退避させてから，インストールし直すことにした．[Emacs Mac Port](https://github.com/railwaycat/homebrew-emacsmacport)の最終更新はわずか2週間前でありいろいろな問題が解決されていると期待してのupgradeである．

```sh
$ cd
$ mv .emacs.d .emac.d.old  # change name of old .emacs.d
$ brew tap railwaycat/emacsmacport
$ brew uninstall emacs-mac  # uninstall old emacs-mac
$ brew install emacs-mac --with-modern-icon --with-imagemagick  # install new emacs-mac
$ ln -s /usr/local/opt/emacs-mac/Emacs.app /Applications
```

これで，<br />
**/usr/local/Cellar/emacs-mac/emacs-26.1-z-mac-7.1 (4,009 files, 114.6MB)** <br />
から <br />
**/usr/local/Cellar/emacs-mac/emacs-26.2-z-mac-7.6 (4,010 files, 114.8MB)**  <br />
へのupgradeが終了した．

あとは，以前に書いた以下の記事に従ってセットアップした．<br />
[Emacsの設定（その1）Preludeの導入（2018年10月9日修正）](../prelude_install) <br />
[Emacsの設定（その2）設定ファイル（init.el）をorg-modeで管理する](../init_org)  <br />
[Emacsの設定（その3）ようやくinit.orgの記述: 日本語の設定，inline-patchの設定など](../japanese_setup)


## LaTeX {#latex}

これについては，以前の記事「[LaTeXをインストールし，texファイルが変更されると，自動的にcompileしてskimでのpdfも自動で更新されるようにする（2018年9月1日追記）](../latexmk)」で書いたとおり **MacTeX 2018** のままである．正確に言うと，mojaveにupgradeする直前に以下のようにして最新版にアップデートしておいた．

```sh
$ sudo tlmgr update --self --all
```

400個ぐらいのパッケージのアップデートに30-40分を要した．

**ヒラギノフォントの埋め込み** についても上述の記事に書いたように，既に，[bibunsho7-patch](https://github.com/munepi/bibunsho7-patch/releases)を適応済みなので，問題ないと考えた．

mojaveにupgradeしてから，latexmkによるcompileやorg-modeからのlatex exportなどを試してみたが，pdfの生成に特に問題なく，また，pdfへのヒラギノフォントの埋め込みも問題なくできていた．


### pdfにフォントが埋め込まれているかどうかを確認する方法 {#pdfにフォントが埋め込まれているかどうかを確認する方法}

-   **Ref:**[ PDFのフォント埋め込み](https://qiita.com/Aqua%5Fix/items/d277fb7e4667d6616c1e)
-   以下のようにhomebrewでpopplerをインストールすれば，その中の **pdffonts** というコマンドを使って確認することができる．このpopplerは以前の記事（[Emacsでpdfを読む (pdf-tools)](../pdf-tools)）で既にインストールしているが，もう一度書いておく．

    ```sh
    $ brew install poppler
    ```
-   たとえば，latexで生成したhogehoge.pdfのフォントの埋め込みを調べるためには，pdffontsを以下のように使う． **emb** の項目で埋め込みの有無がわかる．

    ```sh
    $ pdffonts hogehoge.pdf
    name                                 type              encoding         emb sub
    ------------------------------------ ----------------- ---------------- --- ---
    KQKHHV+LMSans10-Bold                 Type 1C           Custom           yes yes
    JQHYHW+LMRoman17-Regular             Type 1C           Custom           yes yes
    FENZQQ+HiraMinProN-W3-Identity-H     CID Type 0C       Identity-H       yes yes
    ZHPQAJ+LMRoman12-Regular             Type 1C           Custom           yes yes
    BMLTDB+HiraKakuProN-W6-Identity-H    CID Type 0C       Identity-H       yes yes
    NOWECW+LMRoman12-Regular             Type 1C           Custom           yes yes
    SIHLPZ+LMRoman8-Regular              Type 1C           Custom           yes yes
    ```
-   確かに，すべてのフォントは **emb = yes** になっており，埋め込まれているのが確認できた．


## 感想 {#感想}

ネットでは，いろいろ問題ありとの情報が多く様子見をしていたのだが，待ってる間に色々と解決した部分も多いのか，ほとんど大きなトラブルもなくアップグレードできた．なお， **Time Machine** によるバックアップも問題なくできている．「案ずるより産むが易し」であった．
