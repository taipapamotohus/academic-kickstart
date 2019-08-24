+++
title = "Gleanings (display-line-numbers, whitespace-mode, accessibility of iTerm2, etc)"
author = ["taipapa"]
date = 2019-06-02
lastmod = 2019-06-08T23:16:25+09:00
tags = ["line-number", "whitespace", "thunderbird", "emacs", "iTerm2", "accessibility"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/Galata.jpg"
  caption = "Galata Kulesi"
+++

前回久しぶりに記事を投稿したが，しばらく更新していない間に，いくつか細かな環境修正が必要であったので，落ち穂拾いとしてまとめておく．<!--more-->

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [display-line-numbers](#display-line-numbers)
- [Whitespace-mode](#whitespace-mode)
- [iTerm2の権限の変更（mojaveで必要）](#iterm2の権限の変更-mojaveで必要)
- [iTerm2のColor Schemeが効かない](#iterm2のcolor-schemeが効かない)

</div>
<!--endtoc-->


## display-line-numbers {#display-line-numbers}

emacs 26から入った行番号を表示させる機能．

-   Ref 1: [emacs 26で入ったdisplay-line-numbersを利用する](http://garaemon.hatenadiary.jp/entry/2018/07/08/181003)
-   Ref 2: [LineNumbers](https://www.emacswiki.org/emacs/LineNumbers)
-   Ref 3: [Emacs26のdisplay-line-numbers-modeがlinum-modeよりちょっと速くて快適になりそう](https://blog.piyo.tech/posts/2019-01-24-emacs-display-line-numbers/)
-   Ref 4: [Emacsの行番号表示にモヤモヤした話](https://qiita.com/mamo3gr/items/1c6862cba09d5876e52c)

設定は，いつものように，init.orgに下記のとおりに書き込めばよい．

```lisp
#+begin_src emacs-lisp
(global-display-line-numbers-mode)
#+end_src
```

これでもう行番号を表示していても重くならない．表示領域を確保するための設定も不要である．実際の表示状況は下の画像のとおりである．nlinum-modeの設定は削除した．

{{< figure src="/img/display-line-number.jpg" width="100%" target="_self" >}}

行番号に必要な表示領域は自動的に確保される．3桁，4桁の番号になるに連れて表示領域が広くなるのが上の画像からわかるであろう．また，adaptive-wrapによって折り畳まれている行は行番号が空白になって1行であることがわかるようになっている（383行目）．


## Whitespace-mode {#whitespace-mode}

かなり前から，Emacsの長い行の途中でフォントの色が赤くなってしまうようになった．つまり，下の画像のようになってしまう．短い行でもフォントが赤くなってるな．．．

{{< figure src="/img/swiper-2a.jpg" width="100%" target="_self" >}}

何が原因かよくわからなかったのだが，redditの[How do I stop Emacs from coloring text beyond 80 columns differently](https://www.reddit.com/r/emacs/comments/butal1/how%5Fdo%5Fi%5Fstop%5Femacs%5Ffrom%5Fcoloring%5Ftext%5Fbeyond%5F80/)を読んで，もしかしたら，whitespace-modeのせいかもしれないと思ってやってみたらドンピシャであった．M-x customizeとして，Whitespace Styleをサーチして，(Face) Lines, only overlong partのチェックを外せばよい．下の画像の矢印で示した部分である．

{{< figure src="/img/whitespace.jpg" width="100%" target="_self" >}}

下に，設定変更後の上の画像と同じところを示すが，行の途中でフォントの色が変わらなくなっている．これで目がおかしくなりそうな画面からようやく離れることができた．😀

{{< figure src="/img/swiper-3.jpg" width="100%" target="_self" >}}


## iTerm2の権限の変更（mojaveで必要） {#iterm2の権限の変更-mojaveで必要}

以前の記事（[Emacsのorg-modeを保存すると自動的にhtmlにexportされブラウザーが更新されるようにする](../auto_html_export)）の中で，directoryの内容が変更されると，自動でhtmlを再読込するwatch\_safari.rbというスクリプトを紹介した．しかし，mojaveにupgradeしたあとは

```sh
85:141: execution error: System Eventsでエラーが起きました: osascriptにはキー操作の送信は許可されません。 (1002)
```

というエラーを吐いて，自動更新しなくなってしまった．調べてみると，mojaveでユーザーのデータや個人情報を保護するための機能が強化された結果であった．

-   Ref 1: [macOS Mojaveでのアプリケーションの権限、アクセシビリティとフルディスクアクセスの違いは何か？](https://minatokobe.com/wp/os-x/macos-mojave/post-51360.html)
-   Ref 2: [macOS MojaveでiTerm2を利用する場合にフルディスクアクセスを許可する方法](https://www.softantenna.com/wp/tips/macos-mojave-iterm2-full-disk-access/)
-   Ref 3: [MacのターミナルやiTermでOperation not permittedが出る場合の対処法](https://rcmdnk.com/blog/2019/04/29/computer-mac/)
-   Ref 4: [Why Full Disk Access is Useful for iTerm2](https://gitlab.com/gnachman/iterm2/wikis/Whyfulldiskaccess)

AccessibilityとFull Disk Accessのいずれか，あるいは，両方を変更すればよい．試してみた結果，iTerm2にFull Disk Accessを許可しても自動更新しないが，Accessiblityで「アプリケーションにコンピュータの制御を許可」すれば自動更新することがわかった．「システム環境設定」>「セキュリティ＆プライバシー」の「プライバシー」タブに行き，下の画像にあるように，iTerm2にcheckを入れればよい．

{{< figure src="/img/Privacy.jpg" width="80%" target="_self" >}}

crontabが働き，~/Library/Safari folderへのアクセスができるように，iTerm2にFull Disk Accessも与えておいた．方法は，上記とほぼ同じで，「プライバシー」タブに行き，フルディスクアクセスを選択し，iTerm2にcheckを入れればよい．


## iTerm2のColor Schemeが効かない {#iterm2のcolor-schemeが効かない}

いつのまにかiTerm2のcolor schemeを変えても反映されなくなってしまった．ネットを探ると，同じようなことで悩んでいる人が結構いるようで，すぐに解決法が見つかった．

-   Ref: [Profile colors won't change](https://gitlab.com/gnachman/iterm2/issues/4844)

上記のサイトによれば，terminal windowがprofileと連絡を失ってしまったことが原因とのことで，iTerm2の Session > Edit Sessionで下の画像のようにdialogue boxがpop upするので，そこで，好きなcolor scheme (下の画像ではJapanesque) を選択し，右下のUse Selected Profileをクリックすればよい．

{{< figure src="/img/iTerm2.jpg" width="80%" target="_self" >}}

これでリンクが再確立される．
