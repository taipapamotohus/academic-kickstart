+++
title = "How to create presentation slides by reveal.js and org-mode (org-reveal) Part2"
author = ["taipapa"]
date = 2020-05-05
lastmod = 2020-05-06T12:17:26+09:00
tags = ["reveal", "js", "org-reveal", "emacs", "org-mode", "presentation", "slide", "javascript"]
type = "post"
draft = false
weight = 1
subtitle = "reveal.jsとorg-modeでプレゼン用スライドを作成する（org-reveal）その2"
[image]
  placement = 3
  caption = "Testing coverage"
+++

## コロナ肺炎（COVID-19）について {#コロナ肺炎-covid-19-について}

COVID-19に対する緊急事態宣言が5月末まで延長されることになった．現状を鑑みればやむを得ない判断であろう．今回の会見で，ようやくPCR検査が少ないことを認めた．[山中伸弥による新型コロナウイルス情報発信](https://www.covid19-yamanaka.com/cont3/17.html)から辿れる[Testing coverage](https://ourworldindata.org/covid-testing#testing-coverage)に行けば，PCR検査の国別の比較の最新データを見ることができる．冒頭の写真はいつものお気楽な観光写真ではなく，このサイトからのグラフである．一目瞭然ではあるが，既にこれまでも報道されてきた通り，日本のPCR検査数は圧倒的に少ない．「途上国並み」と揶揄する報道もあったが，それでは多くの途上国に失礼になるような数字である．グラフは1000人あたりのPCR検査数を示しており，アイスランドの147.8人（人口の15%!）は別格として，いわゆる先進国は2桁以上である．トルコ，韓国も12ー13，台湾 2.71，ベトナム 2.68などに対して， **日本は1.46** である．

PCR検査が少ないことを認めたわけなので，今後は一刻も早く増やしてもらいたい．脳卒中，心筋梗塞，外傷などの救急治療や癌の手術などは延期することは困難であり，救急に携わる医療スタッフ，救急医，麻酔科医，外科医，手術室スタッフなどは高い感染リスクに晒されている．特に，全身麻酔の際に気管内挿管を行う麻酔科医はエアロゾルを直接浴びることになる．基本的に通常の手術の前でも，B型肝炎，C型肝炎，梅毒，HIVなどの感染症をルーチンに調べる．緊急手術か定期手術かにかかわらず，術前検査にCOVID-19のPCRを加えることは医療崩壊を防ぐためにも必須であろう．僅かではあるが，既にそうしている病院もある．

さて，今回はコロナ肺炎の話で始まってしまったが，ここからの内容は前回のreveal.jsによるスライド作成に関する追加である．弄っているうちに多少装飾もできるようになってきたので報告する．ほとんどは，Cascading Style Sheets (CSS)に関することになるので，分かってる人には不要な話かも．．．😅

<!--more-->

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [コロナ肺炎（COVID-19）について](#コロナ肺炎-covid-19-について)
- [How to add shadow and color to the text box](#how-to-add-shadow-and-color-to-the-text-box)
- [How to add shadow to the image](#how-to-add-shadow-to-the-image)
- [How to add color to table row](#how-to-add-color-to-table-row)

</div>
<!--endtoc-->


## How to add shadow and color to the text box {#how-to-add-shadow-and-color-to-the-text-box}

前回（[How to create presentation slides by reveal.js and org-mode (org-reveal)](../how-to-create-presentation-slides-by-reveal-dot-js-and-org-mode-org-re-reveal)）にデザインを変更するために **custom\_oer.css** を作成した．今回はこのcssにいくつか追記をしてスライドをさらにattractiveにしようという内容である．もはやreveal.jsそのものとはほとんど関係ない．．．．

まず，定番からやってみた．テキストの部分に影をつけて立体的にし四角の角を丸くし，背景色をラベンダーにしてみる．LaTeXのbeamerのblockからのパクリである．以下のコードを **custom\_oer.css** に追加する．

```sh
.shadowLavender{
  /*影を入れて角を丸くし背景色をラベンダーにする*/
  -webkit-box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.5);
  -moz-box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.5);
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.5);
  border-radius: 20px;
  padding-left: 15pt;
  background-color: rgba(230, 230, 250, 0.5);
}
```

padding-leftの部分は文字がテキストボックスの左端に寄りすぎるので，少し隙間が空くように調整している．その上で，前回（[How to create presentation slides by reveal.js and org-mode (org-reveal)](../how-to-create-presentation-slides-by-reveal-dot-js-and-org-mode-org-re-reveal)）に作成したメインファイルである **reveal\_test.org** の最初の部分である「背景と目的」を以下のように修正する． **#+REVEAL\_HTML:** を使うのがミソである．

```lisp

* 背景と目的
  #+REVEAL_HTML: <div class="shadowLavender">
  - hogeとfugaを比較してみると，一方で難易度の高い症例でも他方では容易に行える場合も多い.
  - 当施設では，一方に片寄ることなく，hogeとfugaを相補的に用いることにより合併症の減少を目指す方針をとっている．
  - そこで，自験例から高難度のhogefuga症例についての方針と成績を主にhogefuga surgeonの立場から検討した.
  - 参照サイト：[[https://github.com/yjwen/org-reveal][Introduction to Org-Reveal]]
  #+REVEAL_HTML: </div>
```

これを保存したのちに，C-c C-e RBとして確認すると下図のようになる．やはり多少装飾性を有する方がattractiveであろう．shadowの大きさや背景色はお好みに合わせて調整していただきたい．

{{< figure src="/img/CSS-1.jpg" width="100%" >}}


## How to add shadow to the image {#how-to-add-shadow-to-the-image}

次も定番である．画像に同様に影をつけてみる．先ほどと同様に，以下のコードを **custom\_oer.css** に追加する．

```sh
.image_carousel img {
    box-shadow: 0px 4px 8px 0 rgba(0, 0, 0, 0.5);
    -webkit-box-shadow: 0px 4px 8px 0 rgba(0, 0, 0, 0.5);
    -moz-box-shadow: 0px 4px 8px 0 rgba(0, 0, 0, 0.5);
}
```

ついで，メインファイルである **reveal\_test.org** の画像を含む部分を修正する．例えば，以下のようにする．「hogefuga症例の画像」の部分を以下のように修正する． **#+REVEAL\_HTML:** を使うのは同様である．

```lisp
* hogefuga症例の画像 shadow
  #+REVEAL_HTML: <div class="figure image_carousel"><img src="./figures/hoge_fuga.jpg" alt=""/></div>
```

これを保存したのちに，C-c C-e RBとして確認すると下図のようになる．

{{< figure src="/img/CSS-2.jpg" width="100%" >}}

画像に影をつけると，centering（中央寄せ）がなかなかうまくいかず苦労したが， **figure** と組み合わせることでなんとかなった．もっとうまい方法があるのだろうが，私にはこれが精一杯であった．


## How to add color to table row {#how-to-add-color-to-table-row}

次はテーブルの色つけである．これもbeamerを参考にほんのりラベンダーにしてみた．以下のコードを **custom\_oer.css** に追加する．タイトル行は濃い目に，奇数行は薄めに，偶数行は中間の透明度にしてみた．

```sh
/* テーブルのセルの色を変更 */
.reveal section table th {
  background-color: rgba(230, 230, 250, 1);
  font-weight: bold; }

.reveal section table tr:nth-child(even) {
  background-color:  rgba(230, 230, 250, 0.7); }

.reveal section table tr:nth-child(odd) {
  background-color:  rgba(230, 230, 250, 0.3); }
```

テーブルの色つけは，reveal\_test.orgをいじる必要はなく，cssを設定するだけで良い．以下のようになる．

{{< figure src="/img/CSS-3.jpg" width="100%" >}}

割合と良い感じになった．

こんなところであろうか．個人的には，LaTeXのbeamerでやれていたことはほぼできるようになり，beamerでは不可能なこともできるようになったので，かなり満足している．CSSに詳しい人なら，もっと色々できると思う．アドバイスなどコメントいただければ有り難い．
