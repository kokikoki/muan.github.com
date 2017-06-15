---
layout: post
title: "人感センサー+シリアルカメラ+Google Vision APIで作る<br/>思い出アルバム 運用編"
modified: 2017-05-24 10:27:32 +0900
tags: []
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---

![omame_camera](/images/2017/05/IMG_0868.jpg)

[前回](/2017/05/22/omame-camera)に引き続き、今回は今まで取れた写真の総数などを見ていきます。

# 画像解像度
後から知ったことですが、今回のシリアルカメラは`640 x 480`での撮影が限界でしたが、Google Cloud Vision APIの顔認識に関しては `1600 x 1200`が推奨のようです。<br />
なので、認識の精度が悪いと感じるのも無理はありません。。

# 約2ヶ月間で撮れた写真

 |写真総数|顔認識数|
 |---:|---:|
 |1753|739|
 
<br />
 
 |項目|楽しい感情|悲しい感情|驚き感情|
 |:---|---:|---:|---:|
 |総数|26|8|4|
 |目視|9|4|1|
 |割合|34%|50%|25%|
 
写真を目視して確認しましたが、正しいと思われる表情は半分以下ということになりますね。
やっぱり解像度が悪いということでしょうか。

歯を磨いている顔で驚きの認識だったり、ソファで横になっている顔だと顔認識されなかったりしています。<br />
 
# Google Cloud Vision API について
[価格](https://cloud.google.com/vision/pricing?hl=ja)にあるように月1000回を超えると料金が発生するようです。

1日30枚をAPIに送信すると月900枚になるので、そのくらい撮れるように人感センサーを調整しています。<br />
今回使っているシリアルカメラは状態保持時間を可変抵抗器で変更できるので、5分間ほど保持できるように変更しました。


# まとめ
今回パッと思いつきでこのようなものを作りました。どこで役に立つんだ感は否めないですが実際に手を動かすことで、得られるものは非常に多かったと思っています。<br />
写真総数1753枚のうち思い出に残りそうな写真は、たった2ヶ月間で30枚以上になります。日常のいろいろな行動であったり、おかしなポーズやいろいろな表情の写真が蓄積されています。<br />
普通写真は、撮られることを意識して写真として残りますが、今回のような人感センサーを使って意識していない日常の一瞬を記録として残すということは、センサーが安く手に入るからこそのものだと思っています。<br />

センサーによって意識せずに記録が残るということは、すでに多くあります。<br />
スマホを持っているだけで位置情報がトラッキングされ、歩数やカロリーが記録されるようになっています。ウェアラブルデバイスとして呼吸を記録するものや、姿勢を記録するものまであります(実際に使いました)。今後も意識することなくいろいろな情報がトラッキングされ、様々なシーンで利用されていくと予想されています。<br />
センサーテクノロジーのおかげでセルフトラッキングが容易になり、そのトラッキング情報から定期的に改善の提案を受けることになるでしょう。
指数関数的に進化するテクノロジーをこの目で見て触れる時代に生まれて、そしてそれに興味を持っていて本当に幸せです。


<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=virgoboy-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=4140817046&linkId=b5047e9eb6e4830c0ce528adf44cfc4c"></iframe>