---
layout: post
title: "Github Pages でjekyllを動かしてみたけどやっぱりさくらVPSに戻した"
modified: 2014-04-08 21:31:14 +0900
tags: [jekyll,github]
image:
  feature:
  credit: 
  creditlink: 
comments: true
share: true
---

ということで、githubにバックアップ取りつつもデプロイもやりつつの一石二鳥な事をやってみたいと思います。

まずは、どうやるのか調査しました。

`jekyll github` でグーグル検索して下記サイトがヒットしました。
<a href="http://akkunchoi.github.io/jekyll-github-blogging.html" target="_blank">http://akkunchoi.github.io/jekyll-github-blogging.html</a>



1. [username].github.com リポジトリを作成
1. リポジトリに既存のjekyllファイル群(生成したHTMLを含まない)をコミット
1. 10分後くらいに[username].github.comにアクセスして表示されることを確認

これで公開完了ですね。githubにプッシュするだけでブログが公開できます。

### 独自ドメインで公開

せっかくなので独自ドメインの設定もやってみました。

<a href="http://qiita.com/mofumofu3n/items/b859fb3c5d924cfcca15" target="_blank">http://qiita.com/mofumofu3n/items/b859fb3c5d924cfcca15</a>

Aレコードに `204.232.175.78`を追加するんですね！
そして、[username].github.comリポジトリに`CNAME`ファイルを作って内容を設定したドメインにしますとっ。
あとは、プッシュして少々お待ちいただくだけのようでした。

### Github Pagesでjekyllを使う場合の注意点がある
Github Pagesではプラグインが使えないというのは読んでわかっていましたが、
私の場合コードハイライトが動作しなかったので、さくらVPSにrsyncする形に戻しました...。

