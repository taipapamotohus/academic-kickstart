+++
title = "How to add horizontal scrollbar for codeblock in academic theme of hugo"
author = ["taipapa"]
date = 2019-05-02
lastmod = 2019-05-04T20:28:26+09:00
tags = ["hugo", "blog", "codeblock", "horizontal", "scrollbar", "academic", "theme"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/Roma.jpg"
  caption = "Roma Termini"
+++

いやー，めでたい．ついに，TexLive 2019がreleaseされた．それにしても，TeXってこんなに人気があったのだ．世の中がみんなお祝いしてる，凄いぞ，TeX．．．．と思ったら，全然違った．．．これでまた年齢計算が複雑になる．システム担当者は大変である．まぁ，西暦を使用すればいいだけの話ではあるが．．．．．というわけで，今回はTeXの話，ではなくて，ブログのCodeBlockの長い行がwrapされるのは２行と間違うことがあるので，scrollbarをつけましょうという話である．

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [Wrap or scroll?](#wrap-or-scroll)
- [How to add horizontal scrollbar](#how-to-add-horizontal-scrollbar)
    - [Ref](#ref)
    - [academic theme version](#academic-theme-version)
    - [cssの変更](#cssの変更)

</div>
<!--endtoc-->


## Wrap or scroll? {#wrap-or-scroll}

以前の記事でも触れたように（[How to automatically embed R plot in blog created by Hugo via ox-hugo](../embedrplotinblog)），このサイトは，ox-hugoというorg-modeのパッケージで書いて，Hugoという静的サイトジェネレーター（themeはacademic）で作っているのだが，code blockの長い行がwrapされて，つまり，折り畳まれて表示されるようになっていた．Rのcodeで示してみると，こんな感じである．

{{< figure src="/img/wrap.jpg" width="100%" target="_self" >}}

これは３行のcodeであるが，はっきり言って見にくい．．．．．　

行番号を入れるという方法もあるが，それよりもscrollbarをつけて横にスライドさせる方が分かりやすい．つまり，こうである．

```R
boxplot(data_melt.df$Drug1 ~ data_melt.df$Day, xlab = "", main = "Drug1", xaxt="n", cex.axis=1.5, ylab="Relative value", cex.lab = 1.5, pch=16, cex = 1.5)
axis(1,at=c(1,2,3),formatC(c("Control", "Day1", "Day7")), cex.axis=1.5)
beeswarm(data_melt.df$Drug1 ~ data_melt.df$Day, pch = 16, ad = TRUE, cex=1.5, col=c("black", "green","blue"))
```

これなら，確実に１行を把握できる．

ここ（[Fix How Your Blog’s Code is Displayed on Mobile](http://seankross.com/notes/css-for-code/)）を見るともっと分かりやすく書いてある．


## How to add horizontal scrollbar {#how-to-add-horizontal-scrollbar}


### Ref {#ref}

-   [Fix How Your Blog’s Code is Displayed on Mobile](http://seankross.com/notes/css-for-code/)
-   [Prevent wrapping in code blocks #467](https://github.com/gcushen/hugo-academic/issues/467)
-   [added horizontal scrolling for code #1](https://github.com/ShanEllis/ShanEllis.github.io/pull/1/commits/35c0f3064d3ec2d7b6e35790448994bdb1233f79)

上記のサイトを参考に以下のようにscrollbarをセットアップすることにした．


### academic theme version {#academic-theme-version}

まず，Hugoのthemeであるacademic のversionを以下のように調べてみると，

```bash
less themes/academic/data/academic.toml
```

```text
# Academic

version = "2.4.0"
```

ふ，古い．latest versionは，4.2である．2018年8月に導入してから8ヶ月ほどで2.4から4.2までreleaseされており，開発スピードが並ではない．それは喜ぶべきことなのだが，問題は，私が全くそれについて行けてないことである．（ToT)

最新版ではdirectory構造もかなり変わっている．horizontal scrollbarをつけるのを機会に全部をupgradeしようとしてみたが，なかなかうまくいかない．[Customize style (CSS)](https://sourcethemes.com/academic/docs/customization/#customize-style-css) に書いてあるとおりにしようとしても，古いversionでは相当するdirectoryそのものが存在しない．


### cssの変更 {#cssの変更}

というわけで，academic themeのupgradeはサクッと諦めて姑息策を取ることにした． [Prevent wrapping in code blocks #467](https://github.com/gcushen/hugo-academic/issues/467) を参考にして，<br />
**/Data/hogeblog/fugablog/themes/academic/layouts/partials/css/academic.css**
を <br />
**/Data/hogeblog/fugablog/static/css/academic\_scrollbar.css**  <br />
として保存した．変更箇所は以下の通り

```sh
--- /Data/hogeblog/fugablog/themes/academic/layouts/partials/css/academic.css	2018-08-16 00:55:10.000000000 +0900
+++ /Data/hogeblog/fugablog/static/css/academic_scrollbar.css	2019-05-02 00:05:10.000000000 +0900
@@ -178,9 +178,16 @@
border-color: rgb(248, 248, 248);
 }

+/* pre code { */
+/*   white-space: pre; /\* Override Bootstrap to preserve line breaks in code. *\/ */
+/*   overflow-x: auto; */
+/* } */
+
+/* See http://seankross.com/notes/css-for-code/  */
 pre code {
-  white-space: pre; /* Override Bootstrap to preserve line breaks in code. */
-  overflow-x: auto;
+    overflow: auto;
+    word-wrap: normal;
+    white-space: pre;
 }

 hr {
```

これで，ox-hugoでの

{{< figure src="/img/wrap2.jpg" width="100%" target="_self" >}}

は，以下のように表示されるようになる．

```R
boxplot(data_melt.df$Drug1 ~ data_melt.df$Day, xlab = "", main = "Drug1", xaxt="n", cex.axis=1.5, ylab="Relative value", cex.lab = 1.5, pch=16, cex = 1.5)
axis(1,at=c(1,2,3),formatC(c("Control", "Day1", "Day7")), cex.axis=1.5)
beeswarm(data_melt.df$Drug1 ~ data_melt.df$Day, pch = 16, ad = TRUE, cex=1.5, col=c("black", "green","blue"))
```

-nをつけて行番号をつけると

{{< figure src="/img/wrap3.jpg" width="100%" target="_self" >}}

こうなる．

{{< highlight R "linenos=table, linenostart=1" >}}
boxplot(data_melt.df$Drug1 ~ data_melt.df$Day, xlab = "", main = "Drug1", xaxt="n", cex.axis=1.5, ylab="Relative value", cex.lab = 1.5, pch=16, cex = 1.5)
axis(1,at=c(1,2,3),formatC(c("Control", "Day1", "Day7")), cex.axis=1.5)
beeswarm(data_melt.df$Drug1 ~ data_melt.df$Day, pch = 16, ad = TRUE, cex=1.5, col=c("black", "green","blue"))
{{< /highlight >}}

<br />
   なんとか，これで，code blockにhorizontal scrollbarをつけることができた．次に時間ができたときにacademic themeをupgradeして追いつこう．．．いつになることやら．．．(^^;;;;;;
