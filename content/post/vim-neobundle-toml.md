+++
date = "2015-07-11T11:09:19+09:00"
draft = false
title = "NeoBundleのプラグインをTOMLで管理する"
eyecatch = "vim.png"

+++


# Why?
```.vimrc``` が肥大化してきて、分割したいなーと思っていたのだけれど, ```NeoBundle``` にはTOMLパーサが同梱されてるらしいのでプラグインだけTOMLファイルで管理することにした。あといい感じに遅延読み込みを設定して高速化させたかったってのもある。


# 設定
ShougoさんによるTOMLファイルのサンプルは[こちら](https://github.com/Shougo/shougo-s-github/blob/master/vim/rc/neobundle.toml)。

```
Note: TOML parser is slow.  You should use neobundle cache feature.
```

```NeoBundle``` のヘルプによるとTOMLパーサは遅いらしいのでキャッシュを利用するようにする。


こんな感じ

```vim
if neobundle#load_cache()
  NeoBundleFetch 'Shougo/neobundle.vim'
  call neobundle#load_toml('~/.vim/neobundle.toml')
  call neobundle#load_toml('~/.vim/neobundlelazy.toml', {'lazy' :1} )
  NeoBundleSaveCache
endif
```

あとは、TOMLファイルにプラグインを羅列していくだけ。TOMLのコメントは ```#``` です。

## neobundle.toml

```toml
# syntax {{{
[[plugins]]
repository = 'scrooloose/syntastic'

# appearance {{{
[[plugins]]
repository = 'itchyny/lightline.vim'

# complement {{{
[[plugins]]
repository = 'Shougo/neocomplete.vim'

# l10n {{{
[[plugins]]
repository = 'vim-jp/vimdoc-ja'

# unite {{{
[[plugins]]
repository = 'Shougo/unite.vim'

[[plugins]]
repository = 'Shougo/unite-outline'

# benry {{{

[[plugins]]
repository = 'tyru/caw.vim.git'

[[plugins]]
repository = 'kana/vim-submode'

[[plugins]]
repository = 'Shougo/vimproc.vim'

  [plugins.build]
    windows = 'tools\\update-dll-mingw'
    cygwin = 'make -f make_cygwin.mak'
    mac = 'make -f make_mac.mak'
    unix = 'make -f make_unix.mak'

# move {{{
[[plugins]]
repository = 'Lokaltog/vim-easymotion'

# search {{{
[[plugins]]
repository = 'haya14busa/incsearch.vim'
depends = 'osyo-manga/vim-anzu'

# color {{{
[[plugins]]
repository = 'w0ng/vim-hybrid'

[[plugins]]
repository = 'nanotech/jellybeans.vim'

[[plugins]]
repository = 'upamune/tomorrow-theme'

# optimize {{{
[[plugins]]
repository = 'Yggdroot/indentLine'

[[plugins]]
repository = 'bronson/vim-trailing-whitespace'

# git {{{
[[plugins]]
repository = 'lambdalisue/vim-gista'

[[plugins]]
repository = 'gregsexton/gitv'

[[plugins]]
repository = 'tpope/vim-fugitive'
```

## neobundlelazy.toml

```toml
# like ide {{{
[[plugins]]
repository = 'Shougo/vimfiler'
explorer = 1
mappings = '<Plug>'

[[plugins]]
repository = 'majutsushi/tagbar'
mappings = '<Plug>'

[[plugins]]
repository = 'thinca/vim-quickrun'
mappings = '<Plug>'

[[plugins]]
repository = 'Shougo/neosnippet'
insert = 1

[[plugins]]
repository = 'sjl/gundo.vim'
mappings = '<Plug>'

# search {{{
[[plugins]]
repository = 'rking/ag.vim'
mappings = '<Plug>'

# benrify {{{
[[plugins]]
repository = 'Shougo/vimshell.vim'
mappings = '<Plug>'

# ruby {{{
[[plugins]]
repository = 'marcus/rsense'
filetypes = 'ruby'

[[plugins]]
repository = 'supermomonga/neocomplete-rsense.vim'
filetypes = 'ruby'

[[plugins]]
repository = 'thinca/vim-ref'
filetypes = 'ruby'

[[plugins]]
repository = 'yuku-t/vim-ref-ri'
filetypes = 'ruby'

[[plugins]]
repository = 'tpope/vim-endwise'
filetypes = 'ruby'

# go {{{
[[plugins]]
repository = 'dgryski/vim-godef'
filetypes = 'go'

[[plugins]]
repository = 'vim-jp/vim-go-extra'
filetypes = 'go'

# clang {{{
[[plugins]]
repository = 'rhysd/vim-clang-format'
depends = 'kana/vim-operator-user'
filetypes = 'cpp'

# html {{{
[[plugins]]
repository = 'amirh/HTML-AutoCloseTag'
filetypes = 'html'

[[plugins]]
repository = 'hail2u/vim-css3-syntax'
filetypes = 'html'

[[plugins]]
repository = 'gorodinskiy/vim-coloresque'
filetypes = 'html'

[[plugins]]
repository = 'tpope/vim-haml'
filetypes = 'html'

[[plugins]]
repository = 'mattn/emmet-vim'
filetypes = 'html'

# toml {{{
[[plugins]]
repository = 'cespare/vim-toml'
filetypes = 'toml'

# scala {{{
[[plugins]]
repository = 'derekwyatt/vim-scala'
filetypes = 'scala'
```

# まとめ
TOMLファイルに移行させるのはほとんどコピペで ```s/NeoBundle /[[plugins]]\rrepository = /``` 的なことをするだけだったので楽だった。実際の起動時間は 246ms => 133ms になって **113ms** も速くなったので最高です。
