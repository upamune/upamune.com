+++
date = "2015-07-01T08:53:12+09:00"
draft = false
eyecatch = "wercker.jpg"
title = "HugoをWerckerでgithub.ioに自動デプロイさせた時にハマったこと"
+++


このブログはHugoを使っています。そして、GithubにPushしたら自動でデプロイされるようになっています。それを実現しているのが, [Wercker](http://wercker.com/)というWebサービスです。基本的には [Hugoのインストールから自動デプロイまで](http://motomizuki.github.io/blog/2015/02/28/hugodeploy/)を参考に進めていけば良いのですが、いくつかハマったことがあったのでメモしておきます。

## Dockerを無効にする
```box:wercker/default``` はDockerを使わないので、無効にしておきます。
デフォルトだとDockerが有効になっているのでチェックを外しておきます。

![](http://f.st-hatena.com/images/fotolife/j/jajkeqos/20150701/20150701094913.png)

## テーマは --recursive をつけてクローンする
私は、[aglaus](https://github.com/dim0627/hugo_theme_aglaus)というテーマを使用しているのですが、 ```git clone``` するときに ```--recursive``` オプションをつけてクローンしないとビルド失敗してしまいました。ですが、 ```git clone --recursive git@github.com:dim0627/hugo_theme_aglaus.git``` という風にやると成功したので、つけたほうが良さそうです。

## 最新のhugo-buildを使用する
他人の ```wercker.yml``` では hugo-build がバージョン指定されていることがあります。しかし、古いバージョンだとうまく動かないことがあるので、うまくbuildできない場合はhugo-buildの最新のものを使うとよいです。自分の場合、Hugoのテーマの Url を推奨されるURLに書き換えたらエラーが出てビルドできなかったのですが、最新版にするとちゃんとビルドできました。

## wercker.yml の インデントは半角スペース4つ
最初に貼った記事でも書かれていますが、 wercker.yml のインデントは半角スペース4つです。いつも自分はインデントを半角スペース2つで使用しているので、ハマりました。

## 最終的なwercker.yml

主にハマったところは列挙しておいたので、最後に ```wercker.yml``` の内容を貼っておきます。動作したバージョンで固定しておくのが良さそうですが、まだやってません...

```yml
box: wercker/default
build:
    steps:
        - arjen/hugo-build:
            theme: aglaus
            flags: --buildDrafts=true
deploy:
    steps:
        - lukevivier/gh-pages:
            token: $GIT_TOKEN
            basedir: public
            repo: upamune/upamune.github.io
```

