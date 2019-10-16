+++
title = "Org-rifle"
author = ["taipapa"]
date = 2019-10-14
lastmod = 2019-10-16T22:40:38+09:00
tags = ["emacs", "org-mode", "org-rifle", "search"]
type = "post"
draft = false
weight = 1
[image]
  placement = 3
  caption = "Samuel Beckett Bridge, Dublin"
+++

学会続きで，久方ぶりの更新である．org fileの中を検索する際に，[Swiper, ivy, avy, migemoによるEmacsの検索強化](../swiper_migemo)で取り上げた **swiper** を，もっぱら使用しているのだが，そうすると，ファイル内のどこにいるのかが分からなくなることがある．検索ファイルのパスなどがorg fileの中で分かる検索ソフトはないものかと探したところ，このorg-rifleが見つかった．

<!--more-->

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [References](#references)
- [Install](#install)
- [How to use](#how-to-use)

</div>
<!--endtoc-->


## References {#references}

-   [org-rifle](https://github.com/alphapapa/org-rifle)  ご本家
-   [helm-org-rifle](https://dustinlacewell.github.io/emacs.d/#org7646621)
-   [Org Rifle](http://bnbeckwith.com/bnb-emacs/#orgc5aa916)  Rifle through my org-mode entries.

ご本家サイトの最初に，有名な米海兵隊信条（The Creed of a United States Marine）が，以下のように引用されている．

This is my rifle. There are many like it, but this one is mine. My rifle is my best friend. It is my life. I must master it as I must master my life.

rifleには，「くまなく探す」という意味もあるので，この命名は，それにかけたシャレのようである．非武装平和主義が信条の私にはよく分からん．．．とにかく，詳細に書かれたご本家サイトを読めば，こんなところを読む必要はないような気もするが（笑），後日の自分のためにまとめておく．


## Install {#install}

いつものように，init.orgに下記のように書き込む．

```lisp
#+begin_src emacs-lisp
(use-package helm-org-rifle
  :ensure t
  :after (helm org)
  :bind
  ("s-c r r" . helm-org-rifle)
  ("s-c r a" . helm-org-rifle-agenda-files)
  ("s-c r ." . helm-org-rifle-current-buffer)
  ("s-c r d" . helm-org-rifle-directories)
  ("s-c r f" . helm-org-rifle-files)
  ("s-c r D" . helm-org-rifle-org-directory)
  ("s-c o o" . helm-org-rifle-occur)
  ("s-c o a" . helm-org-rifle-occur-agenda-files)
  ("s-c o ." . helm-org-rifle-occur-current-buffer)
  ("s-c o d" . helm-org-rifle-occur-directories)
  ("s-c o d" . helm-org-rifle-occur-directories)
  ("s-c o f" . helm-org-rifle-occur-files)
  ("s-c o D" . helm-org-rifle-occur-org-directory))
#+end_src
```

key-bindingは諸般の事情により，s-cを使うことにした（s はoption keyを意味する）．

また， **helm-org-rifle-show-path を t にセット** することにより，org file内でのそれぞれのheadingに至るパスが表示される．（実は，デフォルトで t になっている）


## How to use {#how-to-use}

helm-org-rifleは，エントリーベースで結果が表示される．つまり，org fileの中のheadingとその内容が表示されるので，そこに至るパスが分かる，つまり，どこに位置しているかが分かる．これは思っていた以上に便利である．

例えば，このブログの原稿をemacsで開いているときに，org-mode, export, wordを含む部分を探したいときは，s-c r . としてから，key wordを打てば，まず下図のようになる．下のバッファでハイライトされている部分は，最上位のheadingであるPostの下のsubheadingである「Emacsのorg-modeで論文を書く（その4：．．．」の下位に目的の部分が含まれていることを示す．

{{< figure src="/img/org-rifle-1.jpg" width="80%" target="_self" >}}

さらに，ハイライトされている部分で，C-jと打てば，下図のように，上のバッファが該当する領域にジャンプして表示してくれる．

{{< figure src="/img/org-rifle-2.jpg" width="80%" target="_self" >}}

もちろん，検索対象が明確に分かっていて"hogehoge.jpg"など名前も分かっているのであれば，swiperの方が便利であろう．しかし，「hogeとfugaとhogaに関連している部分はどこだったかな？」というようなときは，このorg-rifleが重宝する．

さらにもっと色々な使い方ができるようで，該当項目のrefileも可能である．興味のある方は，tabを打ってでてくるメニューを試していただきたい．
