+++
date = "2015-07-22T22:11:38+09:00"
draft = false
title = "Macクリーンインストールした時のメモ"
toc = true

+++


3ヶ月に1回くらいMacをクリーンインストールして再設定するのでその時のためにメモ。

インストールしたらAppStoreを開いてアップデートがないかどうか確認してから以下に進む。あればアップデートする。


# Homebrew インストール
まずHomebrewのインストールをする。その時に一緒にコマンドラインツールもインストールする。

```bash
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

これでHomebrewを導入できたら必要なツールをインストールする。

```bash
$ brew install clang-format ctags docker gcc git go heroku-toolbelt ruby the_silver_searcher tig tmux tree zsh
$ brew install vim --with-lua --override-system-vi --HEAD
```

Go関連のツールを導入する。pecoもHomebrewではなくてgo get で導入する。
以下のツールをインストールする。

```bash
$ go get -u golang.org/x/tools/cmd/goimports
$ go get -u golang.org/x/tools/cmd/godoc
$ go get -u golang.org/x/tools/cmd/vet
$ go get -u golang.org/x/tools/cmd/cover
$ go get -u github.com/nsf/gocode
$ go get -u github.com/golang/lint/golint
$ go get -u code.google.com/p/rog-go/exp/cmd/godef
$ go get -u github.com/jstemmer/gotags
```

# シェル周りの設定

## ログインシェルの変更

これでHomebrewでインストールしたzshをログインシェルとして選択できるようにする。

```bash
$ echo `which zsh` | sudo tee -a /etc/shells
```

そしてログインシェルをzshに変更する。

```bash
$ chsh -s `which zsh`
```

## Vim と Zsh の設定
自分の場合はdotfiles にまとめてあるのでそのリポジトリをクローンしてくる。ので、GithubにSSH鍵を登録しておく。

```bash
$ ssh-keygen -t rsa
```
んで```~/.ssh/config```に

```
Host github.com
      User git
      Port 22
      Hostname github.com
      IdentityFile ~/.ssh/github
      TCPKeepAlive yes
      IdentitiesOnly yes
```

書いて```~/.ssh/github.pub``` をGithub側に登録すればよい。


```bash
$ git clone git@github.com:upamune/dotfiles.git
```

そして、dotfilesディレクトリに入って、```init.sh```を実行する。実行し終わったら、```exec zsh``` をする。そしてtmux上で ```C-t I``` を押してtmuxのプラグインをインストール。Vimを起動してプラグインをインストールする。

そしてここで1度(気分的に)再起動する。

# GUIアプリケーション
必要なGUIアプリケーションをインストールしていく。ここが一番面倒くさいところ。すべて手動でインストールする。brew-caskは嫌いなので使わない。太字になっているものはAppStoreからダウンロードする。

- 1Password
- Alfred
- Chrome
- Dropbox
- Firefox
- Fraise
- GoogleIME
- IntelliJ IDEA
- iTerm2
- Karabiner
- **Magnet**
- PhpStorm
- **Slack**
- **Tweetdeck**
- Vagrant
- Virtualbox
- WebStorm
- XtraFinder

# 設定

## 基本
まずバッテリーアイコンをクリックして割合(%)を表示するようにする。あとは自分はDockが画面の左側にあって自動で隠れるのが好きなのでその設定をしてクソみたいなアプリをDockから消す。

## セキュリティとプライバシー
スリープとスクリーンセーバの解除にパスワードを要求 →  すぐに
ファイアウォール →  オン

## キーボード
環境光が暗い場合にキーボードの輝度を調整 → 無効
修飾キー → CapsLockキー → Control キー

## トラックパッド
タップでクリック →　有効
副ボタンのクリック → 左下隅をクリック
調べる → 無効
3本指のドラッグ → 有効
軌跡の速さ → 最大
通知センター → 無効
アプリケーションExposé → 有効

## アプリケーション
Karabiner,Magnet,XtraFinderの設定をいい感じにする。Karabinerのxmlファイルはこれを使っている。
Vimのためのに使っている感じだ。あとは ```Use Japanese Keyboard as US Keyboard``` にチェックをいれて終わり。

```xml
<?xml version="1.0"?>
<root>
  <list>
    <item>
      <name>LeaveInsMode with EISUU(Terminal)</name>
      <identifier>private.app_terminal_esc_with_eisuu</identifier>
      <only>TERMINAL</only>
      <autogen>--KeyToKey-- KeyCode::ESCAPE, KeyCode::ESCAPE, KeyCode::JIS_EISUU</autogen>
      <autogen>--KeyToKey-- KeyCode::C, VK_CONTROL, KeyCode::C, VK_CONTROL, KeyCode::JIS_EISUU</autogen>
    </item>
    <item>
      <name>Escape to EISUU+Escape only terminal</name>
      <identifier>remap.jis_escape2eisuuAndEscape_terminal</identifier>
      <only>TERMINAL</only>
      <autogen>--KeyOverlaidModifier-- KeyCode::ESCAPE, KeyCode::JIS_EISUU, KeyCode::ESCAPE</autogen>
    </item>
  </list>
</root>
```


# 雑感
激自動化したい状態。最初の方はただスクリプト書くだけで良さそうだけどGUIアプリケーションのとこをどうにかしたい。とても面倒なので...でもCaskは使いたくないなぁという気持ち。うーん。。。
