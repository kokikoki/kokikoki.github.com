---
layout: post
title: "jekyllのローカル確認時とデプロイ時で_config.ymlを分ける"
modified: 2014-04-07 15:06:53 +0900
tags: [jekyll]
image:
  feature:
  credit: 
  creditlink: 
comments: true
share: true
---

ローカルホストで確認してから、公開サーバーにデプロイする場合に設定ファイルを分けたい場合があると思います。
その場合に設定ファイルを複数ファイル指定で読み込みことができます。

<http://jekyllrb.com/docs/configuration/>

Build Command Optionsに書いてある通り `--config` オプションを指定してカンマ区切りで複数ファイルを指定できます。

#### ローカルで確認する場合

ローカルホストで確認用に_config.ymlとは別に設定を上書いた設定ファイルを用意します。
ここでは、_config_local.ymlとします。

~~~ bash
url: http://localhost:8888
host: localhost
port: 8888
~~~

jk_serve.shを作成します。
ここで、`--config` 指定をして複数ファイルしています。

~~~ bash
jekyll serve --config ./src/_config.yml,./src/_config_local.yml --watch --source ./src/ --destination ./html
~~~

#### デプロイ時の場合

rake ファイルに下記のようなrsyncを書いてデプロイしています。

~~~ ruby
task :deploy do
  sh "jekyll build --destination ../html --source ./"
  sh 'rsync -e "ssh -p port" -avz --delete ../html/ user@domain.com:/var/www/html/blog/'
end
~~~

