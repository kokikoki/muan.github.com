---
layout: post
title: "人感センサー+シリアルカメラ+Google Cloud Vision APIで作る<br/>思い出アルバム 組み立て編"
modified: 2017-05-22 20:31:58 +0900
tags: []
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---

![omame_camera](/images/2017/05/IMG_0711.jpg)

Arduino制作の練習です。<br />
タイトルの通り人感センサーとカメラを組み合わて作りました。
思い出写真を蓄積していつでも振り替えられるようにしています。<br />
長い期間運用して振り返ることで、こんな時期もあったなぁという気分になりたいです(*^^*)

# 大まかなフロー

- だらけている僕や彼女や猫が動く
- 人感センサーが感知
- 1.5秒後に撮影
- JPEGデータを自宅サーバーへPOST送信
- railsサーバーで受信してDBへ格納
- JPEGデータをGoogle Cloud Vision APIへ送信
- APIレスポンスをDBへ格納
- お気に入り登録機能や絞り込み検索で思い出を振り返る
- [Twitter](https://twitter.com/omame_camera)に感情の情報をツイートする


# Arduinoでハードウェアを作る

まずは必要なものを集めます。
カメラは１個4000円もするので壊さないように気をつけます。私は配線を間違えて、１個お亡くなりになりました。
壊れる時は煙が上がって一瞬ですね＼(^o^)／

## 集めるもの

- [ESPr Developer（ESP-WROOM-02開発ボード）](https://www.amazon.co.jp/gp/product/B017U73IFE/ref=oh_aui_detailpage_o01_s00?ie=UTF8&psc=1)
- [e-auto fun汎用スイッチング式ACアダプター 最大出力6W 出力プラグ外径5.5mm(内径2.1mm)PSE取得品 (1A, 6V) RC-0610](https://www.amazon.co.jp/gp/product/B06WGR6FC8/ref=oh_aui_detailpage_o02_s00?ie=UTF8&psc=1)
- [普通のブレッドボード ](https://www.switch-science.com/catalog/313/)  
- [GROVE - シリアルカメラモジュールキット ](https://www.switch-science.com/catalog/1626/)  
- [焦電型赤外線センサーモジュール（焦電人感センサ）](http://akizukidenshi.com/catalog/g/gM-09627/)
- [ＬＸＤＣ５５使用ＤＣＤＣコンバータキット（３．３Ｖ）](http://akizukidenshi.com/catalog/g/gK-09980/)
- [ＬＸＤＣ５５使用ＤＣＤＣコンバータキット（５Ｖ）](http://akizukidenshi.com/catalog/g/gK-09981/)
- [ブレッドボード用ＤＣジャックＤＩＰ化キット](http://akizukidenshi.com/catalog/g/gK-05148/)

※ ワイヤーなど細かいものは含まれていません

ハードを作るのは初めてなので、配線図とか書けないのでありません(*´ω｀*)<br/>

## 電源に関して

ACアダプターは、5Vギリギリの6Vにしました。12VのACアダプターを使っていましたが、DCDCコンバーターが高温になるので変更しました。
電流に関しては、ESP-WROOM-02が一時的に高い電流を要するみたいなので1Aのアダプタになっています。<br />
USB接続での電源供給ではうまく動かないということ知らずに作っているとハマりますし、ハマりました(泣)

やってみてわかったことなのですが、ハードを組み上げることで注意しなければならないのは、電圧と電流です。
人感センサーとシリアルカメラは、電源電圧が5VなのでDCDCコンバーター(5V)で減圧します。<br />
また、ESP-WROOM-02は3.3VなのでこちらもDCDCコンバーター(3.3V)で減圧します。

## Arduinoのソースコード
Arduino言語のソースコードはかなり汚れているのでキレイにしてその内githubにアップします。


# railsで自宅サーバー
プライベートな写真は手元に置いておきたいのと外部に置く理由もないので自宅サーバーです。

Rails5のAPIモードで実装しました。<br/>



# vue.jsとvue-materialでフロントエンド
vue.jsで実装し、vue-materialでマテリアルデザインを適用しています。<br />
vue-cliを使えばwebpackの設定からする必要はなく
```
$ vue init webpack my-project
```
を叩くだけで実装をすぐに始めることができます。

機能としては、下記の一覧のようにしました。

- お気に入り登録できる
- 説明文の登録できる
- カレンダーによる日付の絞り込みができる
- 絞り込み検索
    - お気に入り
    - 説明文がある
    - 楽しい
    - 悲しい
    - 驚き
    - 帽子
    - ぼやけている
    - 肌の露出
    - 顔認識がない

![vue](/images/2017/05/2017-05-23 9.15.20.png)

Google Cloud Vision APIで顔認識したbounds情報で顔に四角を描画します。<br/>
下記のような検知レベルによって色の濃さを変更しています。

- VERY_LIKELY	非常に高いレベル
- LIKELY	高いレベル
- POSSIBLE	そうだと言えるレベル
- UNLIKELY	低いレベル
- VERY_UNLIKELY	非常に低いレベル
- UNKNOWN

実際の写真は恥ずかしいので灰色で塗りつぶしています(*´ω｀*)<br/>
`JOY: VERY_LIKELY`ということで楽しい感情を濃い赤の枠で示されています。

![vue](/images/2017/05/2017-05-23 11.03.53.png)

# バックエンドとフロントエンドのソースコード

[https://github.com/kato-condesire/pir_camera](https://github.com/kato-condesire/pir_camera)

# 完成品

雑に箱につっこんで、
[ボビーノ スクリーンシェルフ](https://www.amazon.co.jp/gp/product/B014GRULXI/ref=oh_aui_detailpage_o02_s00?ie=UTF8&psc=1)でテレビの上に設置しました。<br/>
今回使ったシリアルカメラは、動作時に赤いLEDが光ります。最初は気になりましたが今は気になりません。<br/>
同棲中の彼女は大雑把な性格の人なので、特に意識しないと言ってくれています(*´∀｀*)

今回のカメラの[Twitter](https://twitter.com/omame_camera)に怒り感情と悲しみ感情が出てるのですが、喧嘩したわけではありません。きっときっと。


![omame_camera](/images/2017/05/IMG_0868.jpg)


# 苦労した点

### ハードウェアの組み立てが難しい
工業高校出身なので電気科目を一通り勉強しているはずなのですがまったく覚えていませんでした。ただ、はんだゴテは体が覚えていてキレイにできました。<br />
配線や電圧電流に関して間違っているとエラーも何もかも応答がないので判断が難しいです。
JPEGをPOST送信するサンプルが多くないので実装も大変です。
いくつかサンプルを試しましたが、動作しないものもあるのでトライアンドエラーでやっていくしかありませんでした。<br />

### シリアルカメラが３回に１回くらいしかPOST送信してくれない
原因がわからないまま放置しています(*´艸｀*)カメラは動作しているようですが、POST送信がされていないようです。
電圧の問題なのか検討もつきません。。誰か教えて下さい。。。

### Google Cloud Vision APIがシリアルカメラのJPEGを認識してくれない
シリアルカメラで撮影したJPEGデータの下部が灰色になり破損しているためだと推測されます。これも何故灰色に破損するのかわかっていませんが
見られないほどにはならないです。<br/>
ImageMagickをRailsから使えるRMagickで、jpeg -> png 変換をして対応しています。

### rails自宅サーバーのIPアドレスがDHCPなので時々変更されてしまう
MacBookProによる自宅サーバーなので、`macname.local` でアクセスできるとおもきやArduinoからは名前解決できないようでした。
解決方法としては、ルーターのDHCP予約設定をして、サーバーのIPアドレスに割当します。


# 次回の運用編に続きます
2017年５月22日の時点で、設置してほぼ２ヶ月になります。<br/>
IOT用のシリアルカメラなので画質が悪いということもあり、あまり感情の読み取りは正確ではないように思います。<br/>
写真の総数と顔認識数など出して、目視でGoogle Cloud Vision APIがどの程度正確なのか確認して行きたいと思います。

[> 運用編](/2017/05/24/omame-camera-unyo)


