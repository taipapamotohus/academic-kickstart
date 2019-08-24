+++
title = "beamerでスライド原稿用pdfを作成する（その１）"
author = ["taipapa"]
date = 2018-08-24
lastmod = 2018-09-30T14:49:51+09:00
tags = ["latex", "latexmk", "beamer", "texlive", "mactex", "emacs"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/Kojidai.jpg"
  caption = "Kojidai"
+++

前回のポスト（[LaTeXをインストールし，texファイルが変更されると，自動的にcompileしてskimでのpdfも自動で更新されるようにする（2018年9月1日追記）](../latexmk)）により，既にLaTeXはインストールされたので，今回は学会発表向けのスライド原稿作成についてまとめる．ようやく実質的な話になる．
　


## beamerによるスライド原稿の作成 {#beamerによるスライド原稿の作成}

-   基本的には，通常のLaTeX文書と同じである．実際のスライド原稿を見てもらうほうが早いであろう．以下のtexファイルを作成し，beamer\_test.texと名付け，保存する．Editorは何でも良いが，やはり，Emacsのauctexを使うと補完などあり，便利である．
-   下記のファイルには多少コメントを付けた．フォントの指定は自明．themeは山のようにあるので，ググって好きなものを使う．
-   なお， \usefonttheme{professionalfonts} を入れているのは，これを入れないと，beamer は数式フォントとして sans に指定されたフォントを使うように内部で変更するからで，これを入れるとこの変更を無効にできる．数式がヒラギノになると間抜けである．昔，TeX QAで教えていただいた．参考：[beamerでの数式フォントの変更](https://oku.edu.mie-u.ac.jp/tex/mod/forum/discuss.php?d=729)
-   また，いろいろ余分なパッケージも読み込んでいるが，必要なときに書き込めば良く，不要なら削除する．

```tex
% -*-coding:utf-8-*-
\documentclass[svgnames, table, 14pt]{beamer}
\usepackage{zxjatype}
\usepackage[hiragino-dx]{zxjafont}

% ヒラギノ角ゴ Proを使う
\setjamainfont[Scale=0.95,BoldFont=ヒラギノ角ゴ Pro W6]{ヒラギノ角ゴ Pro W3}
\setjasansfont[Scale=0.95,BoldFont=ヒラギノ角ゴ Pro W6]{ヒラギノ角ゴ Pro W3}

% themeを指定する
\usetheme{Darmstadt}

\usefonttheme[onlylarge]{structurebold}
\setbeamerfont*{frametitle}{size=\large,series=\bfseries}
\setbeamertemplate{navigation symbols}{}

\usepackage[english]{babel}
\usepackage[latin1]{inputenc}
\usepackage{times}
\usepackage[T1]{fontenc}
\usepackage{hyperref}

% Setup TikZ
\usepackage{tikz}
\usetikzlibrary{arrows}
\tikzstyle{block}=[draw opacity=0.7,line width=1.4cm]
% Figure position
\usepackage[absolute,overlay]{textpos}
% math
\usepackage{mathabx}

\usefonttheme{professionalfonts}

% Author, Title, etc.
\title[hoge/fugaによる相補的な治療における高難度症例の治療と成績]
{hoge/fugaによる相補的な治療における高難度症例の治療と成績}
\author[taipapa]
{taipapa, 織田信長, 豊臣秀吉, 徳川家康}
\institute[hogefuga University]
{hogefuga大学大学院 hogefuga研究科　hogefuga分野}


\date[日本hogefuga外科学会 第??回学術総会　\hspace{2.4cm} 201X年X月XX日]
{\scriptsize{Symposium-02「とっーても難しいhogeとfuga」
\\ \vspace{0.15cm} 筆頭演者はhogefuga外科学会へ過去3年間のCOI自己申告を
完了しています．\\本演題の発表に関して開示すべきCOIはありません
}}

% 学会名，日付，スライド番号を挿入
\setbeamertemplate{footline}
{\color{gray} %
\hspace{.075cm}
\insertshortdate%
\hspace{4cm}
\insertframenumber{} / \inserttotalframenumber%
}

\begin{document}

\begin{frame}
\titlepage
\end{frame}

\section{Introduction}

\begin{frame}{背景と目的}
\begin{block}{}
\begin{itemize}
\item hogeとfugaを比較してみると，一方で難易度の高い症例で
も他方では容易に行える場合も多い.
\item 当施設では，一方に片寄ることなく，hogeとfugaを相補的に
用いることにより合併症の減少を目指す方針をとっている．
\item そこで，自験例から高難度のhogefuga症例についての
方針と成績を主にhogefuga surgeonの立場から検討した.
\end{itemize}
\end{block}
\end{frame}

\section{Results}
\begin{frame}
\frametitle{hogefuga症例の画像}
\centering
\includegraphics[width=3.5in]{hoge_fuga.pdf}
\end{frame}
\end{document}
```

-   ターミナルで，cdして上記のbeamer\_test.texのあるdirectoryに移動し，shellで以下のように打ち込む．前回のポスト（[LaTeXをインストールし，texファイルが変更されると，自動的にcompileしてskimでのpdfも自動で更新されるようにする](../latexmk)を参考　

    ```shell
    latexmk -pvc -pdf -view=none beamer_test.tex
    ```
-   これで下記のようなpdfが出来上がるはず．

    {{< figure src="/img/beamer_test.jpg" width="100%" target="_self" >}}

    {{< figure src="/img/beamer_test2.jpg" width="100%" target="_self" >}}

-   画像の貼り付けが必要なら，上の文書にもあるように必要な箇所で，

    ```shell
    \includegraphics[width=2in]{/Data/hoge/fuga/......./hoge_fuga.pdf}
    ```

    などと打てばよい．以下のようなスライドが得られる．

    {{< figure src="/img/beamer_test3.jpg" width="100%" target="_self" >}}

-   なにもしなければ，画像は左寄せになる．中央に寄せたければ，上記の文書内にあるように，\centering を使用する．

-   次回は，beamerで動画を走らせる件について書く予定．
