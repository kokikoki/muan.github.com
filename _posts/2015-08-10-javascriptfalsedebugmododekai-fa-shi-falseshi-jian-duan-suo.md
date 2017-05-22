---
layout: post
title: "JavaScriptのデバッグモードで開発時の時間短縮"
modified: 2015-08-10 10:26:21 +0900
tags: [javascript]
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---


JavaScriptで、開発時に既に確認済みのシーケンス(オープニングアニメーションなど)を毎回表示することは時間の無駄です。<br />   
開発時は、確認までの時間短縮をするためにデバッグモードを追加します。<br />
デバッグモードの分岐処理で、既に確認済みの処理を短縮します。


##仕様

下記のURLにアクセスすることでデバッグモードの切替をすることにします。

####デバッグ開発時
~~~url
http://localhost/?d=1
~~~

####完全な動作確認を行うとき

~~~url
http://localhost/?d=0
~~~

## 準備
[https://github.com/carhartl/jquery-cookie](https://github.com/carhartl/jquery-cookie)
からjquery.cookie.jsをダウンロードしてロードします。

下記のコードを実装します。

~~~javascript
var getParams = function (name) {
    name = name.replace(/[\[]/, "\\[").replace(/[\]]/, "\\]");
    var regex = new RegExp("[\\?&]" + name + "=([^&#]*)"),
        results = regex.exec(location.search);
    return results === null ? "" : decodeURIComponent(results[1].replace(/\+/g, " "));
};

var debugMode = function () {
    var d = getParams('d');
    if (d) {
        $.cookie('debug', d, {path: '/'});
    }
    var debugMode = $.cookie('debug') || d;
    return debugMode || 0;
};
~~~

## 実装 

これで、下記のように分岐処理を行えます。

~~~javascript
var app = app || {};
app.DEBUG_MODE = debugMode();

if (app.DEBUG_MODE) {
    // デバッグ時のコード
} else {
    // 本番時のコード
}
~~~


<br />  

アニメーションの時間を少なくしたり、必要のないDOMを非表示にしたりなどできるのでとっても便利ですね！

