+++
title = "mojaveのライブ変換で快適日本語入力（カーソルの色も日英で変わるように設定）"
author = ["taipapa"]
date = 2019-07-11
lastmod = 2019-07-13T14:50:27+09:00
tags = ["macos", "mojave", "japanese", "input", "emacs", "cursor", "color"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/SanFrancisco.jpg"
  caption = "San Francisco"
+++

最初にmacosにライブ変換が登場した時に使用してみて，これは駄目だとすぐにGoogle inputmethodに戻してしまった．今年の5月にmojaveにupgradeしたのち，ある日，ふと思い立って，ライブ変換を試してみたところ，ほとんどストレスを感じることなくスラスラと入力ができた．ほとんど変換のためにスペースキーを叩く必要がないことに驚いた．予測の精度が登場時よりはるかに改良されているのであろう．エンドユーザーにはありがたいことである．早速乗り換えてしまった．

<!--more-->

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [How to set up](#how-to-set-up)
- [Shortcut](#shortcut)

</div>
<!--endtoc-->


## How to set up {#how-to-set-up}

-   まず画面トップの右端の方の日本語入力のライブ変換にチェックを入れてオンにする．
-   ついで，Emacsのカーソルの色の設定をする．これは以前の記事（[Emacsの設定（その3）ようやくinit.orgの記述: 日本語の設定，inline-patchの設定など](../japanese_setup)）の設定をほんの少し変えるだけのことである．comment outしていた部分を外して，google inputmethodの方をcomment outする．具体的には，init.orgに以下のように書き込めば良い．

```lisp
#+BEGIN_SRC emacs-lisp
(when (fboundp 'mac-input-source)
  (defun my-mac-selected-keyboard-input-source-chage-function ()
    (let ((mac-input-source (mac-input-source)))
      (set-cursor-color
       (if (string-match "com.apple.inputmethod.Kotoeri.Roman" mac-input-source)
           ;; (if (string-match "com.google.inputmethod.Japanese.Roman" mac-input-source)
           "#91C3FF" "#FF9300"))))
  (add-hook 'mac-selected-keyboard-input-source-change-hook
            'my-mac-selected-keyboard-input-source-chage-function))
#+END_SRC
```

これでEmacsでライブ変換を使用している際に，日本語入力の時は赤色のカーソル，英語入力の時は青色のカーソルになる．


## Shortcut {#shortcut}

-   参考：[#Mac のライブ変換で入力をひらがなのままで確定させる ( Control + J )](https://qiita.com/YumaInaura/items/8c74cdf32ad2f5ed57fa)
-   「Control」＋「J」  →   ひらがなに変換
-   「Control」＋「K」  →   カタカナに変換
-   「Control」＋「L」  →   全角英字に変換
-   「Control」＋「;（セミコロン）」  →   半角英字に変換

私のところでは何故か参考サイトと異なり，セミコロンで半角カタカナではなく半角英字に変換される．半角カタカナなんか使わないからいいけど．．．

J, K, L, ; はキーボード上の位置が一直線であり，かつ，左から順番になっているので，指に優しい．

mojaveのライブ変換，とにかく一度使ってみることをお勧めする．
