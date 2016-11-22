---
layout: post
title: "webpackで共通ライブラリを別jsファイルにしてバンドル時間を短縮する"
modified: 2016-05-09 18:17:47 +0900
tags: [react,javascript,webpack,gulp]
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---

React.jsを使ったお仕事をする機会があって、その時に工夫したことを書きます。

今回の環境は、gulpを使いました。jsのバンドルだけwebpackで行い、ファイルが編集されたらbrowsersyncにリロードさせます。
その他css等は、gulpで行いました。

webpackを使用した場合、jsファイル毎にスコープが出来るのでファイル毎に必要なライブラリをrequireすることになります。<br />
React.jsやその他ライブラリのjsコードは、改変することはないのでuglifyで圧縮したり毎回バンドルする必要はありませんが
多くの共通ライブラリ用jsを毎回バンドルすることで、確認の度にバンドル待ちが発生してしまいます。

そこでアプリケーション用のjs以外を共通ライブラリ用のjsを分けて、
バンドルしてしまおうということを行いました。

- lib.js 共通ライブラリ用js
- app.js アプリケーション用js

app.jsからライブラリクラスを参照するために、lib.jsにバンドルされたクラス郡はglobalに宣言します。(任意の変数に宣言することもできます)<br />
app.jsからlib.jsの内容を参照する場合は、下記のようになります。

~~~javascript

// Reactはglobalに宣言されている
React.createClass({})
~~~

globalに宣言されているのでrequireする必要がありません。  <br />
また、任意の変数に紐付けた場合(例えばlib)は、`lib.React.createClass`という使い方になります。

lib.js用のwebpackの設定は下記のようになります。<br />
webpackの[expose-loader](https://github.com/webpack/expose-loader)をインストールしておきます。

~~~javascript

 webpack_lib: {
    entry: {
        lib: glob.sync('/js/lib/*')
    },
    resolve: {
        extensions: ['', '.js', '.es6', '.jsx']
    },
    output: { filename: '[name].js' },
    module: {
        loaders: [
            { test: /underscore\.js$/, loader: 'expose?_' },
            { test: /React\.js$/, loader: 'expose?React' },
            { test: /ReactDOM\.js$/, loader: 'expose?ReactDOM' },
            { test: /ReactRouter\.js$/, loader: 'expose?ReactRouter' },
            { test: /ReactGsapEnhancer\.js$/, loader: 'expose?GSAP' },
            { test: /ReactDocumentTitle\.js$/, loader: 'expose?DocumentTitle' },
            { test: /superagent\.js$/, loader: 'expose?superagent' },
            { test: /ClassNames\.js$/, loader: 'expose?classNames' },
        ]
    }
}

~~~

lib.jsは、`/js/lib/*`配下のjsファイルをバンドルしたものになります。<br />
重要なのは、loadersの`expose`を使用しているところです。<br />
任意の変数に紐付けるには、`expose?lib.React`にようにするとできます。

これで、gulpなどでwatchする場合はapp.jsとlib.jsで別れてバンドルされるので
確認がすぐに行なえます。


試してないけど、loadersに羅列しなくても１つのjsファイルに`require("expose?React");`で羅列してもいいかもしれません。


