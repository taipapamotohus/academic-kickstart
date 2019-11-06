+++
title = "How to use mark in Emacs (helm-all-mark-rings)"
author = ["taipapa"]
date = 2019-10-27
lastmod = 2019-11-06T22:09:05+09:00
tags = ["emacs", "mark", "ring", "helm"]
type = "post"
draft = false
weight = 1
subtitle = "Emacsにおけるマークの使い方"
[image]
  placement = 3
  caption = "Brain atlas of Witkowski, Dublin"
+++

Emacsのカーソルは画面をスクロールして上端または下端に至ると画面の中に表示される状態を維持するように動いてしまい，元の位置には止まらないのが仕様になっている．これは通常のエディターとは違っており，不便と感じて，色々と調べたり試したりしたのだが，要するに思想の違いであると考えるに至った．カーソルの位置は保持できない代わりにマークという方法が提供されている．まぁ，これが結構分かりにくいのだが，使ってみると案外便利である．そこで，マークについてまとめることにした．

<!--more-->

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [Mark](#mark)
    - [References](#references)
- [helm-all-mark-rings](#helm-all-mark-rings)

</div>
<!--endtoc-->


## Mark {#mark}


### References {#references}

-   [マークとリージョン](https://ayatakesi.github.io/emacs/26.1/html/Mark.html)  Emacsのヘルプ （C-h i で自分のヘルプを読むのが一番早いかも）

Emacsでは，テキストのある位置にマーク（mark）をセットすることができる．マークをセットするとマークとカーソルがあるポイントまでの領域はactiveになり，ハイライト表示される．例えば，下図では，5081行の先頭の「例えば」の例にカーソルを置いた状態で， **C-SPC** と打ってマークを置き，そのままアローキーでカーソルを5086行の文末まで移動させた状態であるが，マークを置いたところから現在のカーソルのある位置までがハイライトされている（activeになっている）．windowの表示範囲を越えるような広い範囲を選択する際に特に便利である．

{{< figure src="/img/mark-1.jpg" width="80%" target="_self" >}}

要するに，通常のエディターやワープロにおいて，マウスでクリックして押したままの状態でなぞった時と同じ状態である．この状態で，ハイライトされた領域をコピーしたり削除したりできる．まぁ，これだけだとどうと言うことはないのだが， **マークはmark ringに格納されるので，そこに戻ることができる．** これが便利である．この機能を利用するときはマークをセットしてactiveにしないほうが使いやすい．マークをセットした位置に戻るには **C-u C-SPC** と打つ．

以上をまとめると以下のようになる．

<style>.zebra-striping table { text-align: center;  }</style>

<div class="ox-hugo-table zebra-striping sane-table">
<div></div>

<div class="table-caption">
  <span class="table-number">Table 1</span>:
  Key bindings for mark in emacs
</div>

| Key binding | Description                                   |
|-------------|-----------------------------------------------|
| C-SPC       | マークをセットする．カーソルを移動させるとマークからカーソルの位置までがactiveになる |
| C-SPC C-SPC | マークをactiveにせずにマークをセットしてから、マークリングにpush（格納）する |
| C-u C-SPC   | マークがあった場所にカーソルを移動し、mark ringから1つ前のマークを復元する |

</div>

  要するに，あとで戻りたい位置にいるときに **C-SPC C-SPC** でactiveにせずにマークをつけておき，しばらく作業をしたのち戻りたくなったら， **C-u C-SPC** で戻ると言う使い方ができる．マークはmark ringに格納され，新しいものからリストの上に追加されていく． **C-u C-SPC**  を連打すれば，次々と古いマークの位置に移動できる．なお， **set-mark-command-repeat-pop** を非nilにセットすると、C-u C-SPCの後に続けて、C-u C-SPCではなく、C-SPCでマークリングを巡回できるようになる．これは， **M-x customize** から，Customize Aproposに入り，下図のようにset-mark-command-repeat-popをToggleでonにすれば良い．

{{< figure src="/img/mark-2.jpg" width="80%" target="_self" >}}

最古のマークまで行くと，また，最新のマークに戻ってくる．それで，mark ringなのであろう．これだけでも十分に便利であり，カーソルの位置を保持できないことを補って余りあるともいえる．さて，mark ringには，localとglobalの2種類があるが，これについては以下の解説を参考にしていただきたい．

{{% alert note %}}
**グローバルマークリング（マニュアルから引用）:** 各バッファーに属する普通のマークリングに加えて、Emacsにはグローバルマークリング(global mark ring)が1つあります。以前マークをセットしてからバッファーを切り替えた場合、マークをセットすると、マークはカレントバッファーのマークリングに加えて、グローバルマークリングにも記録されます。その結果、グローバルマークリングには訪れていたバッファーの系列が記録され、各バッファーではマークを設定した箇所が記録されます。グローバルマークリングの長さは、global-mark-ring-maxで制御され、デフォルトは16です。

コマンド **C-x C-SPC**  (pop-global-mark)は、グローバルリングの最新のバッファー位置にジャンプします。これもリングを巡回するので、連続してC-x C-SPCを使うことにより、古いバッファーのマーク位置に移動します。
   {{% /alert %}}

ここまでくると，「mark ringの内容が見られたらもっと便利だろうなぁ」と期待してしまうのは人情というものである．


## helm-all-mark-rings {#helm-all-mark-rings}

-   参照1：[Command: helm-all-mark-rings](https://tuhdo.github.io/helm-intro.html#orgheadline21)
-   参照2：[コマンド: helm-all-mark-rings](https://qiita.com/jabberwocky0139/items/a45cc82d9efd0cb6fd8e#コマンド-helm-all-mark-rings) 上記記事の翻訳

さて，そんな期待を叶えてくれるのが，helm-all-mark-ringsである．localとglobalの両方のmark ringsをフレンドリーなインターフェースで可視化して，以前にいたところにいつでも戻れるようにする．私の場合は，最初にpreludeをインストールしている（[Emacsの設定（その1）Preludeの導入](../prelude_install)）ので，helmは既にインストールされており，このコマンドはデフォルトでは，C-c h SPCにキーバインドされている．下図は，実際にC-c h SPCと打った時の画像である．下のバッファの中をarrow keyで上下し，見たい行のところでC-jと叩けば，上のバッファがそこへとぶ．そのまま次の行に動いてC-jとすれば，今度はそこに飛ぶ．Returnすれば，そのままそのバッファに行って確定する．

mark-ringはarrow keyで上下に動けるのだが，そのままglobal-mark-ringには移ってくれない．どうするかというと，minibufferのpattern: とあるところに，行きたいglobal-mark-ringの項目の最初の文字か2文字目ぐらいまでを入れれば，その項目だけが残るので，それを選択すれば良い．いつものhelmの選択方法である．

{{< figure src="/img/mark-3.jpg" width="80%" target="_self" >}}

今回は地味だ．．．．しかし，役に立つと思う．
