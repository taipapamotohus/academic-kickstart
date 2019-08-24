+++
title = "Emacsのインストール"
author = ["taipapa"]
date = 2018-08-14
lastmod = 2018-08-27T22:12:54+09:00
tags = ["emacs", "homebrew"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/Paris.jpg"
  caption = "Paris"
+++

なにはともあれ，まずはEmacsのインストールから．様々な方法があるが，Mac userなので，ここでは[Homebrew](https://brew.sh/index%5Fja)を使ってサクッとインストール．Homebrew自体のインストールはそちらのサイトを参照．

Emacsで日本語を書いてると，M-xしたときに面倒なことになるので，[Imput Method Editor (IME)](https://ja.wikipedia.org/wiki/インプット%5Fメソッド%5Fエディタ)用のパッチを当てる．既にパッチの当たっているYAMAMOTO Mitsuharu版のMac Port用のemacs-macがよい．railwaycatさんがHomebrewでインストールできるようにしてくれているので，これを使わせていただく（[Emacs Mac Port](https://github.com/railwaycat/homebrew-emacsmacport)）．ありがたい．

```shell
$ brew tap railwaycat/emacsmacport
$ brew install emacs-mac --with-modern-icon --with-imagemagick
$ ln -s /usr/local/opt/emacs-mac/Emacs.app /Applications
```

-   --with-modern-iconを指定すると、新しいアイコンになる。

<!--listend-->

-   なお，他のoptionは以下のように打てば分かる．

```sh
$ brew tap railwaycat/emacsmacport
$ brew info emacs-mac
```
