+++
title = "org-sidebar"
author = ["taipapa"]
date = 2019-10-17
lastmod = 2019-10-19T22:16:46+09:00
type = "post"
draft = false
weight = 1
subtitle = "org-modeのバッファにサイドバーを表示する"
[image]
  placement = 3
  caption = "Christ Church Cathedral, Dublin"
+++

ついにorg-mode用のサイドバーが登場した．最近，精力的にredditに投稿しているalphapapa氏の作った文字通りorg-sidebarがそれである．最初1年ぐらい前に発表され，暫くは様子見をしていたのだが，今回の最新版では色々と改良されており，紹介することにした．なお，前回紹介した[org-rifle](../org-rifle)も同氏の作成したパッケージである．

<!--more-->

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [References](#references)
- [Installation](#installation)
- [Usage](#usage)
    - [Combination of org-sidebar and treemacs](#combination-of-org-sidebar-and-treemacs)

</div>
<!--endtoc-->


## References {#references}

-   [org-sidebar](https://github.com/alphapapa/org-sidebar)  ご本家
-   [Finally solving the lack of a tree-view navigation window in Org](https://www.reddit.com/r/emacs/comments/dbsxn7/finally%5Fsolving%5Fthe%5Flack%5Fof%5Fa%5Ftreeview%5Fnavigation/)  redditへの投稿記事
-   [ts.el](https://github.com/alphapapa/ts.el)  a date and time library for Emacs


## Installation {#installation}

いつものように，init.orgに下記のように書き込む．

```lisp
#+begin_src emacs-lisp
(use-package ts
  :ensure t)

(use-package org-sidebar
  :ensure t
  :quelpa (org-sidebar :fetcher github :repo "alphapapa/org-sidebar")
  :bind
  ("<f1> C-s" . org-sidebar-tree))
#+end_src
```

ただし，まず，ご本家サイトにある通りに後半の部分のみでorg-sidebarをインストールしようとすると，tsがないと文句を言われた．そこで，後半部分はコメントアウトした上で，前半部分を追加し，まず，tsをインストールするようにした．ついで，後半部分のコメントアウトを外して，再度org-sidebarをインストールしたところ，うまくいった．キーバインディングの残りがほとんどなく，とりあえず，sidebarのsに合わせられて，かつ，空いているところということで，org-sidebar-treeをf1 C-sに割り当てた．


## Usage {#usage}

ご本家サイトに詳細な説明とGIF動画が掲載されている．GTD toolとしてorg-modeを全く使っていない私には， **M-x org-sidebar-tree** だけで十分役に立つことが分かった．同コマンドを打つ，あるいは， **f1 C-s** を打つと下図のような画面となる．

{{< figure src="/img/org-sidebar-1.jpg" width="100%" target="_self" >}}

左側がtreeのwindowであり，このブログの原稿であるorg-mode fileのlevel2までのheadingが表示されている．5094行目のorg-sidebarにカーソルがあり，このheadingが選択されていることを示す．そして，右側には，このheading (org-sidebar)の内容を示すバッファが表示されている．左側のTree windowの中をカーソルで上下して目的のheadingに行きReturnを叩けば，右側にそのheadingの内容のみが表示され，かつ，カーソルがそちらに移る．他のheadingは表示されず，そこにだけ集中できる．なかなかよくできている．

ここで注意しないといけないのは，swiperやhelm-org-rifle-current-bufferによる検索を行った際に，どのバッファがactiveかで検索範囲が異なることである．右側のバッファでは，そのheadingの内容しか検索しない．Tree windowで行うと，そのファイルの全体を対象として検索する．ただし，結果に飛ぶところでフレームのlayoutが崩れてしまうことがある．そうなると，みやすくする調整に手間がかかる．一方，helm-org-rifleやprojectileのC-c p s gなどは，普通にどのバッファにいても，そのファイル全体を検索するので，org-sidebarを使用している時には，検索にはこちらを使うべきかもしれない．


### Combination of org-sidebar and treemacs {#combination-of-org-sidebar-and-treemacs}

やはり，treeとつくからには，以前に紹介したtreemacs（[Treemacs and Projectile](../treemacs_projectile)）との併用を試してみたくなり，やってみたのが，下図である．

{{< figure src="/img/org-sidebar-2.jpg" width="100%" target="_self" >}}

このように併用はもちろん可能ではあるが，フレームの配列が乱れてしまい，それを調整するのに手間取ることがある．目的が違うものなので，一緒にしなくても異なるフレームで使えば良いと思う．
