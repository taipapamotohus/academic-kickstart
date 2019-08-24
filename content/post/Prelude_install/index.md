+++
title = "Emacsの設定（その1）Preludeの導入（2018年10月9日修正，2019年6月1日追記）"
author = ["taipapa"]
date = 2018-08-15
lastmod = 2019-06-01T15:02:39+09:00
tags = ["emacs", "prelude"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/Istanbul-long.jpg"
  caption = "Istanbul"
+++

昔はいろいろイチからinit.elを設定したものだけど，今は最初からほぼ全部やってくれる設定集がある．


## Preludeとは {#preludeとは}

-   Emacs初期設定集の一種．他にもいろいろあるようだが，これしか使ったことがないので．．．
-   [Prelude](https://github.com/bbatsov/prelude) Githubはこちら


## Preludeのインストール {#preludeのインストール}

-   上記のGithubのFast Forwardに書いてあるように，macOSで既にgitとcurlとEmacsをインストールしていれば，下記のコマンドを打てば，Preludeがインストールされる．念のために，古い.emacs.dはどこかに退避させておく（名前を変えて保存されるようになってはいるが心配性なもんで）．

    ```shell
    $ curl -L https://git.io/epre | sh
    ```
-   インストール終了後にEmacsを再起動すると，勝手にもの凄い勢いでどんどん各種パッケージをダウンロードしてインストールしてくれる．~~auctexなどもインストールされるので，LaTeXも使えるようになる．素晴らしい！~~ **（2018年10月9日修正）** デフォルトのままではこうならない．下記のように設定する必要あり．
-   prelude-modules.elを見て必要なmodulesがロードされるようにコメントアウトを外す．

    ```lisp
    (require 'prelude-helm) ;; Interface for narrowing and search
    (require 'prelude-helm-everywhere) ;; Enable Helm everywhere
    ..................
    (require 'prelude-latex)
    ```

    helmにすっかり慣らされてしまったのでこのあたりは外している．prelude-latexのコメントアウトを外すことにより，次回にEmacsを立ち上げるときに **auctex** がインストールされる．
-   preludeのdefaultの設定は，ほぼ満足すべきものだが，ひとつだけ，prelude-auto-saveが邪魔である．これは，他のbufferに移動したり，他のアプリを使用したりするたびに自動で保存される機能であるが，非常に鬱陶しい．M-x customize からprelude-auto-saveを検索し，offにする．custom.elに書き込まれる．
-   **super-save** について（ **2019年6月1日追記** ）：この記事を書いてからすぐに気がついたのだが，現在のpreludeはsuper-saveがdefaultで設定されるので，こちらもoffにしないと自動保存が行われてしまう．これは好みの問題だとは思うが，私は嫌いなのでoffにしている．遅まきながら追記しておく．設定の変更は上述のとおりで，M-x customize からsuper-saveを検索し，offにする．下図を参考にしていただきたい．

    {{< figure src="/img/Super-save.jpg" width="100%" target="_self" >}}

-   個人の設定は，~/.emacs.d/personal/init.elに書き込む．これは，org-modeを使って設定するのが良い．まずorg-modeについての記事を書いたあとにinit.orgについて書く予定．
