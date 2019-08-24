+++
title = "beamerでスライド原稿用pdfを作成する（その3）動画が走るスライド原稿を作る（media9に関する追記）"
author = ["taipapa"]
date = 2018-08-26
lastmod = 2018-08-27T22:12:55+09:00
tags = ["beamer", "movie", "latex", "pdf"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/Florence-1.jpg"
  caption = "Florence"
+++

前回のポストを書いた後に，念のために調べてみると，どうやら，media9なるものが，いまやpdfに動画を埋め込むために普通に使われているようだということが判明してしまった！う〜む，勉強不足を痛感する．遅れを取り返すべく，早速やってみたので，それを追加報告とする．

{{% toc %}}

## media9 {#media9}

media9はtexlive2018に含まれており，特に別途インストールする必要はなかった．media9については様々な情報があったが，多くはWindowsやLinuxに関してであり，そのままコピペして動くような極楽情報はなかなか見つからなかった．やはり，餅は餅屋で，OS X TeXにそのものズバリの情報があった．それが参考サイト５である．

-   参考サイト１：[TeXWiki media9](https://texwiki.texjp.org/?media9)
-   参考サイト２：[TeXでプレゼン - LaTeX Beamerを使う人のためのTips集](https://blog.tokor.org/2016/06/04/TeXでプレゼン-LaTeX-Beamerを使う人のためのTips集/)
-   参考サイト３：[How to embed video files in a PDF using LaTeX (a media9/beamer example)](https://www.youtube.com/watch?v=HHzcrP27I08)
-   参考サイト４：[Beamer で PDF ファイルに動画を埋め込む](http://empitsu.hatenablog.com/entry/2012/11/02/182722)
-   参考サイト５：[media9 problems](http://tug.org/pipermail/macostex-archives/2014-July/052673.html)


## beamerで動画が走るスライド原稿を作る（media9版） {#beamerで動画が走るスライド原稿を作る-media9版}

-   同一directoryにhogefuga.movがあるとすれば，以下のように書けば良い
-   preambleに，\usepackage{media9}を忘れずに追加しておく．

    ```tex
    \newcommand{\showmovie}[1]{\includemedia[
      activate=pageopen,
      deactivate=pageclose,
      width=110mm, height=72mm,
      addresource=#1,
      flashvars={
        src=#1
        &loop=true
        &autoPlay=false
      }
      ]{}{StrobeMediaPlayback.swf}
    }

    \begin{frame}
      \frametitle{hogefugaの動画}
      \centering
      \showmovie{hogefuga.mov}
    \end{frame}
    ```
-   110mmと72mmの数値はフレーム内の動画の収まり具合により適宜調整する．
-   loopは，ループ再生するかどうか
-   autoplayは自動再生するかどうか


## media9を使用したときの利点 {#media9を使用したときの利点}

-   なんと言ってもべた書きよりもelegant!
-   スライドを開けたときに，何もしなくても動画の静止画が映るので，前回のやり方のように背景をべた書きしなくて良い．


## media9を使用したときの欠点 {#media9を使用したときの欠点}

もう完全にmedia9に乗り換えるつもりでいたが，以下に述べるようにいくつか欠点もあることが判明した．

-   media9はpdf自体に動画を埋め込むようである．したがって，200MBの動画を走らせるとすると，pdf自体が200MB以上の大きさになってしまう．
-   それだけでなく，200MB程度の動画になると，途中で固まってしまう！これでは使い物にならない．
-   一方，前回記事のべた書き方式だと，pdf自体に動画を埋め込まないので，pdfは大きくならないし，動画指定のパスは効くし，200MBだろうともっと大きかろうと動画はガンガン動く．


## 結論 {#結論}

-   容量の小さな動画であれば，media9でも十分であろう．
-   私のように，容量の大きな動画を使用するような場合は，べた書きを使用するほうが良いであろう．
-   ということで，結局，元の木阿弥に戻ることとなった．
