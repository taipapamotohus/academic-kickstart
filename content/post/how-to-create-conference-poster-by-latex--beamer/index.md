+++
title = "How to create conference posters by latex (beamer)"
author = ["taipapa"]
date = 2019-11-22
lastmod = 2019-12-01T15:21:50+09:00
tags = ["latex", "beamer", "conference", "poster", "emacs", "Inkscape", "tikz", "overleaf"]
type = "post"
draft = false
weight = 1
subtitle = "latex (beamer)による学会発表用のポスターの作り方"
[image]
  placement = 3
  caption = "Sfera con Sfera at Trinity College Dublin"
+++

学会発表用のスライド作成については，[beamerでスライド原稿用pdfを作成する（その１）](../beamer)に始まる一連の記事にまとめたが，ポスター作成については触れていなかったので，今回まとめることにする．

latexを使ってポスターを作成する場合の定番はbeamerを利用したもので，代表例には，beamerposterがある．それ以外にも，latexのポスター作成用のパッケージには，baposter, Gemini, betterposter, tikzposterなどがある．さらに，Inkscapeを使用する方法も報告されている．できるだけこれらの情報を集積してみた．なお，私自身は，標準的なbeamerposterの中に含まれているスタイルファイルを少し改変して用いている．

<!--more-->

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [General remarks](#general-remarks)
- [beamerposter](#beamerposter)
    - [References](#references)
    - [Installation](#installation)
    - [Usage](#usage)
    - [Templates](#templates)
- [Gemini](#gemini)
    - [References](#references)
    - [Installation](#installation)
    - [Usage](#usage)
- [baposter](#baposter)
    - [References](#references)
    - [Installation](#installation)
    - [Usage](#usage)
    - [Templates](#templates)
- [tikzposter](#tikzposter)
    - [References](#references)
    - [Installation](#installation)
    - [Usage](#usage)
    - [Templates](#templates)
- [betterposter](#betterposter)
    - [References](#references)
    - [Installation](#installation)
    - [Usage](#usage)
- [Inkscape](#inkscape)
    - [Making a Math Conference Poster with Inkscape](#making-a-math-conference-poster-with-inkscape)
    - [Creating a Science Conference Poster with Inkscape](#creating-a-science-conference-poster-with-inkscape)
- [まとめ](#まとめ)

</div>
<!--endtoc-->


## General remarks {#general-remarks}

-   [Creating Research Posters](https://guides.lib.wayne.edu/posters/latex)
-   [How to create posters using LaTeX](https://tex.stackexchange.com/questions/341/how-to-create-posters-using-latex)
-   [Gallery — Poster](https://ja.overleaf.com/gallery/tagged/poster)
-   [LaTeX Templates](https://www.latextemplates.com/cat/conference-posters)


## beamerposter {#beamerposter}

おそらく最もよく使われているパッケージ


### References {#references}

-   [beamerposter – Extend beamer and a0poster for custom sized posters](https://ctan.org/pkg/beamerposter)
-   [The LaTeX beamerposter package](https://github.com/deselaers/latex-beamerposter)
-   [Writing posters with beamerposter package in LATEX](http://tug.org/pracjourn/2012-1/shang/shang.pdf)
-   [LaTeX (Beamer)で学会発表用のポスターを作る](https://pecorarista.com/posts/poster.html)
-   [LaTeX(Beamer)でポスターを作る](http://muscle-keisuke.hatenablog.com/entry/2017/12/18/235617)


### Installation {#installation}

texlive (MacTeX)をインストールした時点で入っているので，改めてインストールし直す必要はない．


### Usage {#usage}


#### スタイルシートの変更 {#スタイルシートの変更}

まずスタイルファイルを使用する必要がある．私は，beamerposter packageの中のthemes directoryに入っている **beamerthemeI6dv.sty** を少し変更して **beamerthemeI6dv\_test.sty** として使用している．変更箇所は以下の通りである．これも覚書として残しておく．

```diff
--- beamerthemeI6dv.sty 2019-11-24 14:32:46.000000000 +0900
+++ beamerthemeI6dv_test.sty    2019-11-24 20:55:47.000000000 +0900
@@ -1,4 +1,4 @@
-\ProvidesPackage{beamerthemeI6dv} % this style was created by David Vilar
+\ProvidesPackage{beamerthemeI6dv_test} % this style was created by David Vilar

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\selectcolormodel{cmyk}
@@ -30,7 +30,7 @@

% footline colors and fonts
\setbeamercolor{footline}{fg=white,bg=i6colorschemeHeadline}
-\setbeamerfont{footline}{fg=white, size=\normalsize}
+%\setbeamerfont{footline}{fg=white, size=\normalsize}

% body colors and fonts
\setbeamercolor*{normal text}{fg=black,bg=i6colorscheme3}
@@ -42,7 +42,7 @@

% example environment
\setbeamercolor*{example title}{fg=white,bg=i6colorscheme1}
-\setbeamerfont{example title}{size=\large,series=\bf,bg=i6colorscheme1,fg=white}
+%\setbeamerfont{example title}{size=\large,series=\bf,bg=i6colorscheme1,fg=white}


%\setbeamercolor*{example body}{fg=white,bg=i6colorscheme4}
@@ -88,27 +88,29 @@
\begin{columns}[T]
\begin{column}{.02\paperwidth}
\end{column}
-      \begin{column}{.07\paperwidth}
+      \begin{column}{.1\paperwidth}
\begin{center}
-          %\includegraphics[width=.6\linewidth]{logos/i6-hks44}
+          % \includegraphics[width=.6\linewidth]{logos/i6-hks44}
+          \includegraphics[height=0.7\linewidth]{ISC2017-Logo.pdf}
\end{center}
\vskip1ex
\end{column}
-      \begin{column}{.675\paperwidth}
+      \begin{column}{.6\paperwidth}
\vskip4ex
\raggedleft
\usebeamercolor{title in headline}{\color{fg}\textbf{\LARGE{\inserttitle}}\\[1ex]}
\usebeamercolor{author in headline}{\color{fg}\large{\insertauthor}\\[1ex]}
\usebeamercolor{institute in headline}{\color{fg}\large{\insertinstitute}\\[1ex]}
\end{column}
-      \begin{column}{.25\paperwidth}
+      \begin{column}{.15\paperwidth}
\vskip1cm
\begin{center}
-          \includegraphics[width=.7\linewidth]{/u/figures/deselaers/logos/rwthaachenuniversity-whitegray}
+          % \includegraphics[width=.7\linewidth]{/u/figures/deselaers/logos/rwthaachenuniversity-whitegray}
+          \includegraphics[height=.45\linewidth]{Kobito.jpg}
\end{center}
\vskip1.5cm
\end{column}
-      \begin{column}{.03\paperwidth}
+      \begin{column}{.02\paperwidth}
\end{column}
\end{columns}
\end{beamercolorbox}
@@ -139,7 +141,7 @@
\end{beamercolorbox}

\begin{beamercolorbox}[ht=4ex,leftskip=1cm,rightskip=1cm]{footline}%
-    Lehrstuhl f\"ur Informatik 6 - Computer Science Department - RWTH Aachen University - Aachen, Germany \hfill Mail: \texttt{<surname>@cs.rwth-aachen.de} \hfill WWW: \texttt{http://www-i6.informatik.rwth-aachen.de}
+   Department of HogeFuga, HogeFuga University Graduate School of Medicine, Japan \hfill Mail: \texttt{taipapa@hoge-u.sc.jp} \hfill WWW: \texttt{http://www-hogefuga/TOP.html}
\vskip1ex
\end{beamercolorbox}
```

-   好みによって適当に変更すれば良い．
-   LogoをKobito.jpgに入れ替えているが，これはそれぞれの所属組織のものに変更すれば良い．
-   左側のLogoは学会のもの（ISC2017-Logo.pdf）に変更している．
-   一番最後はフッターに所属組織，メールアドレス，ウェブサイトなどを入れるための情報であり，そこも適宜置き換えれば良い．


#### スタイルシートの置き場 {#スタイルシートの置き場}

私のように，LaTeXをMacTeXでインストールした場合は，上記の変更したスタイルシートは， **~/Library/texmf/tex/latex/beamer/themes/theme/** に置く．これでLaTeXはこのスタイルシートを見つけてくれる．


#### Poster example {#poster-example}

上記スタイルシートを用いて作成すると下図のようなポスターが出来上がる．これは実際の学会発表に使用したものである．ただし，発表者や所属組織やロゴは変更してある．

{{< figure src="/img/Poster-test.jpg" width="100%" target="_self" >}}

このポスターのtex fileは以下のようになっている．元々は．どこかからとってきたtex fileを改変しているのだが，オリジナルをどこから持ってきたかは，もはや記憶の彼方である.....(^^;;;  （どなたかご指摘いただければ幸いである） 素人が色々弄ってなんとか形にしたものだが，これを上書きしていく事で割と簡単にポスターが作成できるようになった．試行錯誤の跡もそのまま残した状態でアップしておく．念のために書いておくが，このファイルをdocument.texとして保存し，[LaTeXをインストールし，texファイルが変更されると，自動的にcompileしてskimでのpdfも自動で更新されるようにする（2018年9月1日追記）](../latexmk) に書いたように， **latexmk -pvc -pdf -view=none document.tex** とすればコンパイルされてpdfが出来上がる．ただし，ファイルの中の画像（pdf）は適当なものと置き換えていただきたい．

-   9行目： **\usetheme{I6dv\_test}** によりスタイルシートを指定している．
-   45行目： **orientation=landscape** で横長， **orientation=portrait** で縦長になる．実際の大きさは， **width=200,height=100** で合わせる（幅 200cm 高さ 100 cmということである）．scaleの部分でも調整する．

<!--listend-->

{{< highlight tex "linenos=table, linenostart=1" >}}
% -*-coding:utf-8-*-
\documentclass[final,t]{beamer}
\mode<presentation>
{
  % \usetheme{Warsaw}
  % \usetheme{Aachen}
  % \usetheme{Oldi6}
  % \usetheme{I6td}
  \usetheme{I6dv_test}
  % \usetheme{I6pd}
  % \usetheme{I6pd2}
}
% additional settings
\setbeamerfont{itemize}{size=\normalsize}
\setbeamerfont{itemize/enumerate body}{size=\normalsize}
\setbeamerfont{itemize/enumerate subbody}{size=\normalsize}

% additional packages
% \usepackage{times}

%% 上記だけではsans serifにならないので，下記を設定　
%% http://www.tug.dk/FontCatalogue/ibmplexsansregular/
%% http://www.tug.dk/FontCatalogue/sansseriffonts.html
% \usepackage[T1]{fontenc}
% \usepackage[usefilenames,% Important for XeLaTeX
% DefaultFeatures={Ligatures=Common}]{plex-otf} %
% \renewcommand*\familydefault{\sfdefault} %% Only if the base font of the document is to be sans serif

%% http://www.tug.dk/FontCatalogue/ibmplexsanstext/
\usepackage[T1]{fontenc}
\usepackage[usefilenames,% Important for XeLaTeX
RMstyle={Text,Semibold},
SSstyle={Text,Semibold},
TTstyle={Text,Semibold},
DefaultFeatures={Ligatures=Common}]{plex-otf} %
\renewcommand*\familydefault{\sfdefault} %% Only if the base font of the document is to be sans serif

\usepackage{amsmath,amsthm, amssymb, latexsym}
\usepackage{exscale}
% \boldmath
\usepackage{booktabs, array}
% \usepackage{rotating} %sideways environment
\usepackage[english]{babel}
\usepackage[latin1]{inputenc}
\usepackage[orientation=landscape,size=custom,width=200,height=100,scale=1.725]{beamerposter}
\listfiles
\graphicspath{{figures/}}

% \usepackage{latexsym}

% Display a grid to help align images
% \beamertemplategridbackground[1cm]

\title{\huge Perioperative Assessment Of Cerebral Perfusion Territories Through Arterial Spin Labeling Magnetic Resonance Imaging In Carotid Stenosis}
\author[taipapa et al.]{taipapa, taimama, taison, taidaughter, other person1,
  other person2, other person3, other person 4, other person 5}
\institute[HogeFuga University]{Department of HogeFuga, HogeFuga Medical Center Japan}
\date[Feb. 23, 2017]{Feb. 17,  2017}

% abbreviations
\usepackage{xspace}
\makeatletter
\DeclareRobustCommand\onedot{\futurelet\@let@token\@onedot}
\def\@onedot{\ifx\@let@token.\else.\null\fi\xspace}
\def\eg{{e.g}\onedot} \def\Eg{{E.g}\onedot}
\def\ie{{i.e}\onedot} \def\Ie{{I.e}\onedot}
\def\cf{{c.f}\onedot} \def\Cf{{C.f}\onedot}
\def\etc{{etc}\onedot}
\def\vs{{vs}\onedot}
\def\wrt{w.r.t\onedot}
\def\dof{d.o.f\onedot}
\def\etal{{et al}\onedot}
\makeatother

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\begin{document}
\begin{frame}{}
  \begin{columns}[t]
    \begin{column}{.32\linewidth}

      %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
      \vspace*{-2.35cm}
      \begin{block}{Introduction}
        % \begin{columns}[t]
        %   \begin{column}{.75\linewidth}
        \begin{itemize}
          % \item Arterial spin labeling (ASL) is a magnetic resonance
          %   imaging (MRI) technique that uses the protons of arterial
          %   blood water molecules as endogenous tracers to evaluate
          %   CBF noninvasively.
        \item Territorial arterial spin labeling (TASL), a modified
          ASL technique, allows for the independent labeling of
          major individual feeding vessels and, consequently,
          visualization of their perfusion territories.
        \item Perfusion territories can change substantially during
          the perioperative periods of carotid endarterectomy (CEA)
          and carotid artery stenting (CAS). Perioperative changes
          in the perfusion territories of major arteries are
          important because these changes can influence the
          perioperative management of hyperperfusion and other
          conditions.

        \end{itemize}
        \vspace{0.5cm}
        \small{
          $\bigstar$ \textbf{Financial Disclosures:} This study was
          supported, in part, by a Grants-in-Aid for Scientific
          Research (C) KAKENHI Number 15K10302 from the Japan
          Society for the Promotion of Science.
        }
      \end{block}

      %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

      \begin{block}{Objective}
        The objective of this study was to use TASL to assess the
        relationships between perioperative changes in the perfusion
        territories of the ICA and the CBF increase after carotid
        revascularization in patients with carotid stenosis.
      \end{block}

      %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
      \begin{block}{Subjects and Methods}
        \begin{columns}[t]
          \begin{column}{.5\linewidth}
            \centering
            \includegraphics[width=1\linewidth]{Figures/ISC2017-subjects-left_Kohmura_opt.pdf}\\%[1ex]
          \end{column}
          \begin{column}{.5\linewidth}

            \centering
            \vspace{-44cm}
            \includegraphics[width=0.85\linewidth]{Figures/ISC2017-subjects-right_opt.pdf}\\[1ex]

            \begin{itemize}
            \item TASL images were fused with T1 weighed
              images(T1WI) on the software (Osirix) to match with
              anatomical images. Perfusion volume of each ICA was
              calculated from perfusion area and thickness of
              slices.
            \item Asymmetry Index (AI) was also calculated
              from perfused volume of ICA on each side.
              % \item The patients were divided into 2 groups on the
              %   basis of the preoperative perfusion volume of the
              %   stenotic ICA, that is, \textbf{normal PV} (n = 13)
              %   or \textbf{reduced PV} (n = 19) groups.
              % \item \textbf{Normal PV} was defined as $\geq$ -2SD of the
              %   healthy volunteer group, that is, 312.4 cm3.
              % \item \textbf{Reduced PV} was defined as $\textless$
              %   -2SD.
            \end{itemize}
          \end{column}
        \end{columns}
      \end{block}
    \end{column}

    %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

    \begin{column}{.32\linewidth}
      \vspace*{-2.35cm}
      \begin{block}{Results}
        \textbf{General postoperative changes in PV of ICA}\\
        \begin{columns}
          \begin{column}{.4\linewidth}
            \vspace*{-17cm}
            \begin{itemize}
            \item The preoperative perfusion volume (\textbf{PV}) of the
              stenotic ICA in the carotid stenosis group was significantly
              smaller than that in the control group. Revascularization
              increased the PV of the stenotic ICA and equalized the PV of
              the ICAs bilaterally.

            \end{itemize}
          \end{column}

          \begin{column}{.6\linewidth}
            \centering
            \includegraphics[width=1\linewidth]{Figures/Figure-2-WNS.pdf}\\
          \end{column}
        \end{columns}
        \vspace*{-0.5cm}
        \begin{itemize}
        \item The patients were divided into 2 groups on the basis of
          the preoperative PV of the stenotic ICA, that is,
          \textbf{normal PV} (n = 13) or \textbf{reduced PV} (n = 19)
          groups.
        \item \textbf{Normal PV} was defined as $\geq$ -2SD of the
          healthy volunteer group, that is, 312.4 cm3.
        \item \textbf{Reduced PV} was defined as $\textless$ -2SD.
        \end{itemize}

        \vspace{0.5cm}

        \textbf{Differential postoperative changes in PV \& CBF }

        \begin{columns}
          \begin{column}{.4\linewidth}
            \vspace*{-18cm}
            \begin{itemize}
            \item \textbf{A:} The changes in the PV of the stenotic ICA
              before and after CEA or CAS in the normal PV group and the
              reduced PV group. After treatment, the PV increases
              significantly in the two groups, while the postoperative
              increase was much larger in the reduced PV group.
            \end{itemize}
          \end{column}

          \begin{column}{.6\linewidth}
            \includegraphics[width=1\linewidth]{Figures/Figure-3-WNS.pdf}\\
          \end{column}
        \end{columns}

        \begin{itemize}
        \item \textbf{B:} The changes in ipsilateral CBF before and
          after CEA or CAS in the normal PV group and the reduced PV
          group. The postoperative increase in the ipsilateral CBF was
          larger in the reduced PV group than in the normal PV group.
        \end{itemize}

        \vspace{0.5cm}

        \textbf{Relationships between postoperative changes in PV \& CBF}

        \begin{columns}
          \begin{column}{.45\linewidth}
            \vspace*{-24cm}
            \begin{itemize}
            \item \textbf{A:} The rate of PV increase is significantly
              higher in the reduced PV group than in the normal PV
              group.
            \item \textbf{B:} The rate of CBF increase is significantly
              higher in the reduced PV group than in the normal PV
              group.
            \item \alert{Red circles} represent 4 patients whose
              postoperative increase in perfusion volume are less
              than lower quartile, and whose increase in CBF is more
              than upper quartile.
            \end{itemize}
          \end{column}

          \begin{column}{.55\linewidth}
            \vspace*{-1.75cm}
            \includegraphics[width=0.96\linewidth]{Figures/Figure-4-WNS.pdf}\\[2ex]

          \end{column}
        \end{columns}

      \end{block}

    \end{column}

    %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

    \begin{column}{.32\linewidth}
      \vspace*{-3cm}
      \begin{block}{}
        \textbf{Differential postoperative ICA flow changes}
        \begin{columns}[t]
          \begin{column}{.6\linewidth}
            \begin{itemize}
            \item The preoperative ICA flow was significantly lower in the
              reduced PV group than in the normal PV group.
            \item After treatment, the perfusion volume increases
              significantly in the two groups, while the postoperative
              increase was much larger in the reduced PV group.

            \end{itemize}
          \end{column}

          \begin{column}{.4\linewidth}

            \vspace*{-4cm}
            \centering
            \includegraphics[width=0.8\linewidth]{Figures/Figure-5-WNS.pdf}\\[2ex]

          \end{column}
        \end{columns}

        \textbf{Illustrative cases}
        \begin{itemize}
        \item \underline{Case 6 (normal PV group)}: PV markedly increased 1 day
          after CEA and was nearly unchanged between 1 and 7 days
          after CEA. ICA flow markedly increased after arterial
          reconstruction during CEA. Regional CBF was stable
          throughout the perioperative period.
        \item \underline{Case 8 (reduced PV group)}: PV 1 day after CEA was not
          significantly different from that before surgery, and then,
          it significantly increased between 1 and 7 days after CEA,
          resulting in equalization of the perfusion volumes of the
          left and right ICAs. ICA flow markedly increased after
          arterial reconstruction during CEA. Regional CBF increased
          significantly at 1 day after CEA (CBF increase $\geq$ 100\%),
          representing typical hyperperfusion. Regional CBF decreased
          significantly at 7 days after CEA compared with the first
          postoperative day and returned to its normal range.
        \end{itemize}

        % \vspace{1cm}
        \centering
        \includegraphics[width=0.8\linewidth]{Figures/UsualCase_legend_v2_opt.pdf}\\

      \end{block}

      \vspace*{-0.95cm}
      \begin{block}{Conclusion}
        \vspace*{-1cm}
        \begin{itemize}
        \item TASL demonstrated that CEA and CAS elicited increases in the
          PV of stenotic ICAs, which resulted in equalization of the
          PV of the left and right ICAs.
        \item Patients with reduced PV tended to increase the PV
          more markedly than patients with PV within normal ranges.
        \item In some patients with a reduced PV, the PV increased
          slightly, while the ICA flow markedly increased, which
          resulted in a large increase in CBF or hyperperfusion.
        \item These findings suggested that an imbalance between
          increases in the PV and ICA flow could play an important
          role in the pathophysiology of hyperperfusion.
        \end{itemize}
        \vspace{-1ex}
      \end{block}

      \vspace*{-0.5cm}
      \begin{block}{References}
        \vspace*{-0.75cm}
        \begin{enumerate}
          \scriptsize{
          \item Uchihashi et al. Clinical application of arterial
            spin-labeling MR imaging in patients with carotid
            stenosis: quantitative comparative study with
            single-photon emission CT. AJNR Am J Neuroradiol 32,
            1545--51, 2011
          \item Hartkamp et al. Mapping of cerebral
            perfusion territories using territorial arterial spin
            labeling: techniques and clinical application. NMR Biomed
            26, 901--12, 2013}
        \end{enumerate}
      \end{block}
     %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

    \end{column}
  \end{columns}
\end{frame}

\end{document}
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%% Local Variables:
%%% mode: latex
%%% TeX-PDF-mode: t
%%% TeX-master: t
%%% End:
{{< /highlight >}}


### Templates {#templates}

[Overleaf beamerposter templates](https://www.overleaf.com/latex/templates?addsearch=beamerposter) : Overleafにあるbeamposterのテンプレート．膨大な数のテンプレートがあるので，参考になる．


## Gemini {#gemini}

texliveには含まれていないbeamerの新しいポスター用テーマ


### References {#references}

-   [Gemini: A Modern LaTeX Poster Theme](https://www.anishathalye.com/2018/07/19/gemini-a-modern-beamerposter-theme/)
-   [Gemini is a modern LaTex beamerposter theme](https://github.com/anishathalye/gemini) : GitHub site


### Installation {#installation}

-   上記のGitHub siteをcloneするかdownloadすれば良い．
-   beamerposterに依存


### Usage {#usage}

-   GitHub siteに書いてある通り


## baposter {#baposter}

texliveには含まれていない．より新し目のパッケージ？


### References {#references}

-   [LaTeX Poster Template](http://www.brian-amberg.de/uni/poster/)  ご本家．ポスターの実例が数多く挙げられており，FAQもある．


### Installation {#installation}

上記サイトから **baposter.tar.bz2** をdownloadする．


### Usage {#usage}

上記サイトや， **[The baposter latex poster style](http://www.brian-amberg.de/uni/poster/baposter/baposter%5Fguide.pdf)** を参照


### Templates {#templates}

[Overleaf baposter templates](https://www.overleaf.com/latex/templates?addsearch=baposter) : Overleafにあるbaposterのテンプレート．


## tikzposter {#tikzposter}

「科学的ポスターを効率的に作成するLaTeXのクラス」である．


### References {#references}

-   [tikzposter – Create scientific posters using TikZ](https://ctan.org/pkg/tikzposter)   CTAN
-   [TikZposter](https://bitbucket.org/surmann/tikzposter/wiki/Home)    Wiki    ガイドをダウンロードできるリンクあり
-   [Elena Botoeva](https://www.doc.ic.ac.uk/~ebotoeva/fancytikzposter.html)   作者のページ


### Installation {#installation}

texliveをインストールすれば，一緒に入ってくる．


### Usage {#usage}

上記のガイドを参照．最も簡単な例は以下の通り．

{{< highlight tex "linenos=table, linenostart=1" >}}
\documentclass{tikzposter}

\title{Title}
\author{Author}
\institute{Institute}

\begin{document}

\maketitle

\block{Block}{Content}
\end{document}
{{< /highlight >}}


### Templates {#templates}

[Overleaf tikzposter templates](https://www.overleaf.com/latex/templates?addsearch=tikzposter) : Overleafにあるtikzposterのテンプレート．多数のテンプレートがあるので，参考になる．


## betterposter {#betterposter}

伝統的な学術ポスターに対する新たなスタイルのポスター．実にユニーク！

{{< figure src="/img/betterposter_example.png" width="80%" target="_self" >}}

要するに中核的なメッセージを簡潔な文章で特大に表示し，方法，結果，考察などは極端に簡潔にし，詳細はQR codeから論文を落として読んでもらうという，とんでもない発想のポスターである．


### References {#references}

-   [Better Scientific Poster](https://osf.io/ef53g/) : ご本家．元々はPowerPointのテンプレート
-   [How to create a better research poster in less time (including templates)](https://www.youtube.com/watch?v=1RwJbhkCA58&feature=youtu.be) : 作者自身のYouTube動画．面白い．必見！
-   [Better Poster Latex Template](https://github.com/rafaelbailo/betterposter-latex-template) : GitHub. LaTeXへの移植版．
-   [Mike Morrison](https://twitter.com/mikemorrison) : 作者のTwitter．多くの人が自分の"betterposter"をアップしているのが見られる．


### Installation {#installation}

上記の [Better Poster Latex Template](https://github.com/rafaelbailo/betterposter-latex-template) に詳細に書いてあるが，repositoryをclone するか，ダウンロードして，パスが通るところに置く．


### Usage {#usage}

上記の [Better Poster Latex Template](https://github.com/rafaelbailo/betterposter-latex-template) を参考．


## Inkscape {#inkscape}

これはオマケ．なぜかというと，ダントツでかっこいいから（笑）．


### [Making a Math Conference Poster with Inkscape](http://blog.felixbreuer.net/2010/10/24/poster.html?utm%5Fsource=share&utm%5Fmedium=ios%5Fapp&utm%5Fname=iossmf) {#making-a-math-conference-poster-with-inkscape}

これを見ると，羨望の念が．．．でも，Inkscapeを使えるようにならないと．．．

{{< figure src="/img/poster-fpsac2010.png" width="80%" target="_self" >}}


### [Creating a Science Conference Poster with Inkscape](https://dionhaefner.github.io/2015/07/creating-a-science-conference-poster-with-inkscape/) {#creating-a-science-conference-poster-with-inkscape}

こちらも同じく．．．

{{< figure src="/img/poster-lowres.png" width="80%" target="_self" >}}


## まとめ {#まとめ}

今回ポスターについてまとめてみて，改めて思ったのは，一旦自分のスタイルが出来上がると，万年一日のごとく，同じフォーマットで作成してしまい，金太郎飴のようになってしまうという事である．もちろん，内容が一番大事なのだが，如何に伝えるかという点でポスターのフォーマットは重要と思う．そういう意味では，[betterposter](#betterposter) や [Inkscape](#inkscape) によるポスターは非常に刺激的であった．特に，betterposterの手法は比較的模倣がしやすそうなので，次回は是非試みてみたい．
