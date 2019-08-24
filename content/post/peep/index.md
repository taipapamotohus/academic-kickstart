+++
title = "peep-diredで画像をチラ見して，orgファイルに簡単にリンクを貼り付ける（おまけ：最近開けたdirectoryを一覧表示する方法）"
author = ["taipapa"]
date = 2019-04-19
lastmod = 2019-04-20T13:33:45+09:00
tags = ["emacs", "dired", "peep-dired", "org-mode", "image", "link"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/Ikaruga.jpg"
  caption = "Ikaruga-cho"
+++

最近Rネタばかり書いていたが，今回は久しぶりのEmacsネタである．org-modeで文書を書いているときに画像を貼り付けたいことがある．そのためには画像ファイルの名前とパスが必要となる．要するに毎回画像ファイルのパスを調べて/hoge/fuga/hogefuga/hugo.jpgというようなことをタイプしなければならず面倒である．そこで，peep-diredの出番である．peep-diredとは，diredでファイルにカーソルを持っていくと中身が見える，すなわち，画像ファイルなら画像が見え，テキストファイルならテキストが読めるというminor modeである．これが画像リンクの貼り付けに便利なのでまとめておく．

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [References](#references)
- [peep-diredのインストールと設定](#peep-diredのインストールと設定)
- [peep-diredの使い方](#peep-diredの使い方)
- [bjm/ivy-dired-recent-dirs -  最近開けたdirectoryを一覧表示する方法（おまけ）](#bjm-ivy-dired-recent-dirs-最近開けたdirectoryを一覧表示する方法-おまけ)

</div>
<!--endtoc-->


## References {#references}

-   [peep-dired](https://github.com/asok/peep-dired)
-   [QUICKLY PREVIEW IMAGES AND OTHER FILES WITH PEEP-DIRED](http://pragmaticemacs.com/emacs/quickly-preview-images-and-other-files-with-peep-dired/)


## peep-diredのインストールと設定 {#peep-diredのインストールと設定}

例によって，use-packagを用いて以下のように，init.orgに書けばよい．

```lisp
#+begin_src emacs-lisp
(use-package peep-dired
  :ensure t
  :defer t ; don't access `dired-mode-map' until `peep-dired' is loaded
  :bind (:map dired-mode-map
              ("P" . peep-dired)))
#+end_src
```

設定は上記参照サイトのパクリで，diredで"P"を打つとpeep-dired modeとなってdiredのリストの画像ファイルが見られるようになり，もう一度"P"と打つとpeep-dired modeは終了する．　


## peep-diredの使い方 {#peep-diredの使い方}

File viewerとしてだけなら，どうということはないのだが，org-modeと組み合わせて使うと便利さが増す．たとえば，下図のように画像をおいているdirectoryをdiredで開いて"P"を打ってpeep-dired modeに入り，画像を確かめながら文書に挿入する画像を決める．上段のdiredのバッファにおいてIMG\_1996.jpgにカーソルがあり，下段のバッファにその画像が表示されている．画像が決まったらその画像ファイルにカーソルが合っていることを確認した上で，C-c lを打つ．これで，画像へのリンクがフルパスも含めてコピーされる．

{{< figure src="/img/peep-dired_ex1.jpeg" width="100%" target="_self" link="/img/peep-dired_ex1.jpeg" >}}

ついで，org-mode文書内の画像を挿入したいところにカーソルを持って行き，そこで，C-c C-lとすると，下部に新たなorg-insert-linkのウィンドウが開いて下図のように先程コピーしたリンクが一番上にフルパスでハイライトされて表示される．ここでリターンすればフルパスのリンクがorg-mode文書内に挿入される．

{{< figure src="/img/peep-dired_ex3.jpg" width="100%" target="_self" link="/img/peep-dired_ex3.jpg" >}}

文章で説明すると複雑だが，実際にやってみると実に簡単で有用である．このやり方に気がつくまでは，いちいちフルパスを手入力したり，コピペしたりしていたが，その必要がなくなり非常に楽になった．


## bjm/ivy-dired-recent-dirs -  最近開けたdirectoryを一覧表示する方法（おまけ） {#bjm-ivy-dired-recent-dirs-最近開けたdirectoryを一覧表示する方法-おまけ}

diredでdirectoryを開けるときにその名前を入力する必要があるが，これが結構面倒である．特に深いところにあるファイルはフルパスを書くのが大変である．どうせ，同じファイルに何回も行くことが多いので，「最近訪れたdirectoryの履歴」みたいなのが一覧表示されると嬉しい．まさに，こんなのぞみにピッタリのものが，bjm/ivy-dired-recent-dirsである．これについては，以前に書いたのでそちらを参考にしていただきたい．というか，以前に書いた事自体を忘れていたので，自分への戒めとして記録しておく．．．(^^;;;

**Ref:** [最近開いたディレクトリを開く](https://taipapamotohus.com/post/swiper%5Fmigemo/#最近開いたディレクトリを開く)
