+++
date = "2015-07-05T19:24:06+09:00"
draft = false
title = "IntelliJでGoを書く"
eyecatch = "golang.png"

+++


IntelliJ IDEAでGoを書く環境を整えようと思ったのだけれど、普通にプラグイン検索してもヒットしなかったのでメモ。ちなみにOSはArchLinux。

基本的に[ここ](http://stormcat.hatenablog.com/entry/2015/04/13/123000)を参照すればいいのだけど、Goをbrewではなくてyaourtでインストールしたので、SDKの設定をするときにどこにPATHを通せばいいのか迷った。
おそらく```GOROOT```を指定すればいいのだけれど、```GOROOT```を設定していないので、どこが```GOROOT``` になっているか分からなかったので調べた。

```bash
$ go env GOROOT
/usr/lib/go
```
となっていたので、ここのディレクトリを指定するとうまくいった。あとは```GOPATH``` も訊かれるので設定しておく。これでIntelliJで最低限Goを書ける環境は整った。

保存時に自動的に```gofmt```かけてほしいなぁ...
