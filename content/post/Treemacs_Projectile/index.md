+++
title = "Treemacs and Projectile"
author = ["taipapa"]
date = 2019-08-25
lastmod = 2019-08-31T21:19:48+09:00
tags = ["emacs", "projectile", "treemacs", "helm", "project"]
type = "post"
draft = false
weight = 1
subtitle = "a tree layout file explorer for Emacs and Project Interaction Library for Emacs"
[image]
  placement = 3
  caption = "Sforza monument, Nagoya"
+++

MacのFinderに相当するのは，EmacsではDiredであろう．しかし，なにかのプロジェクトに関わる文書群を管理するということになると，Diredでは力不足である．というか目的が違う．これにぴったりなのが，treemacsであり，そのバックボーンになるのが，Projectileである．これらは，プログラマーでもない自分には必要ないものと思っていたのだが，このblogを書くのに使用しているHugoとそのテーマであるacademicをアップデートする際に，非常に役に立ったので，いまだによく分かっていない自分自身のために書き留めておく．  <!--more-->


## Projectile {#projectile}


### References {#references}

-   [projectile](https://github.com/bbatsov/projectile)   ご本家
-   [Projectile: The Project Interaction Library for Emacs](https://www.projectile.mx/en/latest/)  ご本家の解説
-   [よく使っているEmacsの拡張](http://blog.aqutras.com/entry/2016/06/15/210000)
-   [ProjectileとHelm](https://tech.camph.net/projectile-and-helm/)
-   [Exploring large projects with Projectile and Helm Projectile](http://tuhdo.github.io/helm-projectile.html)

以下はProjectileご本家の解説からの抜粋

-   「外部への依存関係を導入することなく，プロジェクトを操作する便利な一連の特徴を提供することを目的とする」そうである．これだけでは何のこっちゃである．
-   「例えば，プロジェクトの文書を見つける機能はGNUのfindを使用せずに純粋にEmacs Lispによって実装されている」ということで，そういうことかと思う．
-   プロジェクトとは，特定のファイルを含むフォルダーのこと
-   version-controlであるgit, muecurial, bazaarなどのrepoはデフォルトでプロジェクトとみなされる．


### Installation {#installation}

以前の記事（[Emacsの設定（その1）Preludeの導入（2018年10月9日修正，2019年6月1日追記）](../prelude_install)）に書いたように，私は，Preludeを導入しているが，これにより，prejectileのインストールと設定は既に終わっている．マニュアルで入れる場合は，以下のようにinit.orgに書き込めば良い．

```lisp
#+begin_src emacs-lisp
(use-package projectile
  :ensure t
  :config
  (define-key projectile-mode-map (kbd "s-p") 'projectile-command-map)
  (define-key projectile-mode-map (kbd "C-c p") 'projectile-command-map)
  (projectile-mode +1))
#+end_src
```


### Usage {#usage}

多分，このソフトは解説を読んでいても，有り難みがさっぱり分からない（私がそうであった）．gitを使って開発をしている人とかにはすごく役に立つと思う．では，プログラマーでもない私の様な素人が使ってなんの役に立つのかと思われるであろうが，これが，案外便利なのである．


#### Basic Usage {#basic-usage}

個人的に実際に使うのは，以下の4つぐらい．

-   current project内のファイルを検索: C-c p f
-   current project内のディレクトリを検索: C-c p d
-   current project内のディレクトリ内のファイルを検索: C-c p l
-   current project内のファイルの中身をgrepで検索する: C-c p s g

さらに，[which-key](https://github.com/justbur/emacs-which-key/tree/42a25055163141165aa0269dbca69735e704825c) を導入しておくと，C-c pと打って，「次，なんだっけ？」と考えて1秒経つうちに，下図の様にメニューが下のバッファに表示される．私は，以前に書いた様にPreludeを導入しているが（[Emacsの設定（その1）Preludeの導入（2018年10月9日修正，2019年6月1日追記）](../prelude_install)），Preludeの導入により，which-key はインストールと設定が終わっている様であり，何もせずとも，下図の様になった．これは，/Data/MyBlog/TaipapablogをDiredで開けて，そこで，C-c p と打ってから1秒が経過した時の様子である．これでkey-bindは，C-c p まで覚えておけば良いことがわかった．

{{< figure src="/img/Projectile-which-key.jpg" width="80%" target="_self" >}}

例えば，あるディレクトリ内の文書を比較したりコピペしたりしたいときに，すぐに探し出せるのが便利である．文書名を忘れていても，あるキーワードを含む文書を探すということも簡単にできる（current project内のファイルの中身をgrepで検索する: C-c p s g）．そんなことは，別に，Finderのfindを使うなり，Terminalでgrepなりmdfindすればできるわけだが，何も面倒なコマンドを打たずとも，特定のproject，つまり，特定のディレクトリ内だけで検索ができるというのが肝である．これにより，一瞬で検索は終わるし，すぐにそのファイルに飛べる．後述するtreemacsをインストールせずとも，projectileだけでも，かなり，便利になると思う．例えば，下の画像は，Taipapablogというdirectoryの中にあり，\_index.mdを名前に含むファイルを検索しているところである．下のバッファにズラズラと該当するファイルが並んでおり，C-jするとその中身が上のバッファに表示される．リターンすれば，そのファイルが開く．下のバッファでは，arrow keyで上下すれば別のファイルに行けて，そこでC-jすれば，その中身が見られる．Returnするまではこれを繰り返すことができる．

{{< figure src="/img/Projectile-Find.jpg" width="80%" target="_self" >}}

プログラミングをやっているわけではなく，論文を書くのにEmacsを使用している私のようなレベルの人間にとっても，一つのプロジェクト内のファイルを縦横無尽に検索や閲覧ができるのは，かなり，有用である．以下のtreemacsと組み合わせると，さらに便利になる（ような気がしている　笑）．最近のEmacsは画像でもpdfでも閲覧できるので，応用範囲はかなり広い．


## Treemacs {#treemacs}

一見，neotreeの様に見えるが，特定のprojectに割り当てられている様な仕組みになっている．分かりにくいが，実際に使ってみれば便利である．treemacs-projectileをインストールすることにより，上述のprojectileと統合した状態で使えて，より便利になる．


### References {#references}

-   [Treemacs - a tree layout file explorer for Emacs](https://github.com/Alexander-Miller/treemacs)  ご本家
-   [Emacsの設定を色々いじった -その１-](https://blog.deltabox.site/post/2019/04/emacs%5Fconfig%5Fin%5Fmarch/)
-   [Using all-the-icons for Treemacs](https://blog.jft.rocks/emacs/treemacs-icons.html)


### Installation {#installation}

ご本家の方法を丸写ししておく．以下をinit.orgに書き込めば良い．デフォルト設定なので，これ全部写す必要はなさそうだが．．．

```lisp
#+begin_src emacs-lisp
(use-package treemacs
  :ensure t
  :defer t
  :init
  (with-eval-after-load 'winum
    (define-key winum-keymap (kbd "M-0") #'treemacs-select-window))
  :config
  (progn
    (setq treemacs-collapse-dirs                 (if (treemacs--find-python3) 3 0)
          treemacs-deferred-git-apply-delay      0.5
          treemacs-display-in-side-window        t
          treemacs-eldoc-display                 t
          treemacs-file-event-delay              5000
          treemacs-file-follow-delay             0.2
          treemacs-follow-after-init             t
          treemacs-git-command-pipe              ""
          treemacs-goto-tag-strategy             'refetch-index
          treemacs-indentation                   2
          treemacs-indentation-string            " "
          treemacs-is-never-other-window         nil
          treemacs-max-git-entries               5000
          treemacs-missing-project-action        'ask
          treemacs-no-png-images                 nil
          treemacs-no-delete-other-windows       t
          treemacs-project-follow-cleanup        nil
          treemacs-persist-file                  (expand-file-name ".cache/treemacs-persist" user-emacs-directory)
          treemacs-recenter-distance             0.1
          treemacs-recenter-after-file-follow    nil
          treemacs-recenter-after-tag-follow     nil
          treemacs-recenter-after-project-jump   'always
          treemacs-recenter-after-project-expand 'on-distance
          treemacs-show-cursor                   nil
          treemacs-show-hidden-files             t
          treemacs-silent-filewatch              nil
          treemacs-silent-refresh                nil
          treemacs-sorting                       'alphabetic-desc
          treemacs-space-between-root-nodes      t
          treemacs-tag-follow-cleanup            t
          treemacs-tag-follow-delay              1.5
          treemacs-width                         35)

    ;; The default width and height of the icons is 22 pixels. If you are
    ;; using a Hi-DPI display, uncomment this to double the icon size.
    ;; (treemacs-resize-icons 44)

    (treemacs-follow-mode t)
    (treemacs-filewatch-mode t)
    (treemacs-fringe-indicator-mode t)
    (pcase (cons (not (null (executable-find "git")))
                 (not (null (treemacs--find-python3))))
      (`(t . t)
       (treemacs-git-mode 'deferred))
      (`(t . _)
       (treemacs-git-mode 'simple))))
  :bind
  (:map global-map
        ("M-0"       . treemacs-select-window)
        ("C-x t 1"   . treemacs-delete-other-windows)
        ("C-x t t"   . treemacs)
        ("C-x t B"   . treemacs-bookmark)
        ("C-x t C-t" . treemacs-find-file)
        ("C-x t M-t" . treemacs-find-tag)))

;; (use-package treemacs-evil
;;   :after treemacs evil
;;   :ensure t)

(use-package treemacs-projectile
  :after treemacs projectile
  :ensure t)

(use-package treemacs-icons-dired
  :after treemacs dired
  :ensure t
  :config (treemacs-icons-dired-mode))

(use-package treemacs-magit
  :after treemacs magit
  :ensure t)
#+end_src
```


### Usage {#usage}

上記設定により，projectileと統合した状態で使用することになる．従って，git initしたdirectoryや，git cloneしたdirectoryが対象となる．それらに該当しなければ，directory内に.projectileという 空ファイルを作成すれば良い．まず，最初は， **M-0** と叩いて，Treemacsを開き，C-c C-p a (treemacs-add-project-to-workspace) でプロジェクトをtreemacsのworkspaceに追加する．下図は，Taipapablogというdirectoryを開けて，そこから2つのファイルを横に並べて開いたところである．

左のtreemacsのバッファの行番号114のさらに左のfringeに小さな青いマークがついている．行のハイライトとともに現在アクティブなバッファがどれかを示している．複数のプロジェクトや複数のディレクトリに同じ名前のファイルがあるときなどは，今作業しているファイルが，どこにあるファイルがわからなくなって困ることがある（少なくとも私は）．そのようなときに，このfringe indicatorは有用である．

{{< figure src="/img/Treemacs-fringe.jpg" width="100%" target="_self" >}}

ところで，上述したようなキーバインドを覚える必要はない．treemacsのバッファにいるときに，？を叩けば，下図のごとく，下にヘルプバッファが開く．楽チンである．ファイルやディレクトリやプロジェクトの追加，削除，名前の変更などはもちろん網羅しており，ファイルの開け方も横に並べたり，縦に並べたりと色々できるようになっている．

<img src="/img/Treemacs-help.jpg" alt="Treemacs-help.jpg" width="100%" />
使い始めたときに問題となったのは，treemacsのコマンドはtreemacsのバッファにいるときでないと効かないことである（projectileのコマンドは何処でも効く）．いちいち，マウスでtreemacsのバッファをクリックしてそちらに移ってからコマンドを打たないといけないようではやってられない．これでは，Macのfinderと同じである．そこで，ご本家のサイトをよく読むと， **Winum & ace-window compatibility** と書いてある．上述したインストールのためのuse-packageの設定のconfigにも， **(define-key winum-keymap (kbd "M-0") #'treemacs-select-window)** と書いてある．つまり，横や縦に並べたバッファ間の移動は， **C-x o** の後に行きたいバッファの番号を打てばよく，treemacsのバッファに戻るには， **M-0** を打てば良い．下図は，先ほどの図の状態で， **C-x o** を打った時の様子である．茶色の小さな数字がそれぞれのバッファに割り当てられた番号である．

なお，winumのインストールについては，[emacs-winum](https://github.com/deb0ch/emacs-winum) を参照されたい．

{{< figure src="/img/Treemacs-winum.jpg" width="100%" target="_self" >}}

とは言うものの，やはり，マウスを使う方が便利なこともある．Treemacsはmouse interfaceにも完全に対応しており，右クリックでpopup-menuが出るようになっている（下図参照）．よくできている．

{{< figure src="/img/Treemacs-mouse.jpg" width="100%" target="_self" >}}

まだ使い始めたばかりであり，projectileとtreemacsについて，まだまだ理解しないといけないことがたくさんあるが，日常的に使用できるところまではなんとかなったかな．．．
