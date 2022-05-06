# WSL CentOS7 セットアップ手順

## はじめに

WSLでのCentOS7はminimalインストールのため、必要なソフトは個別にインストールする必要がある

## インストール

1. vimインストール

   ```bash
   yum -y install vim-enhanced
   ```

   コマンドエイリアスを適用

   ```bash
   vi /etc/profile

   最終行にエイリアス追記
   alias vi='vim'

   変更を反映
   source /etc/profile
   ```

2. Nerdフォントのインストール

3. dein.vimのインストール
