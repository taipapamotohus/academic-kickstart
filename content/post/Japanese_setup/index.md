+++
title = "Emacsの設定（その3）ようやくinit.orgの記述: 日本語の設定，inline-patchの設定など"
author = ["taipapa"]
date = 2018-08-18
lastmod = 2018-08-27T22:12:54+09:00
tags = ["emacs", "prelude", "init-el"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/Colosseum.jpg"
  caption = "Colosseum"
+++

ようやく，ここからinit.orgの具体的な記述になる．

{{% toc %}}

## Coding systemの設定 {#coding-systemの設定}

-   まずは，coding systemの設定，つまり，日本語の設定，日本語フォントの設定から
-   init.orgに以下のように書き込む

    ```lisp
    ​* Coding System Environment
    ** 言語を日本語にする
       #+BEGIN_SRC lisp
       (set-language-environment 'Japanese)
       #+END_SRC
    ** 極力UTF-8とする
       #+BEGIN_SRC lisp
         (prefer-coding-system 'utf-8)
       #+END_SRC
    ** 日本語フォントをヒラギノにする
    ​   - 日本語のサイズを指定しないと動的にサイズを変えられるようになる
    ​   - 奥村先生のサイト参照 https://oku.edu.mie-u.ac.jp/~okumura/macosx/
       #+BEGIN_SRC lisp
         (when (or (eq window-system 'mac) (eq window-system 'ns))
           (set-face-attribute 'default nil
                               :family "Menlo"
                               :height 180) ;; 18pt
           (set-fontset-font nil 'japanese-jisx0208
                             (font-spec :family "Hiragino Kaku Gothic ProN"))
           (setq face-font-rescale-alist
                 '((".*Hiragino Kaku Gothic ProN.*" . 1.1))))
       #+END_SRC
    ```
-   これがEmacs起動時にorg-babel-load-fileにより変換されて下記のようなinit.elとなる．

    ```lisp
    (set-language-environment 'Japanese)

    (prefer-coding-system 'utf-8)

    (when (or (eq window-system 'mac) (eq window-system 'ns))
      (set-face-attribute 'default nil
                          :family "Menlo"
                          :height 180) ;; 18pt
      (set-fontset-font nil 'japanese-jisx0208
                        (font-spec :family "Hiragino Kaku Gothic ProN"))
      (setq face-font-rescale-alist
            '((".*Hiragino Kaku Gothic ProN.*" . 1.1))))
    ```
-   つまり，org-modeで書いたinit.orgでの解説はすべて除かれて，lispのみのcodeになってinit.elが生成される．
-   この利点は，init.elの説明が実に書きやすい点にある（実際にはinit.orgに書くわけだが．．．）．org-modeはアウトライナーなので，階層構造も自由自在である．整理もしやすいし，後で順番を変えるのもCommand + arrow keyを使えば実に簡単である．


## Inline-patchの設定 {#inline-patchの設定}

-   ついで，最も重要なinline-patchの設定
-   参考：[Macに最新バージョンのEmacsをインストール](http://keisanbutsuriya.hateblo.jp/entry/2016/04/10/115945)
-   参考：[El Capitan での日本語入力時に Emacs 内のカーソル色を変更する](http://suzuki.tdiary.net/20160103.html)
-   init.orgに以下のように書き込む．

    ```lisp
    ​* inline-patch on macosx
    ** ミニバッファ入力時に自動的に英語入力モードにする
    ​   - 参考：http://keisanbutsuriya.hateblo.jp/entry/2016/04/10/115945
       #+BEGIN_SRC lisp
         (when (functionp 'mac-auto-ascii-mode)  ;; ミニバッファに入力時、自動的に英語モード
           (mac-auto-ascii-mode 1))
       #+END_SRC
    ** 日本語か英語かで，カーソルの色を変える．
    ​   - 参考１：http://keisanbutsuriya.hateblo.jp/entry/2016/04/10/115945
    ​   - 参考２：http://suzuki.tdiary.net/20160103.html
       #+BEGIN_SRC lisp
         (when (fboundp 'mac-input-source)
           (defun my-mac-selected-keyboard-input-source-chage-function ()
             (let ((mac-input-source (mac-input-source)))
               (set-cursor-color
                                                 ; (if (string-match "com.apple.inputmethod.Kotoeri.Roman" mac-input-source)
                (if (string-match "com.google.inputmethod.Japanese.Roman" mac-input-source)
                    "#91C3FF" "#FF9300"))))
           (add-hook 'mac-selected-keyboard-input-source-change-hook
                     'my-mac-selected-keyboard-input-source-chage-function))
       #+END_SRC
    ```
-   これがEmacsの起動時に，org-babel-load-fileによって，下記のようにcodeだけ抜き出されて，init.elに書き込まれる．

    ```lisp
    (when (functionp 'mac-auto-ascii-mode)  ;; ミニバッファに入力時、自動的に英語モード
      (mac-auto-ascii-mode 1))

    (when (fboundp 'mac-input-source)
      (defun my-mac-selected-keyboard-input-source-chage-function ()
        (let ((mac-input-source (mac-input-source)))
          (set-cursor-color
                                            ; (if (string-match "com.apple.inputmethod.Kotoeri.Roman" mac-input-source)
           (if (string-match "com.google.inputmethod.Japanese.Roman" mac-input-source)
               "#91C3FF" "#FF9300"))))
      (add-hook 'mac-selected-keyboard-input-source-change-hook
                'my-mac-selected-keyboard-input-source-chage-function))
    ```
-   これで日本語入力中であっても，M-xなどでミニバッファ入力時に自動的に英語入力モードになってくれる．
-   ついでに行った日本語か英語かでカーソルの色が変わる設定はわりに有用だが，ときに色が変わらないことがあるが，気にしないことにしている．
