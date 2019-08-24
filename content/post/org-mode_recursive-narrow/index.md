+++
title = "Org-modeで再帰的にsubtreeを絞ったり広げたりする（recursive-narrow）"
author = ["taipapa"]
date = 2018-12-24
lastmod = 2018-12-24T21:38:53+09:00
tags = ["orgmode", "emacs", "recursive", "narrow"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/PiazzaNavona.jpg"
  caption = "Piazza Navona"
+++

org-modeで文章を書いているときに，他のsubtreeが邪魔で消したくなることがある．そして必要になれば，また，もとに戻すことができれば便利である．これを実現してくれるパッケージが[recursive-narrow](https://github.com/nflath/recursive-narrow)である．


## インストールと設定 {#インストールと設定}

インストールは例によって，init.orgに以下のように書き込むだけである．

```lisp
#+begin_src emacs-lisp
(use-package recursive-narrow
  :ensure t)
#+end_src
```

これでインストールと設定は終了である．


## 使用法 {#使用法}

使い方も実に簡単であり，"C-x n n"で現在カーソルがあるsubtree以下のみが表示されるようになり，"C-x n w"で元の表示に戻る．これではよくわからないので，実際の画像を示す．まず，最初の画像では全体の画面が表示されており，1951行目の「Org-modeで再帰的に」の行にカーソルがある．

{{< figure src="/img/narrow-1.jpg" width="100%" target="_self" >}}

ここで，"C-x n n"とやると，次の画面のようになる．つまり，1951行目以降のsubtreeのみが表示される．

{{< figure src="/img/narrow-2.jpg" width="100%" target="_self" >}}

次にカーソルを1962行目の「インストールと設定」に移動する（画像ではすでに移動済み）．そして，再度"C-x n n"とやると，以下の画像のようになる．

{{< figure src="/img/narrow-3.jpg" width="100%" target="_self" >}}

つまり，「インストールと設定」のsubtreeのみの表示となるわけである．集中したい領域だけが表示されて，効率よく入力できる．

広い領域の表示が必要となれば，"C-x n w"とやると，1つ前の画像の状態に戻る．そして，もう一度"C-x n w"とやると，最初の状態に戻る．なんということはないのであるが，便利である．
