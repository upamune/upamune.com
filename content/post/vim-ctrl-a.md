+++
date = "2015-07-18T00:42:24+09:00"
draft = false
title = "Vimのコマンドライン上でCtrl-Aを使って先頭に飛ぶ"
eyecatch = "vim.png"

+++

コマンドライン上で長めのコマンドを入力したときに先頭にパッと飛ぼうと思ってCtrl-Aを押しても飛べなかったのでその設定をメモ。


```vim
cnoremap <C-A> <Home>
```

これでCtrl-Aで先頭に飛べるようになる。デフォルトでCtrl-Eで末尾には飛べるのでちょっと謎。


