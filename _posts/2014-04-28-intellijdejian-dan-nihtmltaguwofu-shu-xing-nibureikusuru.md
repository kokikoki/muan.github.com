---
layout: post
title: "IntelliJで簡単にHTMLタグを複数行にブレイクする"
modified: 2014-04-28 23:51:49 +0900
tags: [html,intellij]
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---

こんな一行にまとめたHTMLコードを下のような複数行に崩したい場合が頻繁にあると思います。

~~~html
<li><a href="http://blog.condesire.com/about/">Learn More</a></li>
~~~

~~~html
<li>
    <a href="http://blog.condesire.com/about/">Learn More</a>
</li>
~~~

どうしていましたか？

私は、カーソルを改行したい位置まで持って行ってエンターを押してました。よく行うキー操作ですが面倒くさいです。

Intellijなら自動で行うショートカットキーが存在するはずだと思い探してみますが見つからずでした。

ですが、[In IntelliJ's XML editor, how to break a tag into multiple lines?](http://stackoverflow.com/questions/17472037/in-intellijs-xml-editor-how-to-break-a-tag-into-multiple-lines)このページを見つけて解決しました。

キーボードマクロという発想はありませんでした。

このサイトの`reformat code`がうまく動作しなかったので、自分なりに登録してみました。

## キーボードマクロを登録する

ideavimを使う前提です。

Edit > Macros > Start Macro Recording でマクロ記録を開始します。

1. `0` - 行頭へ
1. `f` `>` - >までカーソル移動
1. `a` `Enter` - >の次で改行挿入
1. `ESC`
1. `$` - 行末へ
1. `F` `<` - <までカーソル移動
1. `i` `Enter` <の前で改行挿入
1. `ESC`

Edit > Macros > Stop Macro Recording でマクロ記録を終了します。

`HTML Tag Break` とか適当に名前を付けて保存します。

そのままキーマップに登録しましょう。

これでまた一つ面倒くさい作業から開放されました！ﾔﾌｩ!!