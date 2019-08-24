+++
title = "Emacsのorg-modeを保存すると自動的にhtmlにexportされブラウザーが更新されるようにする"
author = ["taipapa"]
date = 2019-01-06
lastmod = 2019-02-01T01:06:33+09:00
tags = ["emacs", "org-mode", "autoexport", "html", "autorefresh", "browser"]
type = "post"
draft = false
weight = 1
[header]
  image = "headers/PlaceRoyaleBruxelles.jpg"
  caption = "Place Royale Bruxelles"
+++

org-modeで文書を書いていてhtmlにexportする際は，C-e h oとするわけだが，段々とこれが面倒になってくることがある．そこで，ネットを探ってみると，やはり，なんでも載ってるredditにhtml exportを自動化する関数の記事があった．

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [toggle-org-html-export-on-save](#toggle-org-html-export-on-save)
- [directoryの内容が変更されると，自動でhtmlを再読込する．](#directoryの内容が変更されると-自動でhtmlを再読込する)
- [使用方法](#使用方法)

</div>
<!--endtoc-->


## toggle-org-html-export-on-save {#toggle-org-html-export-on-save}

-   情報元：[How to auto export html when saving in org-mode?](https://www.reddit.com/r/emacs/comments/4golh1/how%5Fto%5Fauto%5Fexport%5Fhtml%5Fwhen%5Fsaving%5Fin%5Forgmode/?sort=old)

例によって，下記のようにinit.orgに書き込めばよい．

```lisp
#+begin_src emacs-lisp
(defun toggle-org-html-export-on-save ()
  (interactive)
  (if (memq 'org-html-export-to-html after-save-hook)
      (progn
        (remove-hook 'after-save-hook 'org-html-export-to-html t)
        (message "Disabled org html export on save for current buffer..."))
    (add-hook 'after-save-hook 'org-html-export-to-html nil t)
    (message "Enabled org html export on save for current buffer...")))
#+end_src
```

これで，toggle-org-html-export-on-saveで，htmlを自動で出力するかどうかを切り替え可能となる．しかし，これだけでは，org文書を保存するたびにブラウザーを手動でreloadしないといけなくなり，面倒である．自動でreloadしてくれるコマンドがあれば便利である．探してみると，これもネットに転がっていた．


## directoryの内容が変更されると，自動でhtmlを再読込する． {#directoryの内容が変更されると-自動でhtmlを再読込する}

-   情報元：[Watch for file changes and refresh your browser automatically](http://brettterpstra.com/2011/03/07/watch-for-file-changes-and-refresh-your-browser-automatically/)
-   上記サイトに有るrubyのスクリプトが使えそうなので，頂いた．
-   rubyのインストールについては，以下のようなサイトを参考
    -   [MacにHomeBrew,rbenv,bundlerをインストールする](https://qiita.com/shinkuFencer/items/3679cfd966f6a61ccd1b)
    -   [Ruby入門 01.導入（Macに最新版のRubyを入れる）](https://qiita.com/prgseek/items/ff037cc6134ff9303c67)
    -   [Ruby初学者のRuby On Rails 環境構築【Mac】](https://qiita.com/TAByasu/items/47c6cfbeeafad39eda07)
-   しかし，上記サイトのスクリプトをそのまま使用するとと，reloadの際にページの先頭まで戻ってしまい不便！
-   結局，上記サイトのFirefox用のスクリプトを参考に少し書き換えた下記のSafari用のスクリプトを使用すると，reloadの際に先頭まで戻らないので，こちらを使用することとした．

```sh
#!/usr/bin/env ruby
# watch.rb by Brett Terpstra, 2011 <http://brettterpstra.com>
# with credit to Carlo Zottmann <https://github.com/carlo/haml-sass-file-watcher>

trap("SIGINT") { exit }

if ARGV.length < 2
   puts "Usage: #{$0} watch_folder keyword"
   puts "Example: #{$0} . mywebproject"
   exit
   end

   dev_extension = 'dev'
   filetypes = ['css','html','htm','php','rb','erb','less','js']
   watch_folder = ARGV[0]
   keyword = ARGV[1]
   puts "Watching #{watch_folder} and subfolders for changes in project files..."

   while true do
         files = []
         filetypes.each {|type|
files += Dir.glob( File.join( watch_folder, "**", "*.#{type}" ) )
                        }
         new_hash = files.collect {|f| [ f, File.stat(f).mtime.to_i ] }
         hash ||= new_hash
         diff_hash = new_hash - hash

         unless diff_hash.empty?
         hash = new_hash

         diff_hash.each do |df|
             puts "Detected change in #{df[0]}, refreshing"
         %x{osascript<<ENDGAME
                tell app "Safari" to activate
                tell app "System Events"
                     keystroke "r" using command down
                end tell
ENDGAME
           }
         end
         end

         sleep 1
         end
```

-   このスクリプトにwatch\_safari.rbという名前をつけてパスが通っている/usr/local/binに保存し，chomod a+x watch\_safari.rbとして実行権限を付けた．
-   **Usage: /usr/local/bin/watch\_safari.rb watch\_folder keyword**
-   パスを通しておけば，watch\_safari.rb watch\_folder keyword で大丈夫


## 使用方法 {#使用方法}

-   /Data/Hoge/Fuga/hogefuga.orgを書いているとすると以下のようにそのディレクトリをみはらせておく．

```sh
$ cd /Data/Hoge
$ watch_safari.rb Hoge hogefuga.html
```

-   org-modeでhogefuga.orgを書きはじめるときに，M-x toggle-org-html-export-on-save として保存するたびに自動的に新たなhtmlがexportされるようにする．
-   最初だけは，C-e h oでhtmlをexportして，safariでhogefuga.htmlを開いておく．
-   以降は，hogefuga.org文書を保存するたびに，現在見ている場所に戻った状態で最新のhtmlに更新されるようになる．便利である．

以上はOSX上のSafariを使用している場合であるが，他のブラウザーでも少し変更するだけで同じことができるはずである．
