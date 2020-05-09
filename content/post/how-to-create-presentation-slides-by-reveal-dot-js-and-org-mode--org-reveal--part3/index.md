+++
title = "How to embed presentation slides created by reveal.js and org-mode (org-reveal) in web page."
author = ["taipapa"]
date = 2020-05-09
lastmod = 2020-05-09T19:54:24+09:00
tags = ["reveal", "js", "org-reveal", "emacs", "org-mode", "presentation", "slide", "javascript", "github"]
type = "post"
draft = false
weight = 1
subtitle = "reveal.jsとorg-modeで作成したプレゼン用スライドをウェブページに埋め込む"
[image]
  placement = 3
  caption = "Molly Malone statue, Dublin"
+++

前回までの2回でプレゼンテーション用スライドをorg-revealを介してreveal.jsで作成する方法を説明してきた．今回は，作成したスライドをwebで公開したり，web pageにそのスライドを埋め込む方法を紹介する．

<!--more-->

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [References](#references)
- [How to publish a web page on GitHub](#how-to-publish-a-web-page-on-github)
- [How to embed presentation slides in web page](#how-to-embed-presentation-slides-in-web-page)

</div>
<!--endtoc-->


## References {#references}

-   [GitHubでWebページを公開する方法](https://qiita.com/Yuki%5FYamashina/items/5d8208c450195b65344c)
-   [【初心者向け】Github pagesでwebページを公開する方法](https://qiita.com/sota%5Fmikami/items/c6038cf13fd84b519a61)
-   [GitHubのmasterブランチをWebページとして公開する手順（GitHub Pages）](https://qiita.com/tonkotsuboy%5Fcom/items/f98667b89228b98bc096)

この2つのサイトを読めば，ここはもう読まなくても良いような．．．


## How to publish a web page on GitHub {#how-to-publish-a-web-page-on-github}

前々回の記事（[How to create presentation slides by reveal.js and org-mode (org-reveal)](../how-to-create-presentation-slides-by-reveal-dot-js-and-org-mode-org-re-reveal)）で[live demo](https://taipapamotohus.com/MySlides/)を作成したが，その際の手順を将来の自分のためにまとめておく．

1.  localでwebページを作る．

    ここでは，以下のようにdirectoryを作って行うこととする．

    ```sh
    $ cd /Users/taipapa/Data/
    $ mkdir Slides
    ```

    そして，このSlides directoryの中でweb pageを作成する．作成方法は，前々回の記事（[How to create presentation slides by reveal.js and org-mode (org-reveal)](../how-to-create-presentation-slides-by-reveal-dot-js-and-org-mode-org-re-reveal)）を参照．このdirectoryの中にreveal.jsをgit cloneしてnpm installも行っておく．プラグインなどもインストールするが，後で不要なファイルを削除し必要最小限に減らしてからGitHub pageにアップすれば良い．

2.  GitHub pageに新しいrepositoryを作成する．

    自分のGitHub pageに行き， **Repositories** tagをクリックして， **Repositories** ページに移動すると下図のようになるので，右端の **New** をクリックする．

    {{< figure src="/img/GitHub-1.jpg" width="100%" >}}

    **New** をクリックすると下図のようになるので，適当な名前を **Repository name** に 入力して新しいrepositoryを作成する．名前はなんでも良いが，ここではMySlidesにした．名前を入力したら，一番下の **Create repository** をクリックして作成する．

    {{< figure src="/img/GitHub-2.jpg" width="100%" >}}

3.  作成したGitHub pageのrepositoryをlocalにクローンする．

    ターミナルで以下のように行った．

    ```sh
    $ cd /Users/taipapa/Data/Slides
    $ git clone https://github.com/ユーザー名/リポジトリ名.git
    # 今回の場合だと以下のようになる
    $ git clone https://github.com/taipapamotohus/MySlides.git
    # クローンしたrepositoryのdirectoryに移動する
    $ cd MySlides
    $ pwd
    /Users/taipapa/Data/Slides/MySlides
    ```

4.  localで作成したウェブページをこのMySlides directoryにコピーする．

    この時に，reveal.jsの中身などを全てコピーする必要はなく，ウェブページができる最小限にすれば容量が節約できる．cssとか画像とか動画など必要なものは全てコピーする．今回は以下のような内容とした．そうそう，作成したhtmlファイルの名前を **index.html** に直すことを忘れないように注意する．

    ```sh
    $ pwd
    $ /Users/taipapa/Data/Slides/MySlides
    $ tree -L 3
    .
    ├── Movies
    │   └── Knight-33990.mp4
    ├── custom_oer.css
    ├── figures
    │   ├── hoge_fuga.jpg
    │   └── hoge_fuga.meta
    ├── index.html
    └── reveal.js
        ├── bower_components
        │   ├── reveal.js-menu
        │   └── reveal.js-plugins
        ├── css
        │   ├── print
        │   ├── reset.css
        │   ├── reveal.css
        │   ├── reveal.scss
        │   └── theme
        ├── js
        │   └── reveal.js
        ├── lib
        │   ├── css
        │   ├── font
        │   └── js
        └── plugin
            ├── highlight
            ├── markdown
            ├── math
            ├── multiplex
            ├── notes
            ├── notes-server
            ├── print-pdf
            ├── search
            └── zoom-js
    ```

5.  gitで，add, commit, push

    必要なファイルがMySlides directoryにコピーできたら以下のようにする．

    ```sh
    $ cd /Users/taipapa/Slides/MySlides
    $ git add .
    $ git commit -m "first commit"
    # pushしてGitHub pageのrepositoryに反映させる
    $ git push origin master
    ```

6.  GitHub pageを確認

    <https://github.com/taipapamotohus/MySlides> に行くとこんな感じになる．

    {{< figure src="/img/GitHub-3.jpg" width="100%" >}}

    私の下手な説明を読むよりも，このページをgit cloneするかzipを落として．内容を見て貰えば，一番わかりやすいかもしれない．

7.  上の画像の右端の **Settings** をクリックしてSettingsに移動する．下の方までスクリールして **GitHub Pages** に移動する．ここで下図のように， **Source** を **master branch** にセットする．

    {{< figure src="/img/GitHub-4.jpg" width="100%" >}}

    [GitHubのmasterブランチをWebページとして公開する手順（GitHub Pages）](https://qiita.com/tonkotsuboy%5Fcom/items/f98667b89228b98bc096)によれば，以前は，gh-pagesという別ブランチを作成して、そこにソースコードをプッシュする必要があったが，2016年8月18日以降は，master branchのみでweb pageを公開できるようになったとのことである．もちろん，不要なデータを公開したくない場合は，gh-pages branchを使用すれば良い（[ブランチ上の一部のデータのみをGitHub Pagesで公開する](https://qiita.com/TakuyaHara/items/031294b7a2e586b5c1a4)）．

    これで完成である．<https://taipapamotohus.com/MySlides/> をクリックして，ちゃんとスライドが見られることを確認する．


## How to embed presentation slides in web page {#how-to-embed-presentation-slides-in-web-page}

次の課題は，作成したウェブページを別のウェブページに埋め込むことである．私の場合はHugoで作成したブログにreveal.jsで作成したスライドを埋め込むということになる．前々回の記事（[How to create presentation slides by reveal.js and org-mode (org-reveal)](../how-to-create-presentation-slides-by-reveal-dot-js-and-org-mode-org-re-reveal)）では埋め込みについてはさらっと書いているだけだが，実は一番苦労したのはこの埋め込みであった． **iframe** を使えば良いのであろうということはなんとなく分かったのだが，どう使えば良いのかでかなり試行錯誤した．結局，HugoのForumで **shortcodes** を使えば良いという記事（[How to use iframe content correctly in a yaml file](https://discourse.gohugo.io/t/how-to-use-iframe-content-correctly-in-a-yaml-file/14360/3)）を見つけてなんとかなった．とは言え，紆余曲折を経てのことなので，以下にその過程を失敗も含めてまとめてみた．

1.  shortcode templateをHugoのroot directoryに作成する．具体的には/Users/taipapa/Data/MyBlog/Taipapablog/layouts/shortcodes/SlideInclusion.htmlとして作成する． **SlideInclusion.html** の中身は以下の通りである．

    ```html
    <iframe src="{{.Get 0}}" width="1000" height="600" frameborder="0" allowfullscreen="allowfullscreen" allow="geolocation *; microphone *; camera *; midi *; encrypted-media *"></iframe>
    ```

2.  投稿記事の内部でこのshortcodeを呼ぶ．

    投稿記事の内部で，埋め込みたい場所に以下のように記述すれば良い．

    ```html
    ｛{< SlideInclusion "https://taipapamotohus.github.io/MySlides/" >}}
    ```

    （最初の **"{"** はescapeできないので全角の **"｛"** にしていることに注意）

3.  上記の書き込みにより，以下のように作成したウェブページが埋め込まれる．

    私の場合は，org-modeのパッケージであるox-hugoを使用して書いているのだが，上記のコードをそのまま書いても埋め込まれることは埋め込まれる．

    {{< SlideInclusion "<https://taipapamotohus.github.io/MySlides/>" >}}

    <br />
    というわけでうまく埋め込まれ．．．ていない！この記事自体が埋め込まれてしまっている．これはこれで面白いが（笑）

4.  iframeを，ox-hugoの中に直接書き込む．

    shortcodeをox-hugoから呼ぶとうまく行かないので，以下のように，直接 **iframe** のコードをox-hugoのファイルに書き込むことにした．

    ```lisp
    #+BEGIN_EXPORT html
    <iframe src="https://taipapamotohus.github.io/MySlides/" width="1000" height="600" frameborder="0" allowfullscreen="allowfullscreen" allow="geolocation *; microphone *; camera *; midi *; encrypted-media *"></iframe>
    #+END_EXPORT
    ```

    <br />
    このコードをox-hugoで書いた記事に入力すれば，下のように，スライドが埋め込まれる．
    <br />

    <iframe src="https://taipapamotohus.github.io/MySlides/" width="1000" height="600" frameborder="0" allowfullscreen="allowfullscreen" allow="geolocation *; microphone *; camera *; midi *; encrypted-media *"></iframe>

    <br />
    前々回の記事（[How to create presentation slides by reveal.js and org-mode (org-reveal)](../how-to-create-presentation-slides-by-reveal-dot-js-and-org-mode-org-re-reveal)）も今回の内容に合わせて修正しておいた．

    reveal.jsに関しては，まだ色々と面白そうなことがある．D3.jsとの組みわせとか．．．いずれまとめることができたらと考えている．
