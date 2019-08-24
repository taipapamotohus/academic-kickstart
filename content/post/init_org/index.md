+++
title = "Emacsの設定（その2）設定ファイル（init.el）をorg-modeで管理する"
author = ["taipapa"]
date = 2018-08-17
lastmod = 2019-01-14T14:05:55+09:00
tags = ["emacs", "init-org", "init-el"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/Honolulu-1.jpg"
  caption = "Honolulu"
+++

自分のinit.elを見てると嫌になってくる．なんとかしようと弄り回すが，結局，訳わからんコードが山のように残ったまま．これをなんとかしようと，以前から気になっていたorg-modeでinit.elを管理するという方法を試してみた．まず，参考にしたサイトを最初にまとめておくので，そちらを見たほうが良いかもしれない．


## 参考サイト {#参考サイト}

-   [俺、ちゃんと全部管理してます（org-modeでinit.elを管理する）](http://blog.lambda-consulting.jp/2015/11/20/article/)
-   [babel-loader:org-mode で init.el を管理する方法](https://futurismo.biz/archives/6057/)
-   [平衡点(2011-12-13)](https://uwabami.junkhub.org/log/?date=20111213)
-   [ORG-Babel + init.el = ?? | くらいまーず　はい](https://ameblo.jp/concello/entry-10786074455.html)
-   [Prelude init.el & org-babel](https://funwithemacs.wordpress.com/2013/04/21/prelude-init-el-org-babel/)


## Preludeを使いながら，init.orgから個人用のinit.elを自動作成させてEmacsを設定する方法 {#preludeを使いながら-init-dot-orgから個人用のinit-dot-elを自動作成させてemacsを設定する方法}


### 基本方針 {#基本方針}

-   個人用の設定内容は，~/.emacs.d/personal/init.orgに書き込む．
-   起動時にEmacsはinit.orgを解釈できないので，init.elにはそれを解釈するように書き込む．
-   具体的には，init.elで，(require 'org)した後にorg-babel-load-fileでinit.orgを読み込む．
-   しかし，Preludeを導入しているので，そのまま~/.emacs.dにinit.elを書き込むわけにはいかず，少し工夫する．


### Preludeの導入 {#preludeの導入}

前回のポスト（[Emacsの設定（その1）Preludeの導入](../prelude_install)）を参考　


### emac-init.elの作成 {#emac-init-dot-elの作成}

-   ~/.emacs.d/personal/emacs-init.elというファイルを作成し，下記のように書き込む

```lisp
(require 'org)
(defvar my-config-dir (concat user-emacs-directory "personal/"))
(org-babel-load-file
 (expand-file-name "init.org" my-config-dir))
```

-   工夫と言っても， **init.elの名前のままではinit.orgからinit.elが生成されるときに衝突してしまう**  ので，違う名前（emacs-init.el）にしていることと，init.orgを~/.emacs.d/personal/に置くようにしているだけ．
-   これでEmacsを起動すると，init.org内のcode blockだけを抜き出したinit.elが同じdirectory (personal)に作成される．


### init.orgの作成 {#init-dot-orgの作成}

-   これでいよいよ肝心のinit.orgの作成を行う
-   org-modeについては，今更説明不要であろう．とにかくすごいやつ．超高機能アウトラインメジャーモード．文書作成，このブログ作成など殆どのことをこれでやっている．
-   具体的な内容は次回以降に記述予定だが，code blockの挿入は特筆すべき者であり，先に書いておく．．


#### Code blockの挿入 {#code-blockの挿入}

-   [俺、ちゃんと全部管理してます（org-modeでinit.elを管理する）](http://blog.lambda-consulting.jp/2015/11/20/article/)で指摘されているとおり，とにかく便利．以下はほとんどそのままコピペしたような記述である．
-   org-modeで以下のようにする．（後述する設定が必要）

    ```lisp
    <l （ここで<TAB>すると．．．）
    以下のように展開される
    #+begin_src emacs-lisp

    #+end_src
    ```

-    **2019年1月14日追加**

    上記の「TABで展開」に関して，Disqusで，mickaushyさんから「<lではなくて<sではないのか」とのご指摘をいただいた．全くそのとおりで，（後述する設定が必要）と自分で書いておきながら，その設定を書き忘れていた．mickaushyさんが指摘されている通りの設定をしている．

    -   参考：<http://pages.sachachua.com/.emacs.d/Sacha.html#org74bcbb3>

    ```lisp
    #+begin_src emacs-lisp
    (setq org-structure-template-alist
          '(("s" "#+begin_src ?\n\n#+end_src" "<src lang=\"?\">\n\n</src>")
            ("e" "#+begin_example\n?\n#+end_example" "<example>\n?\n</example>")
            ("q" "#+begin_quote\n?\n#+end_quote" "<quote>\n?\n</quote>")
            ("v" "#+BEGIN_VERSE\n?\n#+END_VERSE" "<verse>\n?\n</verse>")
            ("c" "#+BEGIN_COMMENT\n?\n#+END_COMMENT")
            ("p" "#+BEGIN_PRACTICE\n?\n#+END_PRACTICE")
            ("l" "#+begin_src emacs-lisp\n?\n#+end_src" "<src lang=\"emacs-lisp\">\n?\n</src>")
            ;; ("l" "#+begin_src lisp\n?\n#+end_src" "<src lang=\"lisp\">\n?\n</src>")
            ("L" "#+latex: " "<literal style=\"latex\">?</literal>")
            ("h" "#+begin_html\n?\n#+end_html" "<literal style=\"html\">\n?\n</literal>")
            ("H" "#+html: " "<literal style=\"html\">?</literal>")
            ("a" "#+begin_ascii\n?\n#+end_ascii")
            ("A" "#+ascii: ")
            ("i" "#+index: ?" "#+index: ?")
            ("I" "#+include %file ?" "<include file=%file markup=\"?\">")))
    #+end_src
    ```

    この設定を，init.orgに書き込んでおいて，「<lのあとにTAB」とすれば，上述のように展開される．この次の記事でまとめて書こうと考えていたが，すっかり失念していた．ここに書いておいたほうが確かにわかりやすい．mickaushyさん，ご指摘ありがとうございました．


#### Codeの記述 {#codeの記述}

-   上記の#+begin\_src emacs-lispと#+end\_srcの間にemacs-lispで設定内容を書く．ここからが便利にできているところ．
-   code-blockの中にいるときに
    -   C-c 'とする
    -   当該のcode blockだけのバッファが表示される（下図の下のバッファ）

        {{< figure src="/img/CodeBlock_small.jpg" width="100%" target="_self" >}}

    -   この中はemacs-lisp modeでsyntaxも普通に効くので，普通にコードを書く．もちろん，括弧の対応もハイライトで表示される．
    -   C-c nとする
    -   すると，インデントも綺麗に整えてくれる．
    -   満足したら，C-c 'で元のバッファに戻る．整形は綺麗なまま反映される．素晴らしい！
-   具体的なinit.orgの内容は次回のポスト以降に記述予定
