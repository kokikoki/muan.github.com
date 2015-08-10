---
layout: post
title: "Alfredのworkflow ＋ AppleScriptでFinderをアクティブにするグローバルホットキーを実現する"
modified: 2014-04-20 07:52:31 +0900
tags: [alfred,applescript]
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---

<img src="/images/2014/2014_04_20_img1.png" />
<br /> <br />

前提として、Alfredの[PowerPack($17)](http://www.alfredapp.com/powerpack/)を購入しているが必要です。

とくかく設定してみます。

## Alfredの設定

Alfredを起動して、`Command + ,`を押して設定画面を開きます。

WorkFlowを選択します。


<img src="/images/2014/2014_04_20_img2.png" />
<br /> <br />

左下の`+`を押してworkflowを追加します。

適当にBlank Flowを選択して名前を設定します。


<img src="/images/2014/2014_04_20_img6.png" />
<br /> <br />

右上`+`を押して、Triggers > HotKeyを選択して好きなホットキーを登録します。

<img src="/images/2014/2014_04_20_img3.png" />
<br /> <br />

さらに、右上`+`を押して、Actions > Run NSAppleScriptを選択してAppleScriptを登録します。

<img src="/images/2014/2014_04_20_img4.png" />
<br /> <br />

### Finderを開く場合
~~~applescript
on alfred_script(q)
  -- your script here

tell application "Finder"
   if frontmost of process "Finder" then
      set the visible of process "Finder" to false
   else
      activate
   end if
end tell

end alfred_script
~~~

### Path Finderを開く場合
~~~applescript
on alfred_script(q)
  -- your script here

    tell application "Finder"
       if frontmost of process "Path Finder" then
          set the visible of process "Path Finder" to false
          return
       end if
    end tell

    tell application "Path Finder"
        activate
    end tell

end alfred_script
~~~

最後に登録したホットキーとアクションを関連付けます。

Windows歴が１０年を超えているので`Command + E`がしっくりきます。

<img src="/images/2014/2014_04_20_img7.png" />
<br /> <br />

これで完了です。`Command + E`でFinderのウインドウがアクティブになり、もう一度`Command + E`でFinderが隠れると思います。

ちなみに`Ctrl + F3 Enter` でドックのショートカットを利用してFinder呼び出しできますが、キーの位置が離れていて押しづらい上に２段階という点で使っていません。

Alfredのworkflowは大変便利です。[ここのサイト](http://blog.ruedap.com/2013/10/30/alfred-workflow-for-front-end-developers)で購入を決めました。

ﾜｰｲ便利！