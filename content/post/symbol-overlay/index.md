+++
title = "Symbol Overlay (Highlight symbols at cursor point with keymap-enabled overlays in Emacs)"
author = ["taipapa"]
date = 2019-07-10
lastmod = 2019-07-10T20:24:34+09:00
tags = ["emacs", "highlight", "symbol", "replace"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/HagiaSophia.jpg"
  caption = "Hagia Sophia"
+++

今回は（も？），redditで拾ってきたネタ（[How to highlight occurences at cursor point in Emacs](https://www.reddit.com/r/emacs/comments/c95cm5/how%5Fto%5Fhighlight%5Foccurences%5Fat%5Fcursor%5Fpoint%5Fin/)）．カーソルの位置にあるシンボル（単語と思えば良い）をバッファ内ですべてハイライトしてくれるEmacsのパッケージを訊いているのだが，いくつか答えがあって，一番便利そうだったのが，今回紹介する **symbol-overlay** である． <!--more-->

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [References](#references)
- [Install](#install)
- [How to use](#how-to-use)

</div>
<!--endtoc-->


## References {#references}

-   [symbol-overlay](https://github.com/wolray/symbol-overlay) （ご本家）
-   [Jump around](https://manuel-uberti.github.io/emacs/2019/02/14/avy/)
-   [Emacsの補完と検索を超強化する](https://qiita.com/blue0513/items/c0dc35a880170997c3f5)


## Install {#install}

例によって，以下のようにinit.orgに書き込んでMELPAからsymbol-overlayをインストールする．

```lisp
#+begin_src emacs-lisp
(use-package symbol-overlay             ; Highlight symbols
  :ensure t
  :config
  (global-set-key (kbd "M-i") 'symbol-overlay-put)
  (global-set-key (kbd "<f7>") 'symbol-overlay-mode)
  (global-set-key (kbd "<f8>") 'symbol-overlay-remove-all)
  )
#+end_src
```


## How to use {#how-to-use}

使用法はご本家に書いてあるが，まず，”M-i”を叩けば，カーソルが位置しているシンボル（単語と思えば良い）が色付きでハイライトされる．同時に，同一バッファ内での全ての同じ単語も同様にハイライトされる．カーソルを動かしてもハイライトされた状態はそのままである．続けて，別の単語にカーソルを持って行って，"M-i"とすれば，その単語が別の色でハイライトされる．勿論，バッファ内の同一の単語も全て同じ色でハイライトされる．しかも，最初にハイライトした単語は，カーソルが離れた後もハイライトされた状態を維持している．文章で書くとわかりにくいが，要するに下図のような状態になる．

{{< figure src="/img/symbol-overlay.jpg" width="80%" target="_self" >}}

さらに，各単語間は "n" で先に進み， "p" で逆戻りできる．別の色のハイライトの単語にカーソルを移動させれば，今度はその単語間で同様に移動できる．そのほかご本家サイトによれば，以下のようなキーバインドになっている．

```lisp
"n" -> symbol-overlay-jump-next
"p" -> symbol-overlay-jump-prev
"w" -> symbol-overlay-save-symbol
"t" -> symbol-overlay-toggle-in-scope
"e" -> symbol-overlay-echo-mark
"d" -> symbol-overlay-jump-to-definition
"s" -> symbol-overlay-isearch-literally
"q" -> symbol-overlay-query-replace
"r" -> symbol-overlay-rename
```

なかでも特筆すべきは "r" のsymbol-overlay-renameで，ハイライトされた単語を一気に書き換えることができる．例えば下図のように "global-set-key" が赤くハイライトされている時，どれかの"global-set-key"にカーソルを置いて "r" を叩けば，minibufferにRenameが表示され，これを消去して新しい名前を書くと赤くハイライトされている単語は一気に新しい名前に変わる．この機能は場合によっては非常に便利である．

{{< figure src="/img/Rename.jpg" width="80%" target="_self" >}}

こういうパッケージを教えてくれるので，redditは有難い．
