---
layout: post
title: "Alfredでintellijのプロジェクトを素早く開く"
modified: 2014-05-10 09:45:39 +0900
tags: [intellij,alfred,ruby]
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---

Alfredが便利すぎて感動する日々です。
Powerpack持ってない人は今すぐ買って使ってみてください。この便利さに驚くはずです。

[http://www.alfredforum.com/forum/3-share-your-workflows/](http://www.alfredforum.com/forum/3-share-your-workflows/)

このサイトでは、多くのAlfred workflowを見つけることができます。
何が出来るのか見てみるといいかと思います。


私は、開発でintellijを活用していまます。
プロジェクトをいくつも開く場合が多々あるので、Alfredで開けるようにしました。
というか友人に誘発されて作りました。

rubyで書きましたが、phpやbashなんでもいけるみたいですね。

~~~ruby
Alfred.with_friendly_error do |alfred|
  fb = alfred.feedback

  target = ARGV[0]
  query = ARGV[1]

  item = {
      :uid      => "",
      :title    => '',
      :subtitle => "intellij Project",
      :arg      => '',
      :valid    => "yes",
  }

  files = Dir.glob(target+"/**/.idea")
  files.each do |path|
    if File.directory?(path) && path.include?('.idea')
      dir = File.dirname(path)
      pname = File.basename(dir)
      item[:arg] = dir
      item[:title] = pname
      if ARGV.length > 1
        if pname.include?(query)
          item[:subtitle] = 'intellij Project query: ' + query
          fb.add_item(item)
        end
      else
        fb.add_item(item)
      end
    end
  end

  puts fb.to_xml
end
~~~

[ダウンロード](https://github.com/kokikoki/alfred2-search-intellij-projects/raw/master/search-for-intellij-projects.alfredworkflow)

ﾊｧ、ｱﾙﾌﾚｯﾄﾞ便利ﾀﾞｧ