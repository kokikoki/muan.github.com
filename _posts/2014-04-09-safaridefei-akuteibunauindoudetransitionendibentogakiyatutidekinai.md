---
layout: post
title: "Safariで非アクティブなウインドウでtransitionEndイベントがキャッチできない"
modified: 2014-04-09 17:11:54 +0900
tags: [javascript,css]
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---



CSSのtransitionプロパティを使う機会があって使ってみたけど、1点問題に当たりました。
Safari以外のブラウザは問題なかったのですが、Safariだけウインドウをアクティブにしていない場合に
transitionEndイベントをキャッチできませんでした。

下記のようなjs,cssを記述した場合に5秒ごとに`.slides`の子要素の画像が切り替わるのですが、
Safariで別タブなどを開いていた場合に途中で画像が切り替わっていない現象がありました。

### html
~~~ html
<div class="slides">
    <div class="slide-img"><img src="slide10.jpg"/></div>
    <div class="slide-img"><img src="slide09.jpg"/></div>
    <div class="slide-img"><img src="slide08.jpg"/></div>
    <div class="slide-img"><img src="slide07.jpg"/></div>
    <div class="slide-img"><img src="slide06.jpg"/></div>
    <div class="slide-img"><img src="slide04.jpg"/></div>
    <div class="slide-img"><img src="slide03.jpg"/></div>
    <div class="slide-img"><img src="slide02.jpg"/></div>
    <div class="slide-img"><img src="slide01.jpg"/></div>
</div>
~~~

### css
~~~ css
.slide-img {
    -moz-transform: translateY(0);
    -webkit-transform: translateY(0);
    -o-transform: translateY(0);
    -ms-transform: translateY(0);
    transform: translateY(0);
}

.slidedown {
    -moz-transform: translateY(100%);
    -webkit-transform: translateY(100%);
    -o-transform: translateY(100%);
    -ms-transform: translateY(100%);
    transform: translateY(100%);
}
~~~

### javascript
~~~ js
function slideshow () {
    var interval = 5000;
    var $slides = $('.slides');
    function slide () {
        var $last = $slides.children().last();
        $last.addClass('slidedown').on('transitionend webkitTransitionEnd oTransitionEnd MSTransitionEnd',
            function (event) { $(this).prependTo($slides).removeClass('slidedown'); }
        );
    }
    setInterval(slide, interval);
}
~~~~


## 解決策

回避するために下記のjsを追加しました。
focusイベントをハンドリングして、transitionendイベントの処理と同じことをするようにします。

これで止まっていたアニメーションが再度開始されます。

~~~ js
$(window).focus(function() {
    var $slides = $('.slides');
    var $last = $('.slides').children().last();
    if ($last.prependTo($slides).hasClass('slidedown')) {
        $last.prependTo($slides).removeClass('slidedown');
    }
});
~~~~
