+++
title = "How to create interactive slides by R, plotly, d3.js and reveal.js (Part 1)"
author = ["taipapa"]
date = 2020-06-14
lastmod = 2020-07-04T15:24:05+09:00
type = "post"
draft = false
weight = 1
subtitle = "R, plotly, d3.js, reveal.jsでインタラクティブなスライドを作成する （その１）"
[image]
  placement = 3
  caption = "Dublin Castle"
+++

前回までreveal.jsとorg-revealを使ったスライド作成についてまとめてきたが，interactiveなスライドの作成には触れていない．そこで，これから何回かに分けてインタラクティブなスライドを作るための私の試行錯誤を後日の自分のためにまとめておくことにした．第１回目の今回はD3.jsについてである．

<!--more-->

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [D3.js](#d3-dot-js)
    - [References](#references)
    - [How to insert a figure created by d3.js into blog by hugo](#how-to-insert-a-figure-created-by-d3-dot-js-into-blog-by-hugo)
    - [How to insert a figure created by d3.js into slide by reveal.js](#how-to-insert-a-figure-created-by-d3-dot-js-into-slide-by-reveal-dot-js)

</div>
<!--endtoc-->


## D3.js {#d3-dot-js}

**Rainbow Worm**

<div id='demo'>
  <script src="http://d3js.org/d3.v3.min.js" charset="utf-8"></script>
  <script>
    var margin = {top: 60, right: 60, bottom: 60, left: 60},
    width = 700 - margin.left - margin.right,
    height = 360 - margin.top - margin.bottom;
    var x = d3.scale.linear()
    .domain([0, 5.9])
    .range([0, width]);
    var y = d3.scale.linear()
    .domain([-1, 1])
    .range([height, 0]);
    var z = d3.scale.linear()
    .domain([0, 5.9])
    .range([0, 360]);
    var points = d3.range(0, 6, .1).map(function(t) {
    return {value: t, 0: x(t), 1: y(Math.sin(t))};
    });
    var svg = d3.select("#demo").append("svg")
    .attr("width", width + margin.left + margin.right)
    .attr("height", height + margin.top + margin.bottom)
    .append("g")
    .attr("transform", "translate(" + margin.left + "," + margin.top + ")");
    var path = svg.selectAll("path")
    .data(quad(points))
    .enter().append("path")
    .style("fill", function(d) { return d3.hsl(z(d[1].value), 1, .5); })
    .style("stroke", "#000");
    var t0 = Date.now();
    d3.timer(function() {
    var dt = (Date.now() - t0) * .001;
    points.forEach(function(d) { d[1] = y(d.scale = Math.sin(d.value + dt)); });
    path.attr("d", function(d) { return lineJoin(d[0], d[1], d[2], d[3], 80 * d[1].scale * d[1].scale + 10); });
    });
    // Compute quads of adjacent points [p0, p1, p2, p3].
    function quad(points) {
    return d3.range(points.length - 1).map(function(i) {
    return [points[i - 1], points[i], points[i + 1], points[i + 2]];
    });
    }
    // Compute stroke outline for segment p12.
    function lineJoin(p0, p1, p2, p3, width) {
    var u12 = perp(p1, p2),
    r = width / 2,
    a = [p1[0] + u12[0] * r, p1[1] + u12[1] * r],
    b = [p2[0] + u12[0] * r, p2[1] + u12[1] * r],
    c = [p2[0] - u12[0] * r, p2[1] - u12[1] * r],
    d = [p1[0] - u12[0] * r, p1[1] - u12[1] * r];
    if (p0) { // clip ad and dc using average of u01 and u12
    var u01 = perp(p0, p1), e = [p1[0] + u01[0] + u12[0], p1[1] + u01[1] + u12[1]];
    a = lineIntersect(p1, e, a, b);
    d = lineIntersect(p1, e, d, c);
    }
    if (p3) { // clip ab and dc using average of u12 and u23
    var u23 = perp(p2, p3), e = [p2[0] + u23[0] + u12[0], p2[1] + u23[1] + u12[1]];
    b = lineIntersect(p2, e, a, b);
    c = lineIntersect(p2, e, d, c);
    }
    return "M" + a + "L" + b + " " + c + " " + d + "Z";
    }
    // Compute intersection of two infinite lines ab and cd.
    function lineIntersect(a, b, c, d) {
    var x1 = c[0], x3 = a[0], x21 = d[0] - x1, x43 = b[0] - x3,
    y1 = c[1], y3 = a[1], y21 = d[1] - y1, y43 = b[1] - y3,
    ua = (x43 * (y1 - y3) - y43 * (x1 - x3)) / (y43 * x21 - x43 * y21);
    return [x1 + ua * x21, y1 + ua * y21];
    }
    // Compute unit vector perpendicular to p01.
    function perp(p0, p1) {
    var u01x = p0[1] - p1[1], u01y = p1[0] - p0[0],
    u01d = Math.sqrt(u01x * u01x + u01y * u01y);
    return [u01x / u01d, u01y / u01d];
    }
  </script>
</div>

D3.jsとはデータに基づいて文書を操作するためのJavaScript libraryである，と御本家に書いてあるが，これだけではよく分からない．私としてはウェブページの図がインタラクティブになるように設定したり，図の動きを設定したりするもの（上図のようなもの）ぐらいに理解した．d3について膨大な情報がネットに流れているのでそれを利用すれば良い，と言うのは易く，行なうのは難い（笑）．とりあえず情報を独断と偏見で以下にまとめた．


### References {#references}

-   [D3 Data-Driven Documents](https://d3js.org/) 御本家
-   [Client-Side Web Development](https://info343.github.io/) Webの基礎知識をつけるために素人が流し読みするには最適
-   [Interactive Data Visualization for the Web, 2nd Ed](https://alignedleft.com/tutorials/d3)　下記のサイトでベストと推薦
-   [Resources that are useful](http://www.rebeccabarter.com/useful%5Fresources/)　色々なリソースを集積してくれている
-   [D3.js と reveal.js の紹介](https://techplay.jp/event/452880)
-   [これから D3.js を始める人のためのメモ](https://qiita.com/corestate55/items/e70d5981c33a89f63367)
-   [Can D3.js visualizations be served using Hugo?](https://stackoverflow.com/questions/60746090/can-d3-js-visualizations-be-served-using-hugo) HugoでのD3.jsの使い方
-   [Rainbow Worm](https://bl.ocks.org/mbostock/4165404/c9d1f8d69fcdf17b352235419c190e1a36645ce8)


#### 注意点 {#注意点}

D3.jsはv3, v4, v5があり，v3からv4, v5にupgradeした時に色々と変更されており，ウェブで見たら動いているのに，ローカルにコピーして，自分でコードを変更したら動かない，なんて時はversionが違うせいのことがあるので注意．

-   [【d3.js】v3からv4とかv5にしたらいろいろ動かない](https://qiita.com/mk-tool/items/d9f74e9b8ecaf1a3e3ab)
-   [D3.jsをv3からv4/v5にバージョンアップした際の変更点メモ](https://qiita.com/zak%5Fy/items/1a4e7117d7a1791d54d3)


### How to insert a figure created by d3.js into blog by hugo {#how-to-insert-a-figure-created-by-d3-dot-js-into-blog-by-hugo}

それではインタラクティブなスライドを作成する前に，実際にこのページにd3.jsで書いた図を埋め込んでみよう．ただのグラフでは面白くないので，有名な（？）[Rainbow Worm](https://bl.ocks.org/mbostock/4165404/c9d1f8d69fcdf17b352235419c190e1a36645ce8)を埋め込むことにした．やり方は上にあげた[Can D3.js visualizations be served using Hugo?](https://stackoverflow.com/questions/60746090/can-d3-js-visualizations-be-served-using-hugo)にあるとおり．[Rainbow Worm](https://bl.ocks.org/mbostock/4165404/c9d1f8d69fcdf17b352235419c190e1a36645ce8)にあるコードのうちscript以下の部分をコピーし以下のように若干変更して，RainbowWorm.jsとして保存する．

{{< highlight js "linenos=table, linenostart=1" >}}
// var margin = {top: 100, right: 100, bottom: 100, left: 100},
//     width = 960 - margin.left - margin.right,
//     height = 500 - margin.top - margin.bottom;

var margin = {top: 60, right: 60, bottom: 60, left: 60},
    width = 700 - margin.left - margin.right,
    height = 360 - margin.top - margin.bottom;

var x = d3.scale.linear()
    .domain([0, 5.9])
    .range([0, width]);

var y = d3.scale.linear()
    .domain([-1, 1])
    .range([height, 0]);

var z = d3.scale.linear()
    .domain([0, 5.9])
    .range([0, 360]);

var points = d3.range(0, 6, .1).map(function(t) {
  return {value: t, 0: x(t), 1: y(Math.sin(t))};
});

var svg = d3.select("#demo").append("svg")
    .attr("width", width + margin.left + margin.right)
    .attr("height", height + margin.top + margin.bottom)
  .append("g")
    .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

var path = svg.selectAll("path")
    .data(quad(points))
  .enter().append("path")
    .style("fill", function(d) { return d3.hsl(z(d[1].value), 1, .5); })
    .style("stroke", "#000");

var t0 = Date.now();
d3.timer(function() {
  var dt = (Date.now() - t0) * .001;
  points.forEach(function(d) { d[1] = y(d.scale = Math.sin(d.value + dt)); });
  path.attr("d", function(d) { return lineJoin(d[0], d[1], d[2], d[3], 80 * d[1].scale * d[1].scale + 10); });
});

// Compute quads of adjacent points [p0, p1, p2, p3].
function quad(points) {
  return d3.range(points.length - 1).map(function(i) {
    return [points[i - 1], points[i], points[i + 1], points[i + 2]];
  });
}

// Compute stroke outline for segment p12.
function lineJoin(p0, p1, p2, p3, width) {
  var u12 = perp(p1, p2),
      r = width / 2,
      a = [p1[0] + u12[0] * r, p1[1] + u12[1] * r],
      b = [p2[0] + u12[0] * r, p2[1] + u12[1] * r],
      c = [p2[0] - u12[0] * r, p2[1] - u12[1] * r],
      d = [p1[0] - u12[0] * r, p1[1] - u12[1] * r];

  if (p0) { // clip ad and dc using average of u01 and u12
    var u01 = perp(p0, p1), e = [p1[0] + u01[0] + u12[0], p1[1] + u01[1] + u12[1]];
    a = lineIntersect(p1, e, a, b);
    d = lineIntersect(p1, e, d, c);
  }

  if (p3) { // clip ab and dc using average of u12 and u23
    var u23 = perp(p2, p3), e = [p2[0] + u23[0] + u12[0], p2[1] + u23[1] + u12[1]];
    b = lineIntersect(p2, e, a, b);
    c = lineIntersect(p2, e, d, c);
  }

  return "M" + a + "L" + b + " " + c + " " + d + "Z";
}

// Compute intersection of two infinite lines ab and cd.
function lineIntersect(a, b, c, d) {
  var x1 = c[0], x3 = a[0], x21 = d[0] - x1, x43 = b[0] - x3,
      y1 = c[1], y3 = a[1], y21 = d[1] - y1, y43 = b[1] - y3,
      ua = (x43 * (y1 - y3) - y43 * (x1 - x3)) / (y43 * x21 - x43 * y21);
  return [x1 + ua * x21, y1 + ua * y21];
}

// Compute unit vector perpendicular to p01.
function perp(p0, p1) {
  var u01x = p0[1] - p1[1], u01y = p1[0] - p0[0],
      u01d = Math.sqrt(u01x * u01x + u01y * u01y);
  return [u01x / u01d, u01y / u01d];
}
{{< /highlight >}}

<br>

最初の部分の変更は大きさや位置の調整である．25行目の var svg = d3.select("#demo").append("svg") の部分は元のままでも良いと思うが，何かを参考にbodyをdemoに書き換えた．このRainbowWorm.jsを/Users/taipapa/Data/MyBlog/Taipapablog/static/js/に保存すればhugoが読みに行ってくれる（要するにhugoのstatic directory以下に保存すれば良い．subdirectoryを作っても良い）．そして，以下のようにox-hugoの原稿に書き込む．

```html
#+begin_export html
<script src="http://d3js.org/d3.v3.min.js" charset="utf-8"></script>
<div id='demo'></div>
<script src="/js/RainbowWorm.js"></script>
#+end_export
```

（最初の行が，d3.v3.min.js とversion 3であることに注意．version 5を意味するv5にすると動かない）

これでC-c C-e HAによりexportすれば，最初の図のようにrainbow wormが表示される．美しい．．．


### How to insert a figure created by d3.js into slide by reveal.js {#how-to-insert-a-figure-created-by-d3-dot-js-into-slide-by-reveal-dot-js}

それでは，スライドにd3.jsで作成した図，今回はRainbow wormを埋め込んでみよう．上述のコードをox-revealのファイルに書き込めば良い． **#+REVEAL\_HTML:** をつけるのがミソである．

```lisp
* Rainbow Worm
  #+REVEAL_HTML: <script src="http://d3js.org/d3.v3.min.js" charset="utf-8"></script>
  #+REVEAL_HTML: <div id='demo'></div>
  #+REVEAL_HTML: <script src="RainbowWorm.js"></script>
```

これでexportすれば，スライドタイトルが "Rainbow Worm" となりその下にrainbow wormが蠢いているhtml スライドができる．せっかくなので，出来上がったスライドを，<http://taipapamotohus.com/MySlide%5Fv2/>にアップしておくとともに，このページにも埋め込んでおくことにする．前々回の記事（[How to create presentation slides by reveal.js and org-mode (org-reveal) Part2](../how-to-create-presentation-slides-by-reveal-dot-js-and-org-mode-org-reveal-part2) ）で説明したボックス，カラー，シャドウの追加なども行って，D3.jsも加えて置く．Rainbow wormは最後のページ（9ページ）に表示されている．

<iframe src="http://taipapamotohus.com/MySlide_v2/" width="1000" height="600" frameborder="0" allowfullscreen="allowfullscreen" allow="geolocation *; microphone *; camera *; midi *; encrypted-media *"></iframe>

<br>
<br>
    よく考えてみたら，rainbow wormではinteractive slideとは言えないことに気が付いたが，その辺りは次回以降で検討していくことにする．どうしてもあれは載せてみたかった．．．．
<br>
<br> ここまでで，hugoで作成するブログやreveal.jsで作成するスライドにD3.jsで作成する画像やグラフを埋め込む方法は分かった．しかし，問題は，d3.jsでグラフを作成するのが（私には）とても面倒である，と言うことである．Rなら，使い慣れているので，Rで作ったグラフをインタラクティブにできないかと考えた．ん，そう言うものがあったような．．．

と言うことで，ようやく **Plotly** の存在を思い出した．とりあえず，今回はここまで．苦労した挙句に，どうやらD3.jsは，私にはあまり使えそうにないと言うことが分かったと言う顛末である.....😅

次回はplotlyについてまとめるつもりである．
