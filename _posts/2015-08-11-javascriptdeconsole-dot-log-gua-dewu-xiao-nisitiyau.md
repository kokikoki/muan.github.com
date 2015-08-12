---
layout: post
title: "JavaScriptでconsole.logを一括で無効にしちゃう"
modified: 2015-08-11 13:41:56 +0900
tags: [javascript]
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---

![コーヒー](/images/2015/08/01405.jpg)


デバッグ表示したい時だけ`console.log(obj)`みたいなコードを追記して確認が出来たら削除したりします。
そうすると手動なので消し忘れてしまい、IE8でハマったりするわけです。IE8は、最低ですね。ほんとに最低。

不要になった場合に、一括で機能を無効にできるようにやっつけてやりましょう。

今回は、関数を上書きせずに新しい関数を定義します。ついでに色も付けてみます。

## 準備

~~~javascript

var app = app || {};

app.log = function () {
    if (app.DEBUG) {
        var args = Array.prototype.slice.call(arguments);
        if (window.console && window.console.log) {
            args.unshift('%c[DEBUG]', 'font-weight: bold; font-size: 13px; color:#999;');// 色を付ける
            console.log.apply(console, args);
        }
    }
};
    
~~~


## 使ってみる

~~~javascript

app.DEBUG = true;
app.log('表示される', 'hoge', 'fuga');

app.DEBUG = false;
app.log('表示されない', 'hoge', 'fuga');
~~~


<br />


「[JavaScriptのデバッグモードで開発時の時間短縮](/2015/08/10/javascriptfalsedebugmododekai-fa-shi-falseshi-jian-duan-suo/)」で書いたデバッグモードと一緒に使うとさらに便利です。

最近まで知りませんでしたけど、`console.error`や`concole.table`なんかもあるんですね。
