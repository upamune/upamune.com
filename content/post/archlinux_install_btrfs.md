+++
date = "2015-07-10T00:02:18+09:00"
draft = false
title = "ArchLinux をインストールする with Btrfs"
eyecatch = "archlinux.png"

+++

# インストール以前

[Arch Linux Downloads](https://www.archlinux.org/download/)から最新版のISOをダウンロードしてくる。そしてUSBメモリをさして焼けばいいのだけれど、自分は毎回以下のようにやっている。

```bash
$ lsblk #USBメモリを確認
$ dd bs=4M if=archlinux.iso of=/dev/sdX #lsblkで確認したデバイスに焼く
```

毎回のように ```dd``` の進捗を確認する方法を検索してしまうのでメモ。

```bash
$ sudo sh -c 'while true; do killall -USR1 dd; sleep 2; done'
```

こうすると2秒おきに進捗が見れるらしい。べんり。
[CentOS ddコマンド実行中に進捗情報を確認する方法](http://kaworu.jpn.org/kaworu/2012-11-29-1.php)

BSD系だと使えないので注意。


# ArchLinuxのインストール

## ネットワークに接続する
ネットワークに接続しないとなにも始まらないので接続。有線でも無線でも良い。
無線の場合は


```bash
# wifi-menu
```

で接続。

```bash
# ping 8.8.8.8
```

接続確認。

## ssh 接続する
そのまま設定を手打ちするのは面倒くさいので, sshで接続して設定をする。


### ホスト側

```bash
# sytemctl start sshd
# systmctl status sshd
# ip a
```

IPアドレスを確認しておく。

```bash
# passwd
```

そして、rootパスワードを設定しておきます。

### クライアント側

```bash
$ ssh root@IP_ADDR
```

ssh接続確認できたら、次に進む。

## パーティション
一番面倒くさい。

```bash
# cgdisk /dev/sda
```

こんなかんじにした。

```bash
NAME             MAJ:MIN  RM  SIZE    RO  TYPE
 sda
 │─sda1            8:1     0    512M   0   part
 ├─sda2            8:2     0      1G   0   part
 └─sda3            8:3     0  117.8G   0   part
```

### フォーマット

```bash
# mkfs.vfat -F32 /dev/sda1
# mkswap /dev/sda2
# mkfs.btrfs /dev/sda3
```

### マウント

```bash
# mount -o noatime,discard,ssd,autodefrag,compress=lzo,space_cache /dev/sda3 /mnt
# mkdir -p /mnt/boot/efi
# mount /dev/sda1 /mnt/boot/efi
```

## pacstrap
パーティションが終わったらあとは楽ちん。 ```pacstrap``` を使って必要なソフト群をインストールする。その前にミラーリストの順序を変更しておくと良い。


```bash
# vi /etc/pacman.d/mirrorlist # Japanを上に持ってくる
# pacstrap /mnt base base-devel grub efibootmgr dosfstools btrfs-progs lzo dialog wpa_supplicant openssh git vim zsh
```

そこそこ時間がかかるので待つ。

## 設定

### fstab

```bash
# genfstab -p /mnt >> /mnt/etc/fstab
# vim /mnt/etc/fstab #/dev/sda の形式ではなくて,UUID=の形式に置き換える
```

### 諸設定

```bash
# arch-chroot /mnt `which zsh`
# passwd
# echo HOSTNAME > /etc/hostname
# ln -s /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
# echo 'LANG=en_US.UTF-8' > /etc/locale.conf
# vim /etc/locale.gen # en_US.UTF-8 のコメントアウトを解除する
# locale-gen
# vim /etc/mkinitcpio.conf # HOOKSのfsck のあとにbtrfsを追加する
# mkinitcpio -p linux
# mkdir /boot/efi/EFI
# grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=Grub --recheck --debug
# grub-mkconfig -o /boot/grub/grub.cfg
# exit
# umount /mnt/{boot/efi,}
# reboot
```

ここまででArchLinuxのインストールは完了です。次は基本的な設定をしていきます


また先ほどの要領でssh接続して進める。

## pacman & yaourt
```/etc/pacman.conf``` を設定してmultilibを有効にしておく。

そしてこれも末尾に追記しておく。

```bash
[archlinuxfr]
SigLevel = Never
Server = http://repo.archlinux.fr/$arch
```

追記し終わったら、更新して ```yaourt``` を有効にしておく。

```bash
# pacman --sync --refresh yaourt
```

## ユーザー作成

```bash
# useradd -m -G wheel -s /bin/zsh upamune
# passwd upamune
# visudo # %wheel ALL=(ALL) ALL のコメントアウトを解除する
```

ユーザー作成したのでこれからはrootユーザーではなく作成したユーザーで作業していく。

# GUI構築

Xfce + SLiM です。

## SLiM 導入
```bash
$ yaourt -Sy xorg-server xorg-server-utils xorg-server-xephyr xorg-utils xterm xf86-video-intel slim archlinux-themes-slim slim-themes
$ vim /etc/slim.conf # login_cmd, daemon の行のコメントアウトを解除して、 themeも変更
$ systemctl enable slim.service
```

## Xfce 導入

```bash
$ yaourt -S xfce4 xfce4-goodies gamin
$ vim ~/.xinitrc # exec startxfce4 と書けば良い
```

## インストール後

### プログラミング言語系

```bash
$ yaourt -S clang go scala
```

### ツール
```bash
$ yaourt -S chromium dropbox gimp guake mikutter mercurial tig xsel tlp tp-smapi tpacpi-bat tmux the_silver_searcher unzip wpa_actiond
```

```wpa_actiond``` をインストールすることで ```netctl-auto``` が入るので、ネットワークに自動接続するように設定する。

```bash
$ systemctl enable netctl-auto@[INTERFACE].service
```

さらに ```tlp``` を有効にするためにも設定しておく。

```bash
systemctl enable tlp.service
```

### 開発環境

```bash
$ yaourt -S ctags typesafe-activator intellij-idea-ultimate-edition
```

### フォント

```bash
$ yaourt -S ttf-koruri ttf-ricty otf-ipaexfont ttf-symbola
```

### 日本語入力

```bash
$ yaourt -S fcitx-qt5 fcitx-im fcitx-configtool fcitx-mozc
```

インストール後にFcitx Configuration からMozcを追加する必要がある。

### テーマ

```bash
$ yaourt numix-themes-git numix-icon-theme-git
```
