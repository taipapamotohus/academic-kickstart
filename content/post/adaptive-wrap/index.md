+++
title = "Emacsの長い行を折り返して見やすくするが実際の行は変えない．adaptive-wrap —Correct indentation for wrapped lines"
author = ["taipapa"]
date = 2019-01-27
lastmod = 2019-02-01T01:06:33+09:00
tags = ["emacs", "org-mode", "adaptive", "wrap", "indentation"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/Femme.jpg"
  caption = "Femme piquée par un serpent"
+++

Emacsで長い行を書いていると，デフォルトの状態ではどんどん横に伸びていく．後で読み返そうと思うと横にスクロールしないといけなくて，非常に不便である．M-qでauro-fillをやればよいと言われそうだが，そうすると改行されてしまい，これまた不便である．そこで，なんとかならないかと探してみると，ちゃんとそういうモノがあったので，まとめておく．

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [adaptive-wrap](#adaptive-wrap)
    - [インストールと設定](#インストールと設定)
    - [実際の使用例](#実際の使用例)

</div>
<!--endtoc-->


## adaptive-wrap {#adaptive-wrap}

-   参照1：[adaptive-wrap](https://elpa.gnu.org/packages/adaptive-wrap.html)　ご本家
-   参照2：[Correct indentation for wrapped lines](https://emacs.stackexchange.com/questions/14589/correct-indentation-for-wrapped-lines)
-   参照3：[Emacsの折り返しの挙動](http://alainmathematics.blogspot.com/2013/07/emacs.html)
-   参照4：[.emacs settings loading issue](https://www.reddit.com/r/emacs/comments/1kw7ip/emacs%5Fsettings%5Floading%5Fissue/)　

長い行をワープロのようにword-wrapしてくれるパッケージである．Emacsのバッファ上では折り返されているように見えるが，実際は長い横1行のままである．


### インストールと設定 {#インストールと設定}

例によって，use-packagを用いて以下のように，init.orgに書けばよい．

```lisp
#+begin_src emacs-lisp
(use-package adaptive-wrap
  :ensure t
  :config
  (setq-default adaptive-wrap-extra-indent 1)
  (add-hook 'visual-line-mode-hook #'adaptive-wrap-prefix-mode)
  (global-visual-line-mode +1)
  (add-hook 'org-mode-hook 'visual-line-mode)  ;; For org macros
  )
#+end_src
```

なお，最後の行を入れておかないと，org-mode fileに

```lisp
#+setupfile: /Sources/org-mode-folder/org-macros-master/org-macros.setup
```

を追加してマクロのパッケージを使用する場合（[Emacsのorg-modeで論文を書く（その5：htmlへのexportの際のフォントの色の変更，ハイライトなど）（12月19日追記）](../html_export)を参照のこと）に，adaptive-wrapが効かなくなる．


### 実際の使用例 {#実際の使用例}

adaptive-wrapをインストールしていない場合が上図，インストールして設定すれば下図のように見える．あくまで，Emacsの画面上でword-wrapしているように見えるだけで，実際のファイルではなが～い横１行のままの状態が維持されている．

{{< figure src="/img/Before_adaptive.jpg" width="100%" target="_self" >}}

{{< figure src="/img/After_adaptive.jpg" width="100%" target="_self" >}}

これも一度使い始めると，無くてはならないモノとなるパッケージである．
