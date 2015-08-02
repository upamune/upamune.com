+++
date = "2015-07-18T00:42:24+09:00"
draft = false
title = "Vimのコマンドライン上でEmacs風の動作をさせる"
eyecatch = "vim.png"

+++

コマンドライン上で長めのコマンドを入力したときに先頭にパッと飛ぼうと思ってCtrl-Aを押しても飛べなかったのでその設定をメモ。


```vim
cnoremap <C-A> <Home>
```

これでCtrl-Aで先頭に飛べるようになる。デフォルトでCtrl-Eで末尾には飛べるのでちょっと謎。

---
追記

Ctrl-k も使いたかったのでこの設定も追記した。ちなみにカーソルから行末まで削除。

```vim
cnoremap <C-K> <C-\>e<SID>KillLine()<CR>
function! <SID>KillLine()
  call <SID>saveUndoHistory(getcmdline(), getcmdpos())
  let l:cmd = getcmdline()
  let l:rem = strpart(l:cmd, getcmdpos() - 1)
  if ('' != l:rem)
    let @c = l:rem
  endif
  let l:ret = strpart(l:cmd, 0, getcmdpos() - 1)
  call <SID>saveUndoHistory(l:ret, getcmdpos())
  return l:ret
endfunction
let s:oldcmdline = [ ]
function! <SID>saveUndoHistory(cmdline, cmdpos)
  if len(s:oldcmdline) == 0 || a:cmdline != s:oldcmdline[0][0]
    call insert(s:oldcmdline, [ a:cmdline, a:cmdpos ], 0)
  else
    let s:oldcmdline[0][1] = a:cmdpos
  endif
  if len(s:oldcmdline) > 100
   call remove(s:oldcmdline, 100)
  endif
endfunction
```
[vim-emacscommandline](https://github.com/houtsnip/vim-emacscommandline)
VimScript書けないのでここから拝借した。このプラグインいれてもいいのだけれど、この2つで十分なので。
