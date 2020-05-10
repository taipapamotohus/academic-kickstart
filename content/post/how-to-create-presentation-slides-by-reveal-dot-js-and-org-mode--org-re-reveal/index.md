+++
title = "How to create presentation slides by reveal.js and org-mode (org-reveal)"
author = ["taipapa"]
date = 2020-04-24
lastmod = 2020-05-10T13:04:31+09:00
tags = ["reveal", "js", "org-reveal", "emacs", "org-mode", "presentation", "slide", "javascript"]
type = "post"
draft = false
weight = 1
subtitle = "reveal.jsとorg-modeでプレゼン用スライドを作成する（org-reveal）（2020年5月9日修正）"
[image]
  placement = 3
  caption = "Trinity College Dublin"
+++

このブログを始めた頃にLaTeXを用いたスライド作成について一連の記事（[beamerでスライド原稿用pdfを作成する（その１）](../beamer)など）にまとめた．しかし，10年以上もこの方法を使ってきて，少々飽きてきたと言うのが正直な感想である．そんな時に気になっていたのが， **reveal.js** である．今回は，思いきってreveal.jsによるスライド作成に挑んでみたので，その顛末をまとめておく．私にはhtmlやjavascriptはさっぱりなので，例によってorg-modeを介しての作成となった．

<!--more-->

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [reveal.js](#reveal-dot-js)
    - [Installation of reveal.js](#installation-of-reveal-dot-js)
    - [Example Presentations](#example-presentations)
- [org-reveal](#org-reveal)
    - [References](#references)
    - [Installation of org-reveal](#installation-of-org-reveal)
    - [Configuration of org-reveal](#configuration-of-org-reveal)
- [Plugin of reveal.js](#plugin-of-reveal-dot-js)
    - [Default plugin (reveal.js/plugin/)](#default-plugin--reveal-dot-js-plugin)
    - [reveal.js-plugins](#reveal-dot-js-plugins)
    - [reveal.js-menu](#reveal-dot-js-menu)
    - [Plugin location](#plugin-location)
- [Creation of slides by org-reveal and reveal.js](#creation-of-slides-by-org-reveal-and-reveal-dot-js)
    - [Creation of main file](#creation-of-main-file)
    - [creation of custom css file](#creation-of-custom-css-file)
    - [Modification of main file](#modification-of-main-file)

</div>
<!--endtoc-->


## [reveal.js](https://github.com/hakimel/reveal.js) {#reveal-dot-js}

「HTMLを使って簡単に美しいプレゼンテーションを作成できるフレームワーク」だそうである．nested slides, Markdown support, PDF export, speaker notes, JavaScript APIなどの特徴を有する．[リンク先のデモ](https://revealjs.com/#/)を見れば，どんなことができるか一目瞭然である．これがなかなかカッコ良い．．．


### Installation of reveal.js {#installation-of-reveal-dot-js}

上述のご本家のサイトに詳細に記述されているとおりにすれば良い．

1.  まず，node.jsをインストールする．macであれば，homebrewを使うのが簡単である．ついでにnpmもインストールする．他のOSでは，各自ググっていただきたい．．．(^^;;;

    ```sh
    $ brew install node.js
    $ brew install npm
    ```
2.  スライドを作る場所として，適当なdirectoryを作成し，そこに移動して，reveal.jsのリポジトリをクローンする．ここでは，Slidesと言うdirectoryを作った．

    ```sh
    $ mkdir /Users/taipapa/Data/Slides
    $ cd /Users/taipapa/Data/Slides
    $ git clone https://github.com/hakimel/reveal.js.git
    ```
3.  reveal.js directoryに移動する

    ```sh
    $ cd reveal.js
    ```
4.  依存関係をインストールする

    ```sh
    $  npm install
    ```
5.  プレゼンテーションを開始する

    ```sh
      $ npm start

    > reveal.js@3.9.2 start /Users/taipapa/Data/Slides/reveal.js
    > grunt serve

    Running "connect:server" (connect) task
    Started connect web server on http://localhost:8000

    Running "watch" task
    Waiting...
    ```
6.  ブラウザで， <http://localhost:8000> が開き，プレゼンテーションが見られる．

ここまで非常に簡単である．defaultで開くプレゼンは素っ気ないものであるが，reveal.js directoryにあるdemo.htmlを以下のようにindex.htmlにすれば，上述のご本家のGitHub pageのリンクにあるデモと同じスライドが開くようになる．

```sh
$ cd /Users/taipapa/Data/Slides/reveal.js
$ ls
CONTRIBUTING.md     css                 js                   package.json
LICENSE             demo.html           lib                  plugin
README.md           gruntfile.js        node_modules         test
bower.json          index.html          package-lock.json
$ mv index.html index.html_original
$ cp demo.html index.html
$ ls
CONTRIBUTING.md     css                 index.html_original  package-lock.json
LICENSE             demo.html           js                   package.json
README.md           gruntfile.js        lib                  plugin
bower.json          index.html          node_modules         test
$ npm start
```

directory（フォルダ）の中身は以下のようになっている．

-   css/ : これがないとプロジェクトが機能しないコアのスタイル
-   js/ : 上記に同じだがJavascript用
-   plugin/ : reveal.jsの拡張用として開発されてきたコンポーネント
-   lib/ : 他のすべてのthird party assets (JavaScript, CSS, fonts)


### [Example Presentations](https://github.com/hakimel/reveal.js/wiki/Example-Presentations) {#example-presentations}

reveal.jsでどんなスライドを作れるのか実例を集めたページ．

-   [Sample scientific talk (on magnetism in small stars) by Peter Williams](https://www.cfa.harvard.edu/~pwilliam/htmltalk/#/)
-   [Autodesk Forge Viewer: Visual Reports with Connected Data by Philippe Leefsma](https://leefsmp.github.io/forge-connected-data/#/1)

などは興味深い．

reveal.jsの特徴として，Markdownでもスライドが作成できると言うことがあるが，私はいつも通りorg-modeを使用することにした．


## org-reveal {#org-reveal}


### References {#references}

-   [org-reveal](https://github.com/yjwen/org-reveal): 御本家
-   [Introduction to org-reveal](http://jr0cket.co.uk/slides/revealjs.html)：御本家のデモ
-   [【Org-mode】Org文書から reveal.js スライドを作成する](https://dev.classmethod.jp/articles/org-mode-re-reveal/)
-   [emacs-reveal](https://gitlab.com/oer/emacs-reveal)：御本家からのフォーク
-   [org-re-reveal](https://gitlab.com/oer/org-re-reveal)：御本家からのフォーク

htmlもJavascriptも知らなくてもブログが書けるように，これらを知らなくても，org-modeとreveal.jsを組み合わせて使えばスライドが簡単に作れる．reveal.js用のorg-modeのパッケージは，元来は， **org-reveal** ([org-reveal](https://github.com/yjwen/org-reveal), [Introduction to org-reveal](http://jr0cket.co.uk/slides/revealjs.html)) であり，そのforkである **org-re-reveal** も活発に開発されているとのことで，どちらをインストールするか迷ったのであるが，上記の御本家のGitHub pageを見ると，2018年6月ごろから作者のyiwenさんが再び盛んにcommitするようになっている．また，フォークのorg-re-revealはemacs-revealのバンドルの一部となっており，少し複雑である．以上の理由から，やはり，御本家を使うことにした．なお，フォークのorg-re-revealに関しては，[【Org-mode】Org文書から reveal.js スライドを作成する](https://dev.classmethod.jp/articles/org-mode-re-reveal/) に詳しく紹介されているので，参考にされたい．


### Installation of org-reveal {#installation-of-org-reveal}

御本家の[org-reveal](https://github.com/yjwen/org-reveal)に書いてある通りにすれば良いのだが，私は，いつものごとく，use-packageを用いて以下のようにinit.orgに書き込んでインストールした．

```lisp
#+begin_src emacs-lisp
(use-package ox-reveal
  :ensure t)
#+end_src
```


#### Installation of emacs-reveal {#installation-of-emacs-reveal}

前述したように，org-revealのフォークであるorg-re-revealはemacs-revealのバンドルの一部である．後述するが，reveal.jsのthemeや設定ファイルとして有用なものが含まれているので，emacs-revealもクローンしておいて必要に応じて使えるようにすることをお勧めする．個々に行うと面倒なので，全部突っ込むことにして，[Emacs-reveal for daily use](https://gitlab.com/oer/emacs-reveal#emacs-reveal-for-daily-use)の方法に従い，emacs-revealをクローンする．この際にrecursive optionをつけて再帰的にクローンすれば，org-re-reveal, org-re-reveal-ref, oer-revealなどが全部一緒に（161MB）入ってくる．

```sh
$ cd /Users/taipapa/Sources
$ git clone --recursive https://gitlab.com/oer/emacs-reveal.git
$ cd emacs-reveal
$ pwd
/Users/taipapa/Sources/emacs-reveal
$ ls
CHANGELOG.org           docker                  org-re-reveal-ref
CONTRIBUTING.md         emacs-reveal-submodules paper.bib
LICENSE                 emacs-reveal.el         paper.bib.license
LICENSES                oer-reveal              paper.md
Makefile                org-mode                paper.md.license
README.md               org-re-reveal           tests
```


### Configuration of org-reveal {#configuration-of-org-reveal}

設定に関しては， [org-reveal](https://github.com/yjwen/org-reveal)に詳細に記載されているが，重要なことを中心に抜粋しておく．


#### [Set the location of Reveal.js](https://github.com/yjwen/org-reveal#set-the-location-of-revealjs) {#set-the-location-of-reveal-dot-js}

org-revealはファイルの内容をexportする前にreveal.jsがどこにあるかを知っておく必要がある．reveal.jsの在り処というのは，reveal.jsのパッケージのトップディレクトリへのパスのことであり，そのディレクトリは **reveal.jsのファイルを含むディレクトリではなく，**  **README.mdを含むディレクトリ** のことである．

デフォルトでは，./reveal.js であるが，このままだと，全てのスライド原稿がreveal.jsと同じディレクトリにないといけなくなり不便である．そこで，init.orgにパスを書き込むか，あるいは，個々のorg fileの冒頭にrevieal.jsの在り処を書き込む．私は後者の方法をとっており，以下のように書き込んでいる．

```lisp
#+REVEAL_ROOT: file:///Users/taipapa/Data/Slides/reveal.js
```

なお，org-re-revealでは，このパスを書き込んでも読み込んでくれない．私の環境の問題なのかは追及していないが，これも御本家のorg-revealを使用する理由である．


#### [Select theme](https://github.com/yjwen/org-reveal#select-theme) {#select-theme}

テーマをREVEAL\_THEMEでセットする．テーマはdefaultで11個ぐらいついてくるが，ネットのあちこちにいろいろなテーマが落ちているので，あちこちのテーマを見て参考にし，自分なりに変更して好みのテーマを作れば良い．上述のemacs-orgの中のテーマ（oer-reveal.css）が割と良かったので，使用することにした．テーマを使用するには，reveal.jsのcss directoryの中のtheme directoryに入れれば良い．具体的には以下のようにした．

```sh
$ cd /Users/taipapa/Sources/emacs-reveal/oer-reveal/css
$ ls
dbis-longtitle.css             folder_inbox.png.license
dbis.css                       index.css
ercis-quote.css                oer-reveal.css
ercis.css                      outgoing-link.png
ercis2019.css                  outgoing-link.png.base
folder_inbox.png               outgoing-link.png.base.license
folder_inbox.png.base          outgoing-link.png.license
folder_inbox.png.base.license  toc-style.css
$ cp oer-reveal.css /Users/taipapa/Slides/reveal.js/css/theme/
```


## Plugin of reveal.js {#plugin-of-reveal-dot-js}

reveal.jsを素のままで使っても十分に有用ではあるが，やはり，pdfやパワーポイントではできないことをやれるようにしたいものである．そこで，プラグインの登場である．


### Default plugin ([reveal.js/plugin/](https://github.com/hakimel/reveal.js/tree/master/plugin)) {#default-plugin--reveal-dot-js-plugin}

デフォルトのプラグインについては，．[デモ](https://revealjs.com/#/)を見て貰えばどんな機能か分かると思う．例えば，notesは，発表者用の別フレームが表示され，その中に現在と次のスライドとタイマーが表示される．あるいは，zoomでは，option keyを押しながらクリックすると拡大し，もう一度同じことをすると元に戻る．


### [reveal.js-plugins](https://github.com/rajgoel/reveal.js-plugins) {#reveal-dot-js-plugins}

これはthird-partyのプラグインである．このサイトにはデモのリンクも貼ってあるので参考にしていただきたい．私は蛍光ペンをスライドショーの際中にリアルタイムで使いたかったので，chalkboardをインストールして設定した．


#### Installation {#installation}

リンク先に書いてある通りbowerを用いてインストールする．bower自体はbrewでインストールした．前述のchalkboardだけでなく，このサイトの全プラグインがインストールされる．

```sh
$ brew install bower
$ brew info bower
bower: stable 1.8.8 (bottled)
Package manager for the web
https://bower.io/
/usr/local/Cellar/bower/1.8.8 (5,413 files, 16.9MB) *
Poured from bottle on 2020-03-01 at 17:58:14
.....
$ bower install reveal.js-plugins
```

なお，次節のmenu-pluginはsubmoduleであり，別途インストールする必要がある．


### [reveal.js-menu](https://github.com/denehyg/reveal.js-menu) {#reveal-dot-js-menu}

各スライドのタイトルにジャンプできるようにするslide out menu プラグインである．また，テーマやスライド遷移を変更できる．この[live demo](https://denehyg.github.io/reveal.js-menu/#/home)を見ると分かりやすい．同じく，bowerでインストールする．


#### Installation {#installation}

```sh
$ bower install reveal.js-menu
```


### Plugin location {#plugin-location}

上述のthird partyのプラグインのインストールされる場所は以下の通りである．

```sh
$ cd /Users/taipapa/Data/Slides/reveal.js/bower_components
$ tree -L 2
.
├── reveal.js-menu
│   ├── CONTRIBUTING.md
│   ├── LICENSE
│   ├── README.md
│   ├── bower.json
│   ├── font-awesome
│   ├── menu.css
│   ├── menu.js
│   └── package.json
└── reveal.js-plugins
├── LICENSE
├── README.md
├── anything
├── audio-slideshow
├── bower.json
├── broadcast
├── chalkboard
├── chart
├── customcontrols
├── embed-tweet
├── fullscreen
├── mathsvg
├── package.json
└── spreadsheet
```

つまり，reveal.jsのbower\_components directoryに入る．

なお，default pluginはreveal.jsのすぐしたのplugin directoryに入る．


## Creation of slides by org-reveal and reveal.js {#creation-of-slides-by-org-reveal-and-reveal-dot-js}

以前の記事（[beamerでスライド原稿用pdfを作成する（その１）](../beamer)）と同じような内容のスライド原稿をorg-revealを用いて作成してみる．作成したものはまとめてGithub pageに置いておくので（[MySlides](https://github.com/taipapamotohus/MySlides)，live demoは[こちら](https://taipapamotohus.com/MySlides/)），クローンするか落とすかして中身を見て貰えば良いのだが，こちらでも順番に説明しておく．なお，前述のプラグインの中にはこのままでは動かないものもあるので，後述するように，一旦，exportしてからhtmlを修正する．（Github pageとlive demoは修正済みのものである）


### Creation of main file {#creation-of-main-file}

まず以下のファイルを作成する．簡単に解説をつけておく．

1.  REVEAL\_ROOTでreveal.jsを設定する．
2.  REVEAL\_HLEVELを999にすると遷移が全て横向きとなる．
3.  REVEAL\_TITLE\_SLIDEでタイトル（%t）と著者（%a）のヘッダーレベルを決める．
4.  REVEAL\_INIT\_OPTIONで各種のコントロールを行う．下図の設定を参照
5.  REVEAL\_THEMEでテーマを決める．前述のように，emacs-revealのバンドルから取ってきたoer-reveal.cssを使用した．
6.  REVEAL\_EXTRA\_CSSで追加のCSSを設定し，これで細かな調整を行う．
7.  REVEAL\_PREAMBLEで，フッターを指定した．

<!--listend-->

```lisp
#+REVEAL_ROOT: ./reveal.js # パスは各自の環境に合わせて書き換えていただきたい

#+REVEAL_HLEVEL: 999 # transitionsは全て横向き

#+REVEAL_TITLE_SLIDE: <h3>%t</h3><h5>%a</h5>

# スライド移動のコントローラーは右下に表示．スライド番号は，スライドの番号／全スライド数．
#+REVEAL_INIT_OPTIONS: width:1200, height:800, controlsLayout: 'bottom-right', slideNumber:"c/t", margin: 0, minScale:0.2, maxScale:2.5, transition: 'fade', menu: {side: 'left', titleSelector: 'h1, h2, h3, h4, h5, h6', hideMissingTitles: false, markers: true, custom: false, themes: true, transitions: true, openButton: true, openSlideNumber: false, keyboard: true, sticky: false, autoOpen: true}, chalkboard: {boardmarkerWidth: 8,	toggleChalkboardButton: { left: "80px" }, toggleNotesButton: { left: "130px"}}

#+REVEAL_THEME: oer-reveal

#+OPTIONS: num:nil toc:nil

#+REVEAL_EXTRA_CSS: ./custom_oer.css

# [[https://github.com/yjwen/org-reveal/issues/254][List of external plugins?#254]]
# #+REVEAL_EXTERNAL_PLUGINS: {src: '%sbower_components/reveal.js-menu/menu.js', async: true }
#+REVEAL_EXTERNAL_PLUGINS: {src: '%sbower_components/reveal.js-menu/menu.js'}


# #+REVEAL_EXTRA_CSS: https://maxcdn.bootstrapcdn.com/font-awesome/4.5.0/css/font-awesome.min.css

# フッター
#+REVEAL_PREAMBLE: <div class="footer"><p>日本hogefuga外科学会 第??回学術総会　202X年X月XX日 日本のどこか</p></div>

#+TITLE: hoge/fugaによる相補的な治療における高難度症例の治療と成績
#+Author: \\
#+Author: hogefuga大学大学院 hogefuga研究科　hogefuga分野　\\
#+Author: taipapa, 織田信長, 豊臣秀吉, 徳川家康
# #+DATE: 2020年3月22日


* 背景と目的
  - hogeとfugaを比較してみると，一方で難易度の高い症例でも他方では容易に行える場合も多い.
  - 当施設では，一方に片寄ることなく，hogeとfugaを相補的に用いることにより合併症の減少を目指す方針をとっている．
  - そこで，自験例から高難度のhogefuga症例についての方針と成績を主にhogefuga surgeonの立場から検討した.
  - 参照サイト：[[https://github.com/yjwen/org-reveal][Introduction to Org-Reveal]]

* Table
  #+ATTR_HTML: :width 25%

  |        | cTIA/SIE |  stable   | asymptomatic |  P   |
  |        | (n = 12) | (n = 122) |  (n = 186)   |      |
  |        |   <c>    |   <c4>    |     <c4>     | <c>  |
  |--------+----------+-----------+--------------+------|
  | stroke | 1 (8.3%) | 4 (3.3%)  |   5 (2.7%)   | 0.32 |
  | death  |    0     | 1 (0.8%)  |      0       | 0.42 |
  | MI     |    0     | 1 (0.8%)  |      0       | 0.42 |
  | Total  | 1 (8.3%) | 6 (4.9%)  |   5 (2.7%)   | 0.26 |


* hogefuga症例の画像
  [[./figures/hoge_fuga.jpg]]

* hogefuga症例の画像２
  #+ATTR_REVEAL: :frag roll-in
  遅延表示も可能
  #+ATTR_REVEAL: :frag roll-in
  #+ATTR_HTML: :height 600px
   [[./figures/hoge_fuga.jpg]]
* hogefuga症例の画像３
  #+ATTR_REVEAL: :frag roll-in
  - 遅延表示も可能
  #+ATTR_REVEAL: :frag roll-in
  - 画像の大きさを変更することも可能
  #+ATTR_REVEAL: :frag roll-in
  #+ATTR_HTML: :height 400px
  [[./figures/hoge_fuga.jpg]]

* ダブルコラムも可能
  #+REVEAL_HTML: <div class="column" style="float:left; width: 50%">
  Nullam eu ante vel est convallis dignissim.  Fusce suscipit, wisi nec facilisis facilisis, est dui fermentum leo, quis tempor ligula erat quis odio.  Nunc porta vulputate tellus.  Nunc rutrum turpis sed pede.  Sed bibendum.  Aliquam posuere.  Nunc aliquet, augue nec adipiscing interdum, lacus tellus malesuada massa, quis varius mi purus non odio.
  #+REVEAL_HTML: </div>
  #+REVEAL_HTML: <div class="column" style="float:right; width: 50%">
  #+CAPTION: Hoge-fuga (2020)
   [[./figures/hoge_fuga.jpg]]
  #+REVEAL_HTML: </div>

  #+ATTR_REVEAL: :frag roll-in
  キャプションも可能
* 動画もOK
  #+REVEAL_HTML: <video controls data-autoplay src="./Movies/Knight-33990.mp4"></video>

  #+CAPTION: Coronavirus
* 動画もOK（大きな画面）
  #+REVEAL_HTML: <video class="stretch" controls data-autoplay src="./Movies/Knight-33990.mp4"></video>
```

このファイルを，/Users/taipapa/Slides/reveal\_test.org として保存する．


### creation of custom css file {#creation-of-custom-css-file}

ついで，微調整のために以下に示すcssファイルを作成し，/Users/taipapa/Slides/custom\_oer.cssとして作成する．locationが内容と齟齬のないように注意する．試行錯誤の痕跡もそのままにしておく．このファイルでは，フォントの大きさや色などを指定している．

```html
/* http://nwidger.github.io/blog/post/making-a-reveal.js-presentation-with-org-reveal/ */
.reveal table th, .reveal table td {
/* text-align: center; */
border: 1px solid white;
}

/* https://stackoverflow.com/questions/4012872/how-to-center-a-footer-div-on-a-webpage/4012888 */
/* https://stackoverflow.com/questions/15629511/how-can-i-make-my-footer-center-to-the-bottom-of-the-page/15629635 */

/* div.footer { */
/*   /\* font-family: "Source Sans Pro", Helvetica, sans-serif; *\/ */
/*   font-size: 2vh; */
/*   color: gray; */
/*   position: fixed; */
/*   left: 400px; */
/*   bottom: 2px; */
/*   /\* z-index: 50; *\/ } */

div.footer {
/* font-family: "Source Sans Pro", Helvetica, sans-serif; */
font-size: 2vh;
color: gray;
position: fixed;
/* left: 400px; */
bottom: 1px;
width: 100%;
text-align: center;
/* margin-left:auto; */
/* margin-right:auto; */
/* margin-top:2em; */
/* z-index: 50; */ }

.reveal .slides section > section {  /*これによりフレームタイトルが上部に固定される．*/
/* height: 100%; */
height: 95%;
/* width:  100%; */
}

.reveal h3 {
font-size: 1.8em; }

.reveal h2 {
font-size: 2.0em; }

/* 段落内の行間の調整 */
/* https://github.com/yjwen/org-reveal/issues/38 */
/* .reveal p { */
/*   line-height: 3; */
/* } */
/* .reveal .org-ul { */
/*   line-height: 2; */
/* } */

/* リストの間のスペースの調整 */
/* https://github.com/yjwen/org-reveal/issues/38 */
.reveal li { margin: 0; padding: 0.5em}

/* Push each slide change to the browser history */
/* スライドの変更をブラウザの履歴に記録する */
Reveal.initialize({
history: true,
});
```

（うーむ，何故かhtml blockはsyntax highlightが効かない．．．まぁ，ここは後日に追求することにする．．．😅）

これでようやく準備が整った．reveal\_test.orgをhtmlファイルとしてexportするために， **C-c C-e** とすると，下図のような画面になる．

{{< figure src="/img/ox-reveal-export.jpg" width="100%" >}}

**R** が **Export to reveal.js HTML Presentation** になっており， **B** によりファイルに保存してブラウザで開くことになる，つまり，続けて **C-c C-e RB** と打てば，作成したスライドがブラウザーで開く．1200x800でwideに設定してあるので，横長の画面で見た方が見やすいと思う．

出来上がったスライドは，ほぼ期待通り動いてくれた．動画も問題なく動く．しかし，残念ながら，プラグインが問題であった．解決策を次節でまとめる．


### Modification of main file {#modification-of-main-file}

これでできたhtmlファイルのスライドをいじってみると，いくつか不具合があることが分かる．要するにプラグインのいくつかが動かないのである．

1.  スライドの左下にある3つのアイコンは，それぞれ，メニュー，黒板，notesのボタンである．
2.  メニューは問題なく動く．
3.  黒板も動く．ペンで書くことも，右クリックでwipeして書いたものを消すこともできる．
4.  しかし，delete keyで描いたものを一挙に消すことはできない．
5.  右端のnotesをクリックすると，カーソルがマーカーに変わり，グレーの蛍光ペンとして書くことはできる．しかし，"x"を押すことにより，グレー，青，赤，緑，オレンジ，紫，黄色と順番に色が変わっていく機能は効かない．また，右クリックでwipeして書いたものを消すことはできるが，delete keyで描いたものを一挙に消すことはできない．

いちばん使いたかった蛍光ペンがうまく動かないのである．色々と弄ってみたが，ox-revealのレベルではorg-modeをどう設定しても機能するようにはならず，結局，出来上がったhtmlファイルに直に以下のように追記することにした．

```sh
--- org-reveal_for_blog_oer-FINAL_v2.html	2020-04-20 21:08:18.000000000 +0900
+++ org-reveal_for_blog_oer-FINAL_v2-keyboard.html	2020-04-11 13:28:15.000000000 +0900
@@ -219,7 +219,16 @@
  { src: 'file:///Users/kohkichi/Data/Slides/reveal.js/plugin/markdown/marked.js', condition: function() { return !!document.querySelector( '[data-markdown]' ); } },
  { src: 'file:///Users/kohkichi/Data/Slides/reveal.js/plugin/markdown/markdown.js', condition: function() { return !!document.querySelector( '[data-markdown]' ); } },
  { src: 'file:///Users/kohkichi/Data/Slides/reveal.js/plugin/zoom-js/zoom.js', async: true, condition: function() { return !!document.body.classList; } },
- { src: 'file:///Users/kohkichi/Data/Slides/reveal.js/plugin/notes/notes.js', async: true, condition: function() { return !!document.body.classList; } }]
+    { src: 'file:///Users/kohkichi/Data/Slides/reveal.js/plugin/notes/notes.js', async: true, condition: function() { return !!document.body.classList; } }],
+    keyboard: {
+	    67: function() { RevealChalkboard.toggleNotesCanvas() },	// toggle notes canvas when 'c' is pressed
+	    66: function() { RevealChalkboard.toggleChalkboard() },	// toggle chalkboard when 'b' is pressed
+	    46: function() { RevealChalkboard.clear() },	// clear chalkboard when 'DEL' is pressed
+	     8: function() { RevealChalkboard.reset() },	// reset chalkboard data on current slide when 'BACKSPACE' is pressed
+	    68: function() { RevealChalkboard.download() },	// downlad recorded chalkboard drawing when 'd' is pressed
+	    88: function() { RevealChalkboard.colorNext() },	// cycle colors forward when 'x' is pressed
+	    89: function() { RevealChalkboard.colorPrev() },	// cycle colors backward when 'y' is pressed
+	},
 });
 </script>
 </body>
```

要するに，keyboard以下が追加部分である．chalkboardの設定を[reveal.js-plugins/chalkboard/](https://github.com/rajgoel/reveal.js-plugins/tree/master/chalkboard)の解説に従って追記した．これにより，上記1ー5で述べた問題は全て解消された．

下に， ~~Hugoのshortcodeを使って~~ **BEGIN\_EXPORT html** を使ってできあがったスライドそのものを埋め込んでみた．

```org
#+BEGIN_EXPORT html
<iframe src="https://taipapamotohus.github.io/MySlides/" width="1000" height="600" frameborder="0" allowfullscreen="allowfullscreen" allow="geolocation *; microphone *; camera *; midi *; encrypted-media *"></iframe>
#+END_EXPORT
```

このコードをox-hugoで書いた記事に入力すれば，下のように，スライドが埋め込まれる． **”f”** を叩けば，フルスクリーンになる．色々と弄って遊んでいただければ有り難い．

<iframe src="https://taipapamotohus.github.io/MySlides/" width="1000" height="600" frameborder="0" allowfullscreen="allowfullscreen" allow="geolocation *; microphone *; camera *; midi *; encrypted-media *"></iframe>

<br />  これで，やりたいことがほぼ出来るスライドの作成が可能になった．他にも色々な機能があるので，うまく動くようになれば，今後も報告していくつもりである．
