+++
title = "Swiper, ivy, avy, migemoによるEmacsの検索強化"
author = ["taipapa"]
date = 2018-10-14
lastmod = 2018-10-14T17:28:22+09:00
tags = ["emacs", "swiper", "ivy", "migemo", "search", "avy"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/Pont_des_Arts.jpg"
  caption = "Pont des Arts"
+++

文章を書いている際に，ある単語を検索したくなるようなことがよくある．Emacsでのデフォルトはisearchであるが，今回は，これを強化するpackageを紹介する．また，最近開いたディレクトリ directory をまた開きたいこともよくあることである．これについてもivyによる検索が便利であるので紹介する．例によってネタ元を見たほうが早いかもしれない．．．(^^;;;

{{% toc %}}

## swiper.el {#swiper-dot-el}

-   参照1：[swiper](https://github.com/abo-abo/swiper)  ご本家
-   参照2：[swiper.el: 一覧付き正規表現isearch！C-sを置き換えろ](http://emacs.rubikitch.com/swiper/)
-   参照3：[Emacsの補完&検索を超強化する](https://qiita.com/blue0513/items/c0dc35a880170997c3f5)
-   Emacsでは，C-sに割り当てられたisearchによる正規表現検索がデフォルトで存在する．これをivyを用いて一覧を付加するようにした強化版である．


### Install & setup {#install-and-setup}

-   以下を，int.orgに書き込む．

```lisp
#+begin_src emacs-lisp
(use-package swiper
  :ensure t
  :config
  (defun isearch-forward-or-swiper (use-swiper)
    (interactive "p")
    ;; (interactive "P") ;; 大文字のPだと，C-u C-sでないと効かない
    (let (current-prefix-arg)
      (call-interactively (if use-swiper 'swiper 'isearch-forward))))
  (global-set-key (kbd "C-s") 'isearch-forward-or-swiper)
  )

(use-package ivy
  :ensure t
  ;; :config
  ;; (fset 'ivy--regex 'identity)
  )
#+end_src
```

-   ivyのコメントアウトしている部分については後述する．


### 使い方 {#使い方}

-   現在開いているEmacsのバッファで，C-sとするだけでよい．
-   下図は，ivyを検索しているところだが，下に"Swiper"と表示されるバッファが表示され，そこに”ivy”と打つと，上の本文の中のivyは黄色でハイライトされる．同時に下のバッファでは，現在見ているivyのある行に下線が引かれ，行数が横に示される．上下のArrow Keyでivyのある行から次の行に飛べる．リターンすれば本文のその行に行ける．非常に便利である．

    {{< figure src="/img/swiper-1a.jpg" width="100%" target="_self" >}}


## migemo {#migemo}

-   参照1：[Migemo: ローマ字のまま日本語をインクリメンタル検索](http://0xcc.net/migemo/)
-   参照2：[【Emacs/macOS】migemoを有効にし、ローマ字のまま日本語検索する](https://www.yokoweb.net/2017/03/05/emacs-macos-migemo/)
-   migemoとは，「ローマ字のまま日本語をインクリメンタル検索するため のツールです。かな漢字変換をすることなく日本語のインクリメン タル検索を快適に行うことができます。」
-   一度使い始めるとやみつきになるので，オススメ！


### cmigemoのinstall {#cmigemoのinstall}

-   まず，C言語で再実装されたcmigemoをインストールする．homebrewで簡単にインストールできる．--HEADのオプションが必要との記載もあるが，なくても同じであった．

```shell
$ brew install cmigemo
```


### migemo.elのInstall & setup {#migemo-dot-elのinstall-and-setup}

-   以下を，int.orgに書き込む．

```lisp
#+BEGIN_SRC emacs-lisp
(use-package migemo
  :ensure t
  :config
  ;; C/Migemo を使う場合は次のような設定を .emacs に加えます．
  (setq migemo-command "cmigemo")
  (setq migemo-options '("-q" "--emacs" "-i" "\a"))
  (setq migemo-dictionary "/usr/local/Cellar/cmigemo/20110227/share/migemo/utf-8/migemo-dict")  ;; 各自の辞書の在り処を指示
  (setq migemo-user-dictionary nil)
  (setq migemo-regex-dictionary nil)
  ;; charset encoding
  (setq migemo-coding-system 'utf-8-unix))
#+END_SRC
```


## avy-migemo（swiperのmigemo対応） {#avy-migemo-swiperのmigemo対応}

-   参照１：[avy-migemo](https://github.com/momomo5717/avy-migemo/blob/master/README.jp.org)
-   参照２：[avy と migemo を組み合わせたパッケージ avy-migemo.el のご紹介](https://dev.classmethod.jp/tool/emacs-avy-migemo/)
-   参照３：[avyのmigemo対応およびswiperのmigemo対応](https://qiita.com/ballforest/items/7810e229d6f771d0ab16)
-   上記のswiperだけでも十分に便利であるが，swiperをmigemoに対応させることで，更に便利になる．
-   前述したswiper.elでのコメントアウトした設定部分，つまり，

    ```emacs-lisp
    ;; (fset 'ivy--regex 'identity)
    ```

    は，コメントアウトしておかないと，migemo化を無効にしてしまうので注意。


### Install & setup {#install-and-setup}

-   以下を，int.orgに書き込む．

```lisp
#+begin_src emacs-lisp
(use-package avy-migemo
  :ensure t
  :config
  (avy-migemo-mode 1)
  (setq avy-timeout-seconds nil)
  (require 'avy-migemo-e.g.swiper)
  (global-set-key (kbd "C-M-;") 'avy-migemo-goto-char-timer)
  ;;  (global-set-key (kbd "M-g m m") 'avy-migemo-mode)
  )
#+end_src
```


### 使い方 {#使い方}

-   現在開いているEmacsのバッファで，C-sとするだけでよい．
-   下図は，"taiou"，つまり，「対応」を検索しているところである．migemo化する前と同じようにローマ字で日本語が検索できる．
-   当たり前だが，漢字を入力しても検索できる．

    {{< figure src="/img/swiper-2a.jpg" width="100%" target="_self" >}}


## 最近開いたディレクトリを開く {#最近開いたディレクトリを開く}

-   参照：[OPEN A RECENT DIRECTORY IN DIRED: REVISITED](http://pragmaticemacs.com/emacs/open-a-recent-directory-in-dired-revisited/)　ネタ元
-   ivyを使って最近開いたディレクトリを動的に探索する方法をコード化してくれているので紹介する．
-   以下のコードをinit.orgに書き込めば良い．

    ```lisp
    #+begin_src emacs-lisp
    (defun bjm/ivy-dired-recent-dirs ()
      "Present a list of recently used directories and open the selected one in dired"
      (interactive)
      (let ((recent-dirs
             (delete-dups
              (mapcar (lambda (file)
                        (if (file-directory-p file) file (file-name-directory file)))
                      recentf-list))))

        (let ((dir (ivy-read "Directory: "
                             recent-dirs
                             :re-builder #'ivy--regex
                             :sort nil
                             :initial-input nil)))
          (dired dir))))

    (global-set-key (kbd "C-x C-d") 'bjm/ivy-dired-recent-dirs)
    #+end_src
    ```

<!--listend-->

-   C-x C-dすれば，下図のように，最近開いたディレクトリが表示され，その中から行きたいディレクトリを選んで，リターンすれば良い．

    {{< figure src="/img/directory-1a.jpg" width="100%" target="_self" >}}

以上，今回は小ネタだが非常に有用なものばかりを紹介した．
