+++
title = "How to update hugo and academic theme (Part2)"
author = ["taipapa"]
date = 2019-08-24
lastmod = 2019-08-25T00:55:39+09:00
type = "post"
draft = false
weight = 1
subtitle = "Hugoとacademic テーマのアップデート　その2"
[image]
  placement = 3
  caption = "Paris"
+++

前回（[How to update hugo and academic theme (Part 1)](../how-to-upgrade-hugo-and-academic-theme)）にhutoとacademicのアップデートをまとめたが，やはり，githubにdeployするときのエラーが，どうしても気になる．そこで，今回は，Academic themeのインストールを一からやり直すことにした．   Hugoとox-hugoのインストールやアップデートは前回の記事（[How to update hugo and academic theme (Part 1)](../how-to-upgrade-hugo-and-academic-theme)）を参考にしていただきたい．   <!--more-->

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [References](#references)
- [Install Academic Kickstart with Git](#install-academic-kickstart-with-git)
- [Migrate Content](#migrate-content)

</div>
<!--endtoc-->


## References {#references}

-   [Install](https://sourcethemes.com/academic/docs/install/) Academicご本家の解説
-   [Update](https://sourcethemes.com/academic/docs/update/) Academicご本家の解説


## Install Academic Kickstart with Git {#install-academic-kickstart-with-git}

やはり，公式ドキュメントに従うのが良いだろうと考え，上記のリンク先のInstallの解説の通りにしてみた．git submoduleを使うので，よく分からんと敬遠していたのだが，やってみると（作業自体は）簡単であった． [Git submodule の基礎](https://qiita.com/sotarok/items/0d525e568a6088f6f6bb) によれば，「git submodule は、外部の git リポジトリを、自分の git リポジトリのサブディレクトリとして登録し、特定の commit を参照する仕組み」である．

~~1. まず，Academic Kickstart repository を自分のウェブサイトを作るためにクローンする．~~

{{% alert note %}}
またも間違っていた．上述の公式ドキュメントにはちゃんと，Fork the Academic Kickstart repository to create a new website と書いてあったのに読み過ごしていた．
{{% /alert %}}

1.  ということで，まず，[academic-kickstart](https://github.com/sourcethemes/academic-kickstart#fork-destination-box) に行って，右上の **Fork** ボタンを押して，自分のgithub pageにacademic-kickstartのrepositoryを作成する．

2.  ついで，この自分のgithub pageのforkをgit cloneで自分のサイトにクローンする．

    ```sh
    $ pwd
    /Data/MyBlog
    $ git clone https://github.com/taipapamotohus/academic-kickstart Taipapablog
    Cloning into 'Taipapablog'...
    remote: Enumerating objects: 579, done.
    remote: Total 579 (delta 0), reused 0 (delta 0), pack-reused 579
    Receiving objects: 100% (579/579), 163.92 KiB | 480.00 KiB/s, done.
    Resolving deltas: 100% (179/179), done.
    ```

3.  ついで，themeをsubmoduleでinitializeする．

    ```sh
    $ cd Taipapablog
    $ pwd
    /Data/MyBlog/Taipapablog
    $ git submodule update --init --recursive
    Submodule 'themes/academic' (https://github.com/gcushen/hugo-academic.git) registered for path 'themes/academic'
    Cloning into '/Data/MyBlog/Taipapablog/themes/academic'...
    Submodule path 'themes/academic': checked out '32b6145c06835e33668100105ac1835593cf8d72'
    $ ls
    LICENSE.md          config/             netlify.toml        themes/
    README.md           content/            scripts/            update_academic.sh*
    academic.Rproj      data/               static/             view.sh*
    $ ll
    total 72
    drwxr-xr-x  18 kohkichi  admin   576 Aug 24 16:34 .
    drwxr-xr-x  18 kohkichi  admin   576 Aug 24 16:35 ..
    -rw-r--r--   1 kohkichi  admin   298 Aug 24 16:34 .editorconfig
    drwxr-xr-x  13 kohkichi  admin   416 Aug 24 16:34 .git
    -rw-r--r--   1 kohkichi  admin    15 Aug 24 16:34 .gitignore
    -rw-r--r--   1 kohkichi  admin   106 Aug 24 16:34 .gitmodules
    -rw-r--r--   1 kohkichi  admin  1078 Aug 24 16:34 LICENSE.md
    -rw-r--r--   1 kohkichi  admin  2671 Aug 24 16:34 README.md
    -rw-r--r--   1 kohkichi  admin   258 Aug 24 16:34 academic.Rproj
    drwxr-xr-x   3 kohkichi  admin    96 Aug 24 16:34 config
    drwxr-xr-x   7 kohkichi  admin   224 Aug 24 16:34 content
    drwxr-xr-x   3 kohkichi  admin    96 Aug 24 16:34 data
    -rw-r--r--   1 kohkichi  admin   380 Aug 24 16:34 netlify.toml
    drwxr-xr-x   3 kohkichi  admin    96 Aug 24 16:34 scripts
    drwxr-xr-x   3 kohkichi  admin    96 Aug 24 16:34 static
    drwxr-xr-x   3 kohkichi  admin    96 Aug 24 16:34 themes
    -rwxr-xr-x   1 kohkichi  admin  1628 Aug 24 16:34 update_academic.sh
    -rwxr-xr-x   1 kohkichi  admin    49 Aug 24 16:34 view.sh
    ```

    う〜む，実質2行のコマンドで終わり，これで，あとは，hugo serverと叩いて，ブラウザで，localhost:1313を開けば，デモ用のサイトが立ち上がる．非常に簡単である．自分のGitHub pageを持っていれば，2-3分でできてしまう．


## Migrate Content {#migrate-content}

前回（[How to update hugo and academic theme (Part 1)](../how-to-upgrade-hugo-and-academic-theme)）にすでに投稿記事などはスクリプトなどを使ってアップデートしてあるし，設定ファイルもアップデート済みである．そちらのディレクトリから，config, content (home, postなど主要な部分を含む), layouts, static, assetsなどを，今回新たに作成したTaipapablog directoryにコピーすれば良い．コピー後にもう一度Taipapablog directoryの中身を見ると，

```sh
$ ls
LICENSE.md                   layouts/
README.md                    netlify.toml
Taipapablog-20180816_v2.org  public/
academic.Rproj               resources/
assets/                      scripts/
config/                      static/
content/                     themes/
data/                        update_academic.sh*
deploy.sh*                   view.sh*
```

こんな風になっているはずである．（Taipapablog-20180816\_v2.orgは，ox-hugoで書いたこのブログの原稿）

これで，hugo root directoryで，hugo serverを叩くと，元のブログが立ち上がる．

あれ，今回は簡単にできてしまった．既に，前回で，Page Bundlesやfeatured imageに合わせて修正していたから，本当に流し込むだけであった．さて，これで，deployしてもエラーが出なければOKである．

~~う〜ん，deployはできて，前回のエラーも消えたけど，また，別のエラーが出る．．．gitは苦手だ．．．(ToT)~~

```sh
remote: Permission to sourcethemes/academic-kickstart.git denied to taipapamotohus.
fatal: unable to access 'https://github.com/sourcethemes/academic-kickstart.git/': The requested URL returned error: 403
```

~~これって，master repositoryを上書きしようとしてるな．deploy scriptを直さないといけない．．．~~

~~ウェブからは見えてるので，とりあえず，後日に見直してみよう．．．(^^;;;~~
