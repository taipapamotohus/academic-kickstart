+++
title = "beamerでスライド原稿用pdfを作成する（その2）動画が走るスライド原稿を作る"
author = ["taipapa"]
date = 2018-08-25
lastmod = 2018-08-27T22:12:55+09:00
tags = ["beamer", "movie", "latex", "pdf"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/Kojidai2.jpg"
  caption = "Kojidai"
+++

実は，同じような内容について2010年に，[TeX QA](https://oku.edu.mie-u.ac.jp/tex/mod/forum/discuss.php?d=399#p2100) に投稿しているが，その後現在に至るまで使い続けることができており，一応，こちらでもまとめておくことにした．


## beamerで動画が走るスライド原稿を作る {#beamerで動画が走るスライド原稿を作る}

-   前述した ，[TeX QA](https://oku.edu.mie-u.ac.jp/tex/mod/forum/discuss.php?d=399#p2100) に経緯は投稿してあるので，詳細はそちらを読んでいただきたい．
-   参考にしたのは，Adobeのpdfのマニュアル（DVI specials for PDF generation） <https://www.tug.org/TUGboat/tb30-1/tb94cho.pdf> の以下の部分

    ```text
    3 Annotations

    An annotation is considered as an object with a location on a page. The type of the object is given by the value of the key `/Subtype', for instance, `/Text', `/Link', `/Sound', `/Movie', etc. (See [1, p. 615] for the list of all annotation types.) The location is given by an array object associated to the key `/Rect'. DVIPDFM(x) provides the following special command for annotations............

     The following example shows a movie annotation that enables us to run the movie file ‘mymovie.avi’ inside a PDF viewer program.

     \special{pdf:ann bbox 0 0 360 180 <<
     /Subtype /Movie /Border [1 0 0]
     /T (My Movie) /Movie <<
     /F (mymovie.avi) /Aspect [720 360]
     /Poster true >>
     /A << /ShowControls false >> >>}
    ```
-   アスペクト比やコントロールバーの有無，リピートするかどうかなども指示できる（下記参照）
-   mymovie.aviのところに動かしたい動画を記入（パスも効く）
-   私の作成したものは読んでいただければおわかりのように，非常にダサいベタ書きである．
-   最近の書き方は以下の通りで，もっぱら，mov形式の画像を使用している．コンテナがaviやwmvだと動かないが，Mac以外でどうなるのかは不明．そういえば，Windowsで試したことはなかった.....

    ```tex
    {
        \usebackgroundtemplate{\put(20, -265){\includegraphics[scale=0.45]{/Data/.../..../Figures/hogefuga.pdf}}}
        \begin{frame}
        \frametitle{hogefugaの対策}
        \special{pdf:ann bbox -10 -130 320 90 <<
                 /Subtype /Movie /Border [0 0 1]
                 /T (My Movie) /Movie <<
                 /F (/Data/.../hogefuga.mov)
                 %/Aspect [720 480]
                 /Aspect [640 480]
                 /Poster false >>
                 /A << /ShowControls true /Mode /Repeat >> >>}
        \end{frame}
    }
    ```
-   \usebackgroundtemplateの部分には動画のキャプチャー画像を貼り付けておく．なにもないと，動画が動き出す前の画面が空白になってしまう（もっと良い方法があれば，どなたかご教示ください）．
-   \putで背景画像（キャプチャー画像）の位置を直接指定し，\includegraphicsのscaleで倍率を指定して動画の大きさに合わせている．
-   これで，画像をクリックすると（ほぼ）同じ大きさの動画が（ほぼ）同じ位置で動くようになる
-   動画の大きさはbboxで，かぶせる静止画の大きさはscaleで調整する．
-   プレゼンテーションにskimを使うと動画が動かないので注意．
-   動画は同一directoryにある必要はなく，パスで指定すれば良い
-   動画自体はpdfの中に埋め込まれないので，pdfの容量がむやみに大きくならないという利点がある．
-   Adobe Acrobat Readerでプレゼンすれば，動画は動くし，音もでる．コントロールバーにより早送りなども可能．
-   最初にpdfで動画をクリックすると「セキュリティ上の問題．．．」というメッセージが表示される．この横にあるオプションボタンをクリックして，信頼するを選択すれば，動画が動くようになる．
