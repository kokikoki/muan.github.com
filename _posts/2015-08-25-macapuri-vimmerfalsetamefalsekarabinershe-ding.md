---
layout: post
title: "【Macアプリ】VimmerのためのKarabiner(旧KeyRemap4MacBook)設定"
modified: 2015-08-25 09:35:44 +0900
tags: [mac,vimmer]
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---

Emacsいいですよね。Vimもいいですね。
そもそもなんで使い始めたかというと矢印キーまで手を移動させるのが面倒くさいからです。

ホームポジションのままカーソル移動したい。そうしたらEmacsキーバインドかVimキーバインドしかないです。
Emacsキーバインドで10年くらい頑張ってましたが、小指が痛いので止めました。
なので、最近はVimキーバインドで開発しています。

Vimには2種類のモードを切り替えて編集します。カーソル移動するときはノーマルモードを使って、入力する時はインサートモードです。
ここで、日本語を入力する時にややこしくなります。

ノーマルモードからインサートモードに切り替えてさらに、IMEをONにしないとなりません。そして、入力が終わったらIMEをOFFにしてから
ノーマルモードへ切替を行います。

とても面倒くさいですね。
せめてノーマルモードへ切替と同時にIMEもOFFにしたい。

そんな願いを叶えてくれるのが、実現してくれるアプリが[Karabiner](https://pqrs.org/osx/karabiner/index.html.ja)です。


# Karabinerとは

Karabinerは、キーボードの入力をカスタマイズしてくれるアプリケーションです。
今回は、デフォルトで用意されている設定項目は割愛します。

自分で詳細にカスタマイズするために用意されているprivate.xmlファイルについてやっていきます。

## Karabinerのインストール

[https://pqrs.org/osx/karabiner/index.html.ja](https://pqrs.org/osx/karabiner/index.html.ja)
からダウンロードでインストールします。

## Karabinerのprivate.xmlを開く

<img src="/images/2015/08/karabiner_icon.png" alt="" /><br />
こんなアイコンがメニューバーに追加されているので、クリックして設定画面を開きます。

設定画面の`Misc & Uninstall`タブを開ます。

![](/images/2015/08/2015-08-25_9_59_11.png)

`Open Private.xml`をクリックして、private.xmlファイルを開きます。

![](/images/2015/08/2015-08-25_10_00_59.png)

xmlファイルを開くと以下のようになっています。

~~~xml
<?xml version="1.0"?>
<root>
</root>
~~~

このファイルを編集していきます。


## private.xmlの編集

まず、アプリケーションごとの設定を行いますのでアプリケーションを登録していきます。

root内に追記します。

~~~xml
    <appdef>
        <appname>iTerm</appname>
        <equal>com.googlecode.iterm2</equal>
    </appdef>
    <appdef>
        <appname>MACVIM</appname>
        <equal>org.vim.MacVim</equal>
    </appdef>
~~~

`<appname>`は任意の識別できる文字列を設定します。

`<equal>`はアプリケーションのBundle Identifierというのを設定します。
<br />
<br />

### Bundle Identifierの取得

先ほどの`Misc & Uninstall`タブを開いて`Launch EventViewer` をクリックします。

![](/images/2015/08/2015-08-25_10_01_59.png)

`App`タブを開きます。

`EventViewer`を開いた状態でBundle Identifierを知りたいアプリケーションにフォーカスします。

![](/images/2015/08/2015-08-25_10_20_25.png)

別のアプリケーションにフォーカスをあてると、
`EventViewer`のApplicationsの一覧にフォーカスしたアプリケーションが表示されますので
`Application Bundle Identifier`の項目をコピーします。

`copy to pasteboard`をクリックしてクリップボードにコピーします。

![](/images/2015/08/2015-08-25_10_23_13.png)

こんな情報が得られます。

    BundleIdentifier: org.vim.MacVim
    WindowName:       [No Name] - VIM
    UIElementRole:    AXWindow

コピーした内容から`BundleIdentifier:`を抜粋して編集中のxmlファイルのequalタグを設定できます。


アプリケーションの定義が終わったら、キー入力の定義を行います。


### キー入力の定義設定

private.xmlに追記します。

~~~xml
    <list>
        <item>
            <only>MACVIM,iTerm2</only>
            <name>Leave Insert Mode with EISUU (vim keybind apps)</name>
            <identifier>private.vim_keybind_apps_esc_with_eisuu</identifier>
            <autogen>--KeyToKey-- KeyCode::ESCAPE, KeyCode::ESCAPE, KeyCode::JIS_EISUU</autogen>
            <autogen>--KeyToKey-- KeyCode::C, VK_CONTROL, KeyCode::C, VK_CONTROL, KeyCode::JIS_EISUU</autogen>
            <autogen>--KeyToKey-- KeyCode::BRACKET_LEFT, VK_CONTROL, KeyCode::BRACKET_LEFT, VK_CONTROL, KeyCode::JIS_EISUU</autogen>
            <autogen>--KeyToKey-- KeyCode::S, VK_COMMAND, KeyCode::S, VK_COMMAND, KeyCode::ESCAPE, KeyCode::JIS_EISUU</autogen>
        </item>
    </list>
~~~

`<list>`に`<item>`が内包されています。`<item>`は複数設定することができます。

`<only>`は、`<appdef>`で設定した`<appname>`をカンマ区切りで設定します。

`<name>`と`<identifier>`は任意の分かりやすい文字列を設定します。

`<autogen>`の`--KeyToKey--`シンタックスでキーの入れ替えを行います。

今回は、`Esc`キーを押したと同時にIMEをOFFに設定したいので下記のような設定になります。


    <autogen>
        --KeyToKey--
        KeyCode::ESCAPE,
        KeyCode::ESCAPE, KeyCode::JIS_EISUU
    </autogen>
    
`Esc`入力時は、`ESCAPE`入力後`JIS_EISUU`を入力する


<br />
同じ要領で、`Ctrl+C`と`Ctrl+[`入力時の設定も行います。

    <autogen>
        --KeyToKey--
        KeyCode::C, VK_CONTROL,
        KeyCode::C, VK_CONTROL, KeyCode::JIS_EISUU
    </autogen>
    
`Ctrl+C`入力時は、`Ctrl+C`入力後`JIS_EISUU`を入力する

    <autogen>
        --KeyToKey--
        KeyCode::BRACKET_LEFT, VK_CONTROL,
        KeyCode::BRACKET_LEFT, VK_CONTROL, KeyCode::JIS_EISUU
    </autogen>

`Ctrl+[`入力時は、`Ctrl+[`入力後`JIS_EISUU`を入力する

## private.xmlファイルの全体

今回編集したprivate.xmlは、下記のようになります。

~~~xml
<?xml version="1.0"?>
<root>

    <appdef>
        <appname>iTerm2</appname>
        <equal>com.googlecode.iterm2</equal>
    </appdef>
    <appdef>
        <appname>MACVIM</appname>
        <equal>org.vim.MacVim</equal>
    </appdef>
    
    <list>

        <item>
            <only>iTerm2,MACVIM</only>
            <name>Leave Insert Mode with EISUU (vim keybind apps)</name>
            <identifier>private.vim_keybind_apps_esc_with_eisuu</identifier>
            <autogen>--KeyToKey-- KeyCode::ESCAPE, KeyCode::ESCAPE, KeyCode::JIS_EISUU</autogen>
            <autogen>--KeyToKey-- KeyCode::BRACKET_LEFT, VK_CONTROL, KeyCode::BRACKET_LEFT, VK_CONTROL, KeyCode::JIS_EISUU</autogen>
            <autogen>--KeyToKey-- KeyCode::C, VK_CONTROL, KeyCode::C, VK_CONTROL, KeyCode::JIS_EISUU</autogen>
            <autogen>--KeyToKey-- KeyCode::S, VK_COMMAND, KeyCode::S, VK_COMMAND, KeyCode::ESCAPE, KeyCode::JIS_EISUU</autogen>
        </item>

    </list>
</root>
~~~


## private.xmlの反映

設定画面の`Change Key`タブを開きます。

`Reload XML`をクリックして、編集した`private.xml`をロードします。

するとremapping一覧に先ほど任意の名前を付けた設定が表示されると思いますので、チェックボックスにチェックを付けます。

![](/images/2015/08/2015-08-25_11_00_39.png)

以上で、完了です。

さっそく設定したアプリケーションで試してみましょう！

<br />
<br />
[Karabiner](https://pqrs.org/osx/karabiner/index.html.ja)
