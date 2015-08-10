---
layout: post
title: "javascriptで追随メニューをもっと便利に"
modified: 2014-04-17 08:21:27 +0900
tags: [jquery,javascript]
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---


`position:fixed`で追随メニューを作りました。
しかし、ただの追随ではありません。一定のラインを超えたら自動で消えてくれます。そして、フッターまで来ると誘導してくれるかのように表示してくれます。
さらに、ページをスクロールアップした場合にも表示してくれます。

[追随メニューテスト](/page/imitation-menu-test.html)

### html
~~~ html
<div id="menu">
    MENU
</div>
~~~

### css
~~~ css
body {
    height: 2000px;
}
#menu {
    position: fixed;
    top: 0;
    right: 0;
    width: 200px;
    height: 200px;
    background-color: #ccc;
    text-align: center;
}
~~~

### javascript

ちょっとコード見づらくてごめんなさい。

~~~js
$(function () {

    $.fn.imitationMenu = function (options) {

        var $doc = $(document);
        var minHeight = options['minHeight'];
        if (minHeight > $doc.height()) {
            return;
        }
        var defaultTop = options['defaultTop'];
        var hideOffset = options['hideOffset'];
        var showBottom = options['showBottom'];
        var $target = $(this);
        var isUpScroll = false;
        var upShow = false;
        var beforeTop = $(window).scrollTop();
        var beginUpTop = beforeTop;

        $(window).scroll(onScroll).trigger('scroll');

        function onScroll() {
            var $this = $(this);
            var top = $this.scrollTop();
            var scrollHeight = $doc.height();
            var scrollPosition = $this.height() + top;

            isUpScroll = (beforeTop > top);
            if (isUpScroll) {
                if (beginUpTop == 0) {
                    beginUpTop = top;
                }
                upShow = (beginUpTop - top) > 50;
            } else {
                beginUpTop = 0;
                upShow = false;
            }

            if (top > defaultTop) {
                $target.css({position: 'fixed', top: 0});
            } else if (top < defaultTop) {
                $target.css({position: 'absolute', top: defaultTop});
            }

            if (top > hideOffset) {
                if ((scrollHeight - scrollPosition < showBottom) || upShow) {
                    if ($target.css('opacity') == '0') {
                        $target.stop().css({visibility: 'visible'}).animate({opacity: '1'});
                    }
                } else if ($target.css('opacity') == '1') {
                    $target.stop().animate({opacity: '0'}, function () {
                        $(this).css({visibility: 'hidden'});
                    });
                }
            } else if (top < hideOffset) {
                if ($target.css('opacity') == '0') {
                    $target.stop().css({visibility: 'visible'}).animate({opacity: '1'});
                }
            }
            beforeTop = top;
        }
    };

    $('#menu').imitationMenu({defaultTop: 100, hideOffset: 300, showBottom: 100, minHeight: 1000});
});

~~~

## 使い方説明します

~~~ js
$('#menu').imitationMenu({defaultTop: 100, hideOffset: 300, showBottom: 100, minHeight: 1000});
~~~


`imitationMenu`関数を登録して呼び出ししています。

オプションは下記のようになっています。

* `defaultTop` メニューの規定のTOP位置です。
* `hideOffset` 非表示にするTOP位置です。300pxに達したら非表示になります。
* `showBottom` 表示にするBOTTOM位置です。下から100pxに達したら表示します。
* `minHeight` 追随する最低の高さです。ドキュメントサイズが1000px以上の場合にのみ追随動作を行います。

オプションにするの忘れてましたが、スクロールアップの判定に遊びを50pxほど設けてます。

限られた画面のスペースを有効に使いたい場合に大いに役に立ちそうですね。
