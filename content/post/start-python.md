+++
date = "2015-08-08T16:33:31+09:00"
draft = false
title = "Pythonをはじめたので環境設定のメモ"
eyecatch = "python.png"
tag = "python"

+++

新しい言語を始めるときは環境設定が一番面倒くさい。Pythonって2系と3系どっちやればええねんって感じがする。

とりあえず、諸々のインストールから。

# Python インストール

```bash
$ yaourt -S python # Python2.x は python2
```

これでPython3.x が入る。

PythonにはRubyでいうところの ```gem``` 的な ```pip``` というものがあるらしいのでそれも一緒にインストールしておく。

```bash
$ yaourt -S python-pip
```

あと ```virtualenv``` っていうやつも入れるらしい。 ```Bundler``` 的なものかな？ ```virtualenv``` のラッパーの ```virtualenvwrapper``` も一緒にさっき導入した ```pip``` を使ってインストールする。

```bash
$ sudo pip install virtualenv virtualenvwrapper
```

これをインストールしたら ```.zshrc``` に以下を書いて、パスを通してスクリプトを読み込むようにしておく。

```bash
if which virtualenvwrapper.sh > /dev/null 2>&1 ; then
  export WORKON_HOME=$HOME/.virtualenvs
  source `which virtualenvwrapper.sh`
fi
```

これで ```virtualenv``` が使えるようになったらしい。

環境を作るには ```mkvirtualenv``` , 環境を切り替えるには ```workon``` , デフォルトにするなら ```deactivate``` を使えばいいらしい。
一覧は ```lsvirtualenv``` , 削除は ```rmvirtualenv``` 。 他のコマンドについては[ここ](https://virtualenvwrapper-docs-ja.readthedocs.org/en/latest/command_ref.html#id2)を参照すると良さそう。

# Vim の設定
だいたいインストールしなければいけないものはインストールし終わったので、Vimで快適にPythonを書くための設定を行う。設定したいと思っているのは以下の項目。

- シンタックスチェック
- コード補完
- コーディング規約チェック

## シンタックスチェック

シンタックスチェックには ```flake8``` を利用する。

```bash
$ sudo pip install flake8
```

で, ```flake8``` をインストールできたので、あとは設定を書く。

```vimscript
let g:syntastic_python_checkers = ["flake8"]
```

## コード補完

補完には ```jedi-vim``` を利用する。

```vimscript
NeoBundle "davidhalter/jedi-vim"
```

デフォルトの設定では使いづらいところがあるので設定を書いておく。
ここを参照した。 [[vim]python補完プラグイン「jedi-vim」を快適にする方法] (http://dackdive.hateblo.jp/entry/2014/08/13/130000)

```vimscript
autocmd FileType python setlocal completeopt-=preview
autocmd FileType python setlocal omnifunc=jedi#completions
let g:jedi#auto_vim_configuration = 0
if !exists('g:neocomplete#force_omni_input_patterns')
  let g:neocomplete#force_omni_input_patterns = {}
endif
let g:neocomplete#force_omni_input_patterns.python = '\h\w*\|[^. \t]\.\w*'
```

あとは ```jedi``` プラグインの中で

```bash
$ git submodule update --init
```

を実行すると使用できるようになる。

## コーディング規約チェック

```pep8``` 準拠のコードを書くように指摘してもらうようにする。
保存時にチェックしてもらうために, ```andviro/flake8-vim``` を入れる。
設定も書いておく。

```vimscript
NeoBundle "andviro/flake8-vim"
let g:PyFlakeOnWrite = 1
let g:PyFlakeCheckers = 'pep8'
```

あとは ```pep8``` 基準のインデントするようにしたいので, ```hynek/vim-python-pep8-indent``` もインストールする。

```vimscript
NeoBundle "hynek/vim-python-pep8-indent"
```

## その他

```vim-virtualenv``` っていう ```virtualenv``` をべんりに扱えるプラギンがあるらしいのでそれも入れておく。

```vimscript
NeoBundle "jmcantrell/vim-virtualenv"
```

# まとめ
やっぱり新しい言語を始めるときは環境設定が面倒くさいなぁ。PythonはRubyより厳格で,書き方が限られていて好き。これでようやく[言語処理100本ノック](http://www.cl.ecei.tohoku.ac.jp/nlp100/) を始められる。
