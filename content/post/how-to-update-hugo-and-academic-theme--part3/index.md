+++
title = "How to update hugo and academic theme (Part3)"
author = ["taipapa"]
date = 2019-09-04
lastmod = 2019-09-04T21:19:38+09:00
tags = ["hugo", "academic", "theme", "upgrade", "ox-hugo", "org-mode", "emacs"]
type = "post"
draft = false
weight = 1
subtitle = "Hugoとacademic テーマのアップデート　その3"
[image]
  placement = 3
  caption = "Beyoğlu, Istanbul"
+++

先日，ブログのアップデートについて2回に分けてまとめたが（[How to update hugo and academic theme (Part1)](../how-to-upgrade-hugo-and-academic-theme), [How to update hugo and academic theme (Part2)](../how-to-update-hugo-and-academic-theme-part2)），読み直してみるとhugoとacademic themeの全体的なアップデートのことで終わっていて，具体的な内容のアップデートについてはあまり書いていないことに気がついた．そこで，今回はその辺の細かいところについて，後日の自分のためにも，まとめておくことにした．今回の内容に関しては，ox-hugoのレベル，つまり，org-mode file ではどうすれば良いのか分からず，直接マークダウンファイルを弄らざるを得ない事が多かった．  <!--more-->

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [自己紹介](#自己紹介)
- [Widget in Academic](#widget-in-academic)
- [home pageの最初の画像](#home-pageの最初の画像)
- [Day (light) and night (dark) mode](#day--light--and-night--dark--mode)

</div>
<!--endtoc-->


## 自己紹介 {#自己紹介}

-   [Introduce yourself](https://sourcethemes.com/academic/docs/get-started/#introduce-yourself) academicのご本家の解説
-   リンク先に書いてあるが，content/authors/admin/\_index.mdに自分のプロファイルを書く．不要な部分は#で全てコメントアウトすれば良い．
-   avatorを表示するには，画像の名前をavatorとしてcontent/authors/admin/に保存する．例として元々あった画像と置き換えれば良い．


## Widget in Academic {#widget-in-academic}

-   [Getting Started With the Page Builder](https://sourcethemes.com/academic/docs/page-builder) ご本家の解説
-   widgetとは．．．ウィジェットである．(^^;;; ブログを組み立てる部品のようなもの．上記のリンク先に詳細に解説されている．例えば， **About** widjetは読者にブログ主を紹介するためのものである．ただし，実際のプロファイルの内容は前述のようにcontent/authors/admin/\_index.mdに記載されている． このブログのように，投稿記事を載せているだけの場合は，ほとんどのwidgetは不要である．
-   不要なwidgetは，content/home/のディレクトリから削除すれば良い．参考のために残して置きたければ， それぞれのindex.mdのfront matterの最初の方にある active = true を **active = false** とすれば良い．下図は， content/home/featured/index.mdのfeatured widget の場合である．これで featured widget は表示されなくなる．

<!--listend-->

```sh
+++
# A Featured Publications section created with the Featured Content widget.
# This section displays publications from `content/publication/` which have
# `featured = true` in their front matter.

widget = "featured"  # See https://sourcethemes.com/academic/docs/page-builder/
headless = true  # This file represents a page section.
active = false  # Activate this widget? true/false
weight = 80  # Order that this section will appear.
```


## home pageの最初の画像 {#home-pageの最初の画像}

ブログの最初のページに掲げる画像だが，アップデート前は， **hero widget** で設定していた．具体的には，content/home/hero.mdのなかで，front matterに

```sh
+++
# Hero widget.
widget = "hero"
active = true
.....
overlay_img = "headers/hogehoge.jpg"  # Image path relative to your `static/img/` folder.
.....
```

と書いていた．

academicをアップデート後は，画像の一部しか表示されなくなったために，content/home/hero/index.mdのfront matterに **active = false** と書いて，hero widget自体を無効にした．代わりに，content/home/slider/index.md のfront matterに　

```sh
+++
# Slider widget.
widget = "slider"  # See https://sourcethemes.com/academic/docs/page-builder/
headless = true  # This file represents a page section.
active = true  # Activate this widget? true/false
weight = 1  # Order that this section will appear.

# Slide interval.
# Use `false` to disable animation or enter a time in ms, e.g. `5000` (5s).
interval = false

# Slide height (optional).
# E.g. `500px` for 500 pixels or `calc(100vh - 70px)` for full screen.
height = "400px"

# Slides.
# Duplicate an `[[item]]` block to add more slides.
  [[item]]
title = "完璧な秋の日"
content = "とりあえず備忘録として :rocket:"
#  align = "center"  # Choose `center`, `left`, or `right`.

  # Overlay a color or image (optional).
  #   Deactivate an option by commenting out the line, prefixing it with `#`.
#  overlay_color = "#666"  # An HTML color value.
overlay_img = "hogefuga.jpg"  # Image path relative to your `static/img/` folder.
#  overlay_filter = 0.5  # Darken the image. Value in range 0-1.
.....
```

と書いて， **slider widget** を使うようにしたところ，ほぼ望み通りの画像表示となった（画像自体はstatic/img/に置いた ）．


## Day (light) and night (dark) mode {#day--light--and-night--dark--mode}

-   [Academic: the website builder for Hugo](https://themes.gohugo.io/theme/academic/post/getting-started/) こちらに書いてある機能

アップデートにより，今流行りのdark modeと通常のlight modeを，読者が右上のsun/moon iconをクリックすることで切り替えられるようになった．設定は，config/\_default/params.tomlの中で，以下のように **day\_night = true** とすれば良い．

```sh
.....
# Enable users to switch between day and night mode?
day_night = true
.....
```

  \\
今回は本当に殴り書きメモのような内容．それにhugoに関することは全然なくて，academicのことしか書いてない．看板に偽りありだな．．．(^^;;;
