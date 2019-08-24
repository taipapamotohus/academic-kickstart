+++
title = "How to update hugo and academic theme (Part 1)"
author = ["taipapa"]
date = 2019-08-18
lastmod = 2019-08-25T00:28:02+09:00
tags = ["hugo", "academic", "theme", "upgrade", "ox-hugo", "org-mode", "emacs"]
type = "post"
draft = false
weight = 1
subtitle = "Hugoとacademic テーマのアップデート"
[image]
  placement = 3
  caption = "Ancient greek pottery, Musée du Louvre"
+++

最近のニュースを見ていて思い出した言葉がある．

> "True patriotism hates injustice in its own land more than anywhere else.” ― Clarence Darrow
>
> "Patriotism is the last refuge of a scoundrel." ― Samuel Johnson
>
> "Violence is the last refuge of the incompetent."" ― Salvor Hardin ( Isaac Asimov)

3番目はオマケ

閑話休題，以前の記事（[How to add horizontal scrollbar for codeblock in academic theme of hugo](../horizontal_scrollbar)）で触れたように，このブログは，Hugoという静的サイトジェネレーター（themeはacademic）で作成している． <!--more--> 実際には，ox-hugoというemacsのorg-modeのパッケージを用いて書いて，それをhugoのmarkdownとしてexportしている．同記事内で，academic themeのupgradeが速すぎて全く追随できていないと書いた．記事はゴールデンウィークの5月4日に投稿しており，既に3ヶ月以上が経過している．この夏休みにようやくupdateすることができたので，後日のためにまとめておく．

本来なら，まずox-hugoを用いたhugoでのブログの作り方をまとめるべきであろうが，ネットを少し探せば，私のような素人よりはるかに詳しい方が懇切丁寧に解説しているサイトが山のように存在する．また，ブログ設定の一から十まで溯る気力もないので，順番が逆になるが，今回はアップグレードからということにした．素人がアップデートに困ってあれこれやったことの詳細なメモということで．．．(^^;;;

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [References](#references)
- [Hugoのupdate](#hugoのupdate)
- [ox-hugo update](#ox-hugo-update)
- [Academic theme update](#academic-theme-update)
- [Migrate Content](#migrate-content)
    - [Front matter](#front-matter)
    - [Featured image](#featured-image)
    - [Recent Postsのサマリが長すぎる！](#recent-postsのサマリが長すぎる)
    - [Recent Postsの画像表示のフォーマット変更](#recent-postsの画像表示のフォーマット変更)
    - [Alerts](#alerts)
    - [WARN ..... found no layout file for "CSS" for "home"](#warn-dot-dot-dot-dot-dot-found-no-layout-file-for-css-for-home)
- [追記](#追記)

</div>
<!--endtoc-->


## References {#references}

1.  [HUGO](https://gohugo.io)  ご本家
2.  [Academic](https://sourcethemes.com/academic/)  academic themeのご本家
3.  [Getting started with the Academic framework for Hugo](http://www.mit.edu/~k2smith/post/getting-started/)  academic themeのインストール
4.  [Update](https://sourcethemes.com/academic/docs/update/)  academic themeのupdateについて
5.  [ox-hugo](https://ox-hugo.scripter.co)  ox-hugoのご本家


## Hugoのupdate {#hugoのupdate}

hugoを最新版にする．

```sh
$ brew upgrade hugo
..........
$ hugo version
Hugo Static Site Generator v0.57.2/extended darwin/amd64 BuildDate: unknown

$ brew info hugo
hugo: stable 0.57.2 (bottled), HEAD
Configurable static site generator
https://gohugo.io/
/usr/local/Cellar/hugo/0.57.2 (41 files, 59.2MB) *
Poured from bottle on 2019-08-19 at 21:45:45
From: https://github.com/Homebrew/homebrew-core/blob/master/Formula/hugo.rb
==> Dependencies
Build: go ✘
==> Options
--HEAD
Install HEAD version
==> Caveats
Bash completion has been installed to:
/usr/local/etc/bash_completion.d
==> Analytics
install: 23,734 (30 days), 46,557 (90 days), 196,750 (365 days)
install_on_request: 22,865 (30 days), 45,011 (90 days), 186,183 (365 days)
build_error: 0 (30 days)
```


## ox-hugo update {#ox-hugo-update}

ox-hugoを最新版にする．

```lisp
M-x package-utils-upgrade-by-name
...........
ox-hugo
```

C-h C-l ox-hugo-pkg.elすると，

```lisp
(define-package "ox-hugo" "20190802.1755" "Hugo Markdown Back-End for Org Export Engine"
  '((emacs "24.4")
    (org "9.0"))
  :keywords
  '("org" "markdown" "docs")
  :url "https://ox-hugo.scripter.co")
;; Local Variables:
;; no-byte-compile: t
;; End:
```

最新版であることが確認できる．


## Academic theme update {#academic-theme-update}

[Update](https://sourcethemes.com/academic/docs/update/) のサイトには，以下の3つの場合でのupdateの方法が書かれている．

1.  If you installed Academic Kickstart
2.  If you installed by Git cloning hugo-academic
3.  If you installed from a ZIP

問題は，自分がどうやってインストールしたかを忘れている（！）ことだった.....(^^;;;  versionも 2.4.0とかなり古い． しかも，恐らくは，数種類の方法を重ねて試した結果が現在の状況と考えられるので，ドキュメントの通りにやってもうまくいくはずがない気がした．

{{% alert note %}}
 以下のhugo new citeを行ってから，academic をgit cloneする方法はお勧めしない．この方法では，git repositryの中にgit repositoryを埋め込むことになりエラーが出る．1のAcademic Kickstartを使う方法が良い．次回の記事（[How to update hugo and academic theme (Part2)](../how-to-update-hugo-and-academic-theme-part2)）を参考にしていただきたい．以下の記述は自戒のために残しておく．
{{% /alert %}}
~~色々悩んだ末に，2の方法をいちから，やり直すことにした．要するにクリーンインストールを行い，その上に，現在までの内容を流し込むという泥臭いやり方を選択したわけである．~~

~~以下，[Getting started with the Academic framework for Hugo](http://www.mit.edu/~k2smith/post/getting-started/) に沿って，まず，hugoで新しいサイトを作成し，そこにacademic themeをgitでインストールし，最新版の4.4.0になっていることを確認する．~~

```sh
$ cd /Data/hoge
$ hugo new site Taipapablog
$ cd Taipapablog
$ pwd
/Data/hoge/Taipapablog
$ git clone https://github.com/gcushen/hugo-academic.git themes/academic
$ less themes/academic/data/academic.toml
# Academic

version = "4.4.0"
themes/academic/data/academic.toml (END)
```

~~そして，academicのexampleSite folder の中身を全てwebsite root folder（今回はTaipapablog directory）にコピーする．これらは，config fileなどを含んでおり，自分のウェブサイトの鋳型になる．~~

```sh
$ pwd
/Data/hoge/Taipapablog
$ ls themes/academic/exampleSite/
config/  content/ static/
$ cp -av themes/academic/exampleSite/* .
```

~~ここで，Hugo serverをwebsite root folderから立ち上げる．~~

```sh
$ pwd
/Data/hoge/Taipapablog
$ hugo server --watch
```

~~これで，ブラウザで，url windowにlocalhost:1313と打てば，Academic powered websiteのデモが立ち上がる．ここまでは実に簡単である．~~


## [Migrate Content](https://sourcethemes.com/academic/docs/update/#migrate-content) {#migrate-content}

ここから以前の記事の内容を流し込んでいくわけであるが，academicの新旧versionの間には断絶があって，単純に流し込んで終わりというわけにはいかない．これも，上書きアップデート（？）のようなやり方を諦めた理由である．この断絶を **"Breaking changes"** と呼んでいる．これを乗り越えてブログの内容を移行するための[academic-scripts](https://github.com/sourcethemes/academic-scripts) のリンクが上記ページにある．これは，hugoの新しい機能である[Page Bundles](https://gohugo.io/content-management/page-bundles/) への移行を自動化するスクリプトである．簡単に言えば，Page Bundlesとは，[Page Resources](https://gohugo.io/content-management/page-resources/) をグループ化する方法であり，詳細は，リンク先を参照されたい．Page Bundlesは，下記のような構造になる．つまり，ファイルの構成が変わってmdファイルひとつだったのが，フォルダ（ディレクトリ）となり，mdファイルの名前はindex.mdとなり，かつ，featured.jpgが自動的にページの冒頭に置かれるようになる．同一記事に使用する画像などは，同じディレクトリ内にまとめることができる．以下は，アップデート前後の /Data/hoge/Taipapablog/content/post の比較である．

```sh
Old version
$ tree -L 2
.
├── Different-segment-to-each-facet-in-ggplot.md
├── Emacs_Install.md
├── EmbedRplotInBlog.md
├── ExportRplot.md
├── FullTextSearch.md
├── Japanese_setup.md
..........

Current version
$ tree -L 2
.
├── Different-segment-to-each-facet-in-ggplot
│   ├── featured.jpg
│   └── index.md
├── Emacs_Install
│   ├── featured.jpg
│   └── index.md
├── EmbedRplotInBlog
│   ├── featured.jpg
│   └── index.md
├── ExportRplot
│   ├── featured.jpg
│   └── index.md
├── FullTextSearch
│   ├── featured.jpg
│   └── index.md
├── Japanese_setup
│   ├── featured.jpg
│   └── index.md
..........
```

先ほどのスクリプトは，このフォルダ構成への移行を自動化してくれる．これをgitでクローンする．

```sh
$ cd /Data/hoge
$ ls
Taipapablog
$ git clone https://github.com/sourcethemes/academic-scripts.git
Cloning into 'academic-scripts'...
remote: Enumerating objects: 27, done.
remote: Total 27 (delta 0), reused 0 (delta 0), pack-reused 27
Unpacking objects: 100% (27/27), done.
$ ls
Taipapablog
academic-scripts
```

このacademic-scriptsの中身を見ると，

```sh
$ cd academic-scripts/
$ ls
LICENSE.md
README.md
refactor-homepage-sections-to-bundles.sh*
refactor-pages-to-page-bundles.sh*
refactor_page_bundles_to_pages.sh*
$ less refactor-pages-to-page-bundles.sh
#!/bin/sh

# Helps migrate from v2.4.0 to v3.0.0
#
# Refactor a page named `X.md` to `content/<section>/X/index.md` to use the
# new page bundles and featured image system
#
..........
$ less refactor-homepage-sections-to-bundles.sh
#!/usr/bin/env bash

# Helps migrate from v4.1 to v4.2
#
# Refactors homepage sections named `content/home/X.md` to `content/home/X/index.md`,
# treating homepage sections as headless page bundles in Hugo.
#
# - E.g. an About section named `content/home/about.md` is converted to `content/home/about/index.md`
..........
```

という具合に，確かに，pageからpage bundlesへの移行を自動でやってくれるようになっている．そこで，これまでのブログをTaipapablog\_OLDとして保存し，そのポスト（投稿記事）にこのスクリプトを適用する．その後，これらの記事を全て新しい方のTaipapablogのpostにコピーする．

```sh
$ cd /Data/hoge
$ ls
Taipapablog
Taipapablog_OLD
academic-scripts
$ cd Taipapablog_OLD/
$ pwd
/Data/hoge/Taipapablog_OLD
$ ../academic-scripts/refactor-pages-to-page-bundles.sh
./content/posts/annotation.md -> ./content/posts/annotation/index.md
./content/post/org-html-export-theme.md -> ./content/post/org-html-export-theme/index.md
./content/post/org-mode_paper_2.md -> ./content/post/org-mode_paper_2/index.md
..........
$ cp -a content/post/* ../Taipapablog/content/post/
```

以降の作業は全て，新しい方のTaipapablog directoryで行う．こちらのcontent/home/にも先ほどのスクリプトのホームページセクション用のものを適用しておいた．こちらは不要かもしれない．

```sh
$ ../academic-scripts/refactor-homepage-sections-to-bundles.sh
./content/home/search.md -> ./content/home/search/index.md
./content/home/hero_carousel.md -> ./content/home/hero_carousel/index.md
./content/home/hero.md -> ./content/home/hero/index.md
```

これにより，/Data/hoge/Taipapablog/content/posts/\*.mdや/Data/hoge/Taipapablog/content/home/.mdが，先ほどtreeで示したようなディレクトリ構造になる．

ここから先は，config/, content/, content/home/, content/post/の中身を弄って，アップデートする前と同じになるように修正していく．私は，blogをhugoのmarkdown自体はほとんど弄ることなく，org-modeのパッケージであるox-hugoで書いているので，そちらを中心に述べる．


### Front matter {#front-matter}

-   content/home/には，demo, experienceなど多くのwidgetが入っているが，ほとんど使用してないので，それぞれのindex.mdのfront matterの最初の方にあるactive = trueを **active = false** にする．
-   上記のスクリプトを適用した際に，headless = true  # This file represents a page section. が二重になることがあったので，余分な部分は全て削除した．
-   新規ポストを投稿する際に，page bundlesの形式になるようにするために，propertiesに **EXPORT\_HUGO\_BUNDLE** を使用する．詳細は次節で述べる．
-   **subtitle** を使用するのが可能となった．もしかして以前から？　以下のように，ox-hugoでpropertiesに追加すれば良い．

    ```lisp
    :EXPORT_HUGO_CUSTOM_FRONT_MATTER: :subtitle "Hugoとacademic テーマのアップデート"
    ```


### [Featured image](https://sourcethemes.com/academic/docs/managing-content/#featured-image) {#featured-image}

-   各ポストの冒頭に掲げていた画像は，アップデート前は明示的に場所と名前を指示しないといけなかったが，アップデート後はPage Bundlesとなり，同じフォルダにfeatured.jpgとして置いておけば，自動的にその記事の冒頭に表示されるようになる．各画像の移動が面倒であったが，今後は楽になりそう．．．(^^
-   ox-hugoから，Page Bundlesとしてexportするためには，propertiesに **EXPORT\_HUGO\_BUNDLE** を使って以下のように書けば，Front Matterとしてexportされる．2個目の項目追加からは，\*EXPORT\_HUGO\_BUNDLE+\* とする．

    ```lisp
    :PROPERTIES:
    :EXPORT_HUGO_BUNDLE: how-to-upgrade-hugo-and-academic-theme
    :EXPORT_FILE_NAME: index
    :EXPORT_HUGO_CUSTOM_FRONT_MATTER: :subtitle "Hugoとacademic テーマのアップデート"
    :EXPORT_HUGO_CUSTOM_FRONT_MATTER+: :image '((placement . 3) (caption . "Ancient greek pottery, Musée du Louvre"))
    ```
-   Featured imageをどのような大きさで表示するかについては，上述のリンク先に説明がある．

    ```sh
    # Featured image
    # To use, place an image named `featured.jpg/png` in your page's folder.
    # Placement options: 1 = Full column width, 2 = Out-set, 3 = Screen-width
    # Focal point options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
    # Set `preview_only` to `true` to just use the image for thumbnails.
    ```

    とりあえずは，旧記事に合わせて，スクリーン幅となるplacement = 3とした．ox-hugoでのpropertiesで上述のように指定する．captionのつけ方も上記の通り．


### Recent Postsのサマリが長すぎる！ {#recent-postsのサマリが長すぎる}

Home ページの下方には，Recent Postsがあり，最近の記事のサマリが画像とともに表示される．この表示形式は以下のように5種類が選べる（[View](https://sourcethemes.com/academic/docs/page-builder/#view)）．

```sh
Several widgets have a view option to let you choose the layout of the widget. The following layouts are available:

1 = List (previously Simple)
2 = Compact (previously Stream)
3 = Card (previously Detailed)
4 = Citation (previously APA and MLA), only available for publications
Optionally, edit the value of citation_style in params.toml to APA or MLA
5 = Showcase (large images), only available for projects
```

この中から，以前のversionに一番近い 3，つまり，カード形式を選択した．しかし，画像のcropがイマイチで，しかもサマリがサマリと言えないくらいに長い．updateする前のフォーマットが気に入っていたので，この変化は残念．[Create a blog post](https://sourcethemes.com/academic/docs/managing-content/#create-a-blog-post) に記載されているように， **&lt;!--more-->** を記事本文の適当なところに挿入して，サマリの長さを調整することにした．


### Recent Postsの画像表示のフォーマット変更 {#recent-postsの画像表示のフォーマット変更}


#### References {#references}

-   [Crop Less with Focal Point](https://discourse.gohugo.io/t/crop-less-with-focal-point/15387)
-   [Image Processing](https://gohugo.io/content-management/image-processing/)
-   [Partial Templates](https://gohugo.io/templates/partials/)
-   [Hugoのテンプレート構文「template」「partial」「block」「define」のわかりやすい解説](https://ottan.xyz/hugo-template-partial-define-block-20190101/)

上述のfeatured imageを記事のheader imageとして表示する際のサイズやクロップのやり方が気に入らないので，変更することにした．cardでacademic themeに全文検索をかけると， **li\_card.html** というファイルが見つかり，どうやら，これが，カード形式のテンプレート（[Partial Templates](https://gohugo.io/templates/partials/)）と推察され，これを変更すれば良いと気がついた．

1.  _Data/hoge/Taipapablog/layouts_ に **partials** directoryを作成し，/Data/hoge/Taipapablog/themes/academic/layouts/partials/ **li\_card.html** をコピーする．

2.  Hugoは以下の優先順位で読み込まれる．（[Partial Template Lookup Order](https://gohugo.io/templates/partials/#partial-template-lookup-order)）

    1.  layouts/partials/\*<PARTIALNAME>.html
    2.  themes/<THEME>/layouts/partials/\*<PARTIALNAME>.html

    したがって，1にコピーしたli\_card.htmlを弄れば，それが優先されることになる．

3.  以下のように，li\_card.htmlの53行目を変更する．

    ```sh
    .....
    52 {{ with $resource }}
    53 {{ $image := .Fill (printf "918x517 q90 %s" $anchor) }}
    54 <a href="{{ $item.RelPermalink }}">
    .....

    ----->
    .....
    53 {{ $image := .Resize "900x"  }}
    .....
    ```

    これは， [Image Processing](https://gohugo.io/content-management/image-processing/) に解説されているように，FillをResizeに変更しただけであるが，ほぼ望み通りの画像表示となった．


### [Alerts](https://sourcethemes.com/academic/docs/writing-markdown-latex/#alerts) {#alerts}

-   これは今回見つけた新たな小道具
-   ノート，ヒント，警告などに有用．
-   いくつか方法はあるが，shortcodeを使うのが一番簡単．\\
    ｛{% alert note %}}\\
      A Markdown aside is useful for displaying notices, hints, or definitions to your readers.\\
      ｛{% alert  %}}\\
    により，（最初の **"{"** はescapeできないので全角の **"｛"** にしていることに注意）

{{% alert note %}}
 A Markdown aside is useful for displaying notices, hints, or definitions to your readers.
 {{% /alert %}}
 となる．

｛{% alert warning %}}\\
 A Markdown aside is useful for displaying notices, hints, or definitions to your readers.\\
 ｛{% /alert %}}\\
 は，以下のようになる．（最初の **"{"** はescapeできないので全角の **"｛"** にしていることに注意）\\
 {{% alert warning %}}
 A Markdown aside is useful for displaying notices, hints, or definitions to your readers.
 {{% /alert %}}
 また，使ってみよう．


### WARN ..... found no layout file for "CSS" for "home" {#warn-dot-dot-dot-dot-dot-found-no-layout-file-for-css-for-home}

アップデートしていじっているうちに，
  {{% alert warning %}}
WARN 2019/08/23 00:36:28 found no layout file for "CSS" for "home": You should create a template file which matches Hugo Layouts Lookup Rules for this combination.
  {{% /alert %}}
のような警告が出るようになった（早速使ってみた）．サイト自体のビルドはできて，実害はないが，気持ちが悪いので，ネットを探ると，academicの作者のGitHub pageに，そのものズバリの [Featurette widget does not change from example #1060](https://github.com/gcushen/hugo-academic/issues/1060) があった．

> You have updated to the unreleased version. You'll need to remove the "CSS" output entry from config.Toml

ということなので，以下のように作業した．

```sh
$ vi /Data/hoge/Taipapablog/config/_default/config.toml
.....
45 [outputs]
46   home = [ "HTML", "RSS", "JSON", "WebAppManifest", "CSS"]
47   section = [ "HTML", "RSS" ]
.....
----->
.....
45 [outputs]
46 #  home = [ "HTML", "RSS", "JSON", "WebAppManifest", "CSS"]
47   home = [ "HTML", "RSS", "JSON", "WebAppManifest"]
48   section = [ "HTML", "RSS" ]
.....
```

46行目の"CSS"を削除するだけで，警告が出なくなった．(^o^)

見え方にまだまだ不満はあるが，こんなところだろうか．アップデートしてから，貼り付けた画像の上にマウスを持っていくと，拡大鏡のアイコンになり，クリックすると2段階で拡大されるようになった．他にも変化はあるようだが，全然活用できていない．ぼちぼち触っていって，面白いことがあれば，また，まとめてみよう．．．


## 追記 {#追記}

githubにdeployした時に，以下のようなエラーが生じた．

```lisp
warning: adding embedded git repository: themes/academic
hint: You've added another git repository inside your current repository.
hint: Clones of the outer repository will not contain the contents of
hint: the embedded repository and will not know how to obtain it.
hint: If you meant to add a submodule, use:
hint:
hint: 	git submodule add <url> themes/academic
hint:
hint: If you added this path by mistake, you can remove it from the
hint: index with:
hint:
hint: 	git rm --cached themes/academic
hint:
hint: See "git help submodule" for more information.
```

エラーは吐くものの，deployはできて，ウェブでサイトも見られるので，疲れたし（笑），しばらくは，このままで行くことにする．themeのインストールは，submoduleで行うべきだったと，今更にして気がついた．次回アップデートする時にやってみよう．．．(^^;;;

で，結局，気になって，academic themeのインストールをやり直した（笑）．次回の記事（[How to update hugo and academic theme (Part2)](../how-to-update-hugo-and-academic-theme-part2)）を参照されたい．
