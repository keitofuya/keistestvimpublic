# Windows Terminalセットアップ

- [Windows Terminalセットアップ](#windows-terminalセットアップ)
  - [Windows Terminal側セットアップ](#windows-terminal側セットアップ)
    - [Oh My Poshのインストール](#oh-my-poshのインストール)
    - [フォントのインストール](#フォントのインストール)
    - [PoshsThemesを適用する](#poshsthemesを適用する)
    - [起動時にPoshsTemesを読み込むように$profileに設定を書き込む](#起動時にposhstemesを読み込むようにprofileに設定を書き込む)
    - [SSH接続のプロファイルを追加する](#ssh接続のプロファイルを追加する)
    - [WSLのインストール](#wslのインストール)
    - [Windows機能の有効化](#windows機能の有効化)
    - [仮想マシンプラットフォームを有効化する](#仮想マシンプラットフォームを有効化する)
    - [WSL2をデフォルトに設定](#wsl2をデフォルトに設定)
  - [WSL側の設定](#wsl側の設定)
    - [プラグインの設定](#プラグインの設定)
      - [Nerdフォントのインストール(WSL側でインストールすればよいので不要)](#nerdフォントのインストールwsl側でインストールすればよいので不要)
      - [dein.vimのインストール](#deinvimのインストール)
    - [Gitのインストール](#gitのインストール)
  - [wsl間のクリップボードの共有](#wsl間のクリップボードの共有)
    - [win32yank.exeを使用する](#win32yankexeを使用する)
    - [Oh my Poshのインストール手順](#oh-my-poshのインストール手順)
      - [Oh my Poshのインストール](#oh-my-poshのインストール-1)
      - [Promptの変更](#promptの変更)
    - [WindowsにXServerを導入してvimで使用する](#windowsにxserverを導入してvimで使用する)
  - [CentOS7.xのインストール](#centos7xのインストール)
    - [GitよりCentOS7をダウンロード](#gitよりcentos7をダウンロード)
    - [SSHServerのインストール](#sshserverのインストール)
  - [vimrc設定](#vimrc設定)

## Windows Terminal側セットアップ

### Oh My Poshのインストール

```bash
Import-Module oh-my-posh
```

### フォントのインストール

1. [Nerdフォントのサイト](https://www.nerdfonts.com/)からCaskaydia Cove Nerd Fontをインストールする
2. Downloadsを選択する
3. Caskaydia Cove Nerd FontのDownloadを選択する
4. ダウンロードファイルを解凍し、Caskaydia Cove Nerd Font Complete.ttfをダブルクリックし、インストールを選択する

### PoshsThemesを適用する

```bash
Set-PoshPrompt -Theme takuya
```

ここではtakuyaというThemeを使用しているが、Get-PoshThemesで好みのThemeを設定してよい

### 起動時にPoshsTemesを読み込むように$profileに設定を書き込む

```bash
notepad $profile
```

notepadで以下を入力する

```notepad
Import-Module oh-my-posh
Set-PoshPrompt -Theme takuya
```

### SSH接続のプロファイルを追加する

### WSLのインストール

### Windows機能の有効化

Linux用Windowsサブシステムのチェックを入れる
OSの再起動を行う
Microsoft StoreよりUbuntu 18.04 LTSをインストールする

### 仮想マシンプラットフォームを有効化する

管理者権限でPowerShellを起動し、以下コマンドを実行する

```bash
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

PCの再起動を行う

### WSL2をデフォルトに設定

```bash
wsl --set-default-version 2
```

カーネルコンポーネントの更新が必要と表示された場合は、[WSL2 Linuxカーネルの更新](https://docs.microsoft.com/ja-jp/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package)を参照する

再度実行する

```bash
wsl --set-default-version 2
```

## WSL側の設定

### プラグインの設定

#### Nerdフォントのインストール(WSL側でインストールすればよいので不要)

```bash
git clone --branch=master --depth 1 https://github.com/ryanoasis/nerd-fonts.git
cd nerd-fonts
./install.sh
cd ..
rm -rf nerd-fonts
```

#### dein.vimのインストール

```bash
mkdir -p ~/.vim/dein
curl https://raw.githubusercontent.com/Shougo/dein.vim/master/bin/installer.sh > installer.sh
sh ./installer.sh ~/.vim/dein
```


### Gitのインストール

```bash
yum install git
sudo yum -y install gcc curl-devel expat-devel gettext-devel openssl-devel zlib-devel perl-ExtUtils-MakeMaker autoconf
sudo yum install wget
cd /usr/local/src/
sudo wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.9.5.tar.gz
sudo tar xzvf git-2.9.5.tar.gz
```

failed: Name or service not known.エラーとなった場合は以下の対応をする

```bash
vi /etc/resolv.conf
```

```text
#nameserver 172.31.224.1  # コメントにする
nameserver 8.8.8.8 #追加する プライマリDNSサーバー
nameserver 8.8.4.4 #追加する セカンダリDNSサーバー
options single-request-reopen
```

なんかうまく動かなかった

## wsl間のクリップボードの共有

### win32yank.exeを使用する

1. [win32yank.exe](https://github.com/equalsraf/win32yank/releases)をダウンロードする
2. win32yank.exeのPATHを通す
    ~/.profileに以下追加

    ```bash
    export PATH=$PATH:/mnt/d/Tool/win32yank
    ```

3. 以下をvimrcに追記

    ```text
    nnoremap <silent>yy :.w !win32yank.exe -i<CR><CR>
    vnoremap <silent>y :w !win32yank.exe -i<CR><CR>
    nnoremap <silent>dd :.w !win32yank.exe -i<CR>dd
    vnoremap <silent>d x:let pos = getpos(".")<CR>GpVG:w !win32yank.exe -i<CR>VGx:call setpos(".", pos)<CR>
    nnoremap <silent>p :r !win32yank.exe -o<CR>
    vnoremap <silent>p :r !win32yank.exe -o<CR>
    ```

### Oh my Poshのインストール手順

#### Oh my Poshのインストール

```bash
## Install Oh my Posh
sudo wget https://github.com/JanDeDobbeleer/oh-my-posh/releases/latest/download/posh-linux-amd64 -O /usr/local/bin/oh-my-posh
sudo chmod +x /usr/local/bin/oh-my-posh
## Download the themes
mkdir ~/.poshthemes
wget https://github.com/JanDeDobbeleer/oh-my-posh/releases/latest/download/themes.zip -O ~/.poshthemes/themes.zip
sudo apt install unzip
unzip ~/.poshthemes/themes.zip -d ~/.poshthemes
chmod u+rw ~/.poshthemes/*.json
rm ~/.poshthemes/themes.zip
```

#### Promptの変更

以下のコマンドを実行する

```bash
echo '"$(oh-my-posh --init --shell bash --config ~/.{theme}.omp.json)"' >> ~/.bash_profile
. ~/.bash_profile
```

{theme}には好きなテーマを設定する。おすすめはtakuya.omp.json

### WindowsにXServerを導入してvimで使用する

1. [VcXsrv](https://sourceforge.net/projects/vcxsrv/)をインストール
2. インストールが終わったらスタートメニューから「XLaunch」を実行(全てデフォルト値。クリップボードオプションにチェックが入っていることを確認)
3. bashrcにexport DISPLAY=localhost:0.0を追記

## CentOS7.xのインストール

Ubuntuではなく、CentOS7.xをインストールする手順

### GitよりCentOS7をダウンロード

[yuk/CentWSL](https://github.com/yuk7/CentWSL)
C:\Users\username\AppData\Local\Packagesのディレクトリに設置
ダウンロードしたファイルを任意のフォルダへ解凍する
以下のbatファイルを解凍したフォルダと同じ位置へ格納し、実行する
[CentOS7Setup.bat](https://www.system-v.cf/wsl/CentOS7setup.bat)

### SSHServerのインストール

別端末からSSH接続できるようにする

1. openssh-sshserverのインストール

    ```bash
    yum install openssh-server -y
    ```

2. パスワード認証を有効にする

    ```bash
    vi /etc/ssh/sshd_config
    ```

    ```text
    #To disable tunneled clear text passwords, change to no here!
    PasswordAuthentication yes
    #PermitEmptyPasswords no
    PasswordAuthentication yes
    ```

3. サービスを再開する

   ```bash
   service sshd restart
   ```

   以下のようにエラーが発生して失敗する

   ```text
   Redirecting to /bin/systemctl restart sshd.service
   Failed to get D-Bus connection: Operation not permitted
   ```

4. /usr/bin/systemctlの変更

    ```bash
    mv /usr/bin/systemctl /usr/bin/systemctl.old
    ```

    ```bash
    curl https://raw.githubusercontent.com/gdraheim/docker-systemctl-replacement/master/files/docker/systemctl.py > /usr/bin/systemctl
    ```

    ``` bash
    chmod +x /usr/bin/systemctl
    ```

5. RSAキーの作成

    ```bash
    ssh-keygen -A
    ```

    ```bash
    service sshd restart
    systemctl enable sshd
    ```

## vimrc設定

```vimrc
if &compatible
  set nocompatible               " Be iMproved
endif

set runtimepath+=~/.vim/dein/repos/github.com/Shougo/dein.vim

if dein#load_state('~/.vim/dein')
  call dein#begin('~/.vim/dein')

  " Let dein manage dein
  call dein#add('~/.vim/dein/repos/github.com/Shougo/dein.vim')

  " Add or remove your plugins here like this:
  "call dein#add('Shougo/neosnippet.vim')
  "call dein#add('Shougo/neosnippet-snippets')
  call dein#add('vim-airline/vim-airline')
  call dein#add('vim-airline/vim-airline-themes')
  call dein#add('ryanoasis/vim-devicons')
  call dein#add('tpope/vim-surround')
  call dein#add('easymotion/vim-easymotion')
  "call dein#add('junegunn/fzf')
  "call dein#add('junegunn/fzf.vim')
  call dein#add('mattn/vim-maketable')
  call dein#add('vim-jp/vimdoc-ja')
  call dein#add('kana/vim-operator-user')
  call dein#add('kana/vim-operator-replace')
  call dein#add('vim-jp/cpp-vim')
  call dein#add('shougo/neocomplete.vim')
  call dein#add('LeafCage/yankround.vim')
  call dein#add('tomtom/tcomment_vim')
  call dein#add('Shougo/unite.vim')
  call dein#add('Shougo/neomru.vim')

  call dein#end()
  call dein#save_state()
endif

filetype plugin indent on
syntax enable

" If you want to install not installed plugins on startup.
if dein#check_install()
  call dein#install()
endif

let g:airline_theme = 'angr'               " テーマの指定
let g:airline#extensions#tabline#enabled = 1 " タブラインを表示
let g:airline_powerline_fonts = 1            " Powerline Fontsを利用
" ファイルを上書きする前にバックアップを作ることを無効化
set nowritebackup
" ファイルを上書きする前にバックアップを作ることを無効化
set nobackup
" vim の矩形選択で文字が無くても右へ進める
set virtualedit=block
" 挿入モードでバックスペースで削除できるようにする
set backspace=indent,eol,start
" 全角文字専用の設定
set ambiwidth=double
" wildmenuオプションを有効(vimバーからファイルを選択できる)
set wildmenu
" リーダー設定をスペースにする
let mapleader = "\<Space>"
" 自動インデント
set autoindent

"----------------------------------------
" 検索
"----------------------------------------
" 検索するときに大文字小文字を区別しない
set ignorecase
" 小文字で検索すると大文字と小文字を無視して検索
set smartcase
" 検索がファイル末尾まで進んだら、ファイル先頭から再び検索
set wrapscan
" インクリメンタル検索 (検索ワードの最初の文字を入力した時点で検索が開始)
set incsearch
" 検索結果をハイライト表示
set hlsearch
hi search ctermbg=Red
hi search ctermfg=White

"----------------------------------------
" 表示設定
"----------------------------------------
" ビープ音すべてを無効にする
set visualbell t_vb=
" エラーメッセージの表示時にビープを鳴らさない
set noerrorbells
" Windowsでパスの区切り文字をスラッシュで扱う
set shellslash
" 対応する括弧やブレースを表示
set showmatch matchtime=1
" インデント方法の変更
set cinoptions+=:0
" メッセージ表示欄を2行確保
set cmdheight=2
" ステータス行を常に表示
set laststatus=2
" カーソルがある画面上の行をCursolLineで強調する
"set cursorline
"set statusline=%F%m%h%w\ %<[ENC=%{&fenc!=''?&fenc:&enc}]\ [FMT=%{&ff}]\ [TYPE=%Y]\ %=[CODE=0x%02B]\ [POS=%l/%L(%02v)]
" ファイル名表示
"set statusline=%F
" 変更チェック表示
"set statusline+=%m
" 読み込み専用かどうか表示
"set statusline+=%r
" ヘルプページなら[HELP]と表示
"set statusline+=%h
" プレビューウインドウなら[Prevew]と表示
"set statusline+=%w
" これ以降は右寄せ表示
"set statusline+=%=
" ファイルタイプ表示
"set statusline+=%y
" file encoding
"set statusline+=[%{&fileencoding}]
" ファイルフォーマット表示
"set statusline+=[%{&fileformat}]
" 現在行数/全行数/パーセント
"set statusline+=[L%l:C%c\ (%p%%)]
" ウィンドウの右下にまだ実行していない入力中のコマンドを表示
set showcmd
" 省略されずに表示
"set display=lastline
" タブ文字を CTRL-I で表示し、行末に $ で表示する
set list
" 行末のスペースを可視化
set listchars=tab:^\ ,trail:~
" コマンドラインの履歴を10000件保存する
set history=10000
" コメントの色を水色
hi Comment ctermfg=3
" 入力モードでTabキー押下時に半角スペースを挿入
set expandtab
" インデント幅
set shiftwidth=2
" タブキー押下時に挿入される文字幅を指定
set softtabstop=2
" ファイル内にあるタブ文字の表示幅
set tabstop=2
" ツールバーを非表示にする
set guioptions-=T
" yでコピーした時にクリップボードに入る
set guioptions+=a
" メニューバーを非表示にする
set guioptions-=m
" 右スクロールバーを非表示
set guioptions+=R
" 対応する括弧を強調表示
set showmatch
" 改行時に入力された行の末尾に合わせて次の行のインデントを増減する
set smartindent
" スワップファイルを作成しない
set noswapfile
" 検索にマッチした行以外を折りたたむ(フォールドする)機能
set nofoldenable
" タイトルを表示
set title
" 行番号の表示
set number
" ヤンクでクリップボードにコピー
set clipboard=unnamed,autoselect
" Escの2回押しでハイライト消去
nnoremap <Esc><Esc> :nohlsearch<CR><ESC>
" シンタックスハイライト
syntax on
" すべての数を10進数として扱う
set nrformats=
" 行をまたいで移動
set whichwrap=b,s,h,l,<,>,[,],~
" バッファスクロール
set mouse=a
" カーソル上のman実行
nnoremap <Leader>k <S-k>
" 移動
nnoremap <S-j> 5j
nnoremap <S-k> 5k
nnoremap <S-h> ^
nnoremap <S-l> $
" manの日本語化
set helplang=ja
" 対象をヤンクしている文字に置き換える(vim-operator-replace)
map R <Plug>(operator-replace)

" 中括弧の自動補完
inoremap { {}<Left>
inoremap {<Enter> {}<Left><CR><ESC><S-o>
inoremap ( ()<ESC>i
inoremap (<Enter> ()<Left><CR><ESC><S-o>

" auto reload .vimrc
augroup source-vimrc
  autocmd!
  autocmd BufWritePost *vimrc source $MYVIMRC | set foldmethod=marker
  autocmd BufWritePost *gvimrc if has('gui_running') source $MYGVIMRC
augroup END

" auto comment off
augroup auto_comment_off
  autocmd!
  autocmd BufEnter * setlocal formatoptions-=r
  autocmd BufEnter * setlocal formatoptions-=o
augroup END

" HTML/XML閉じタグ自動補完
augroup MyXML
  autocmd!
  autocmd Filetype xml inoremap <buffer> </ </<C-x><C-o>
  autocmd Filetype html inoremap <buffer> </ </<C-x><C-o>
augroup END

" 特定の言語のタブ設定
augroup FileTypeIndent
  autocmd!
  autocmd BUfRead,BufNewFile *.c setlocal noexpandtab tabstop=4 softtabstop=4 shiftwidth=4
  autocmd BUfRead,BufNewFile *.cpp setlocal noexpandtab tabstop=4 softtabstop=4 shiftwidth=4
  autocmd BUfRead,BufNewFile *.h setlocal noexpandtab tabstop=4 softtabstop=4 shiftwidth=4
augroup END

" 編集箇所のカーソルを記憶
if has("autocmd")
  augroup redhat
    " In text files, always limit the width of text to 78 characters
    autocmd BufRead *.txt set tw=78
    " When editing a file, always jump to the last cursor position
    autocmd BufReadPost *
    \ if line("'\"") > 0 && line ("'\"") <= line("$") |
    \   exe "normal! g'\"" |
    \ endif
  augroup END
endif

" カーソルの形状を各モードで変更する
if has('vim_starting')
  " 挿入モード時に非点滅の縦棒タイプのカーソル
  let &t_SI .= "\e[6 q"
  " ノーマルモード時に非点滅のブロックタイプのカーソル
  let &t_EI .= "\e[2 q"
  " 置換モード時に非点滅の下線タイプのカーソル
  let &t_SR .= "\e[4 q"
endif

" wsl-OS間のクリップボードの共有
nnoremap <silent>yy :.w !win32yank.exe -i<CR><CR>
vnoremap <silent>y :w !win32yank.exe -i<CR><CR>
nnoremap <silent>dd :.w !win32yank.exe -i<CR>dd
vnoremap <silent>d x:let pos = getpos(".")<CR>GpVG:w !win32yank.exe -i<CR>VGx:call setpos(".", pos)<CR>
nnoremap <silent>p :r !win32yank.exe -o<CR>
vnoremap <silent>p :r !win32yank.exe -o<CR>

" yank時に遡って検索
nmap p <Plug>(yankround-p)
xmap p <Plug>(yankround-p)
nmap P <Plug>(yankround-P)
nmap gp <Plug>(yankround-gp)
xmap gp <Plug>(yankround-gp)
nmap gP <Plug>(yankround-gP)
nmap <C-p> <Plug>(yankround-prev)
nmap <C-n> <Plug>(yankround-next)

" Unite
let g:unite_enable_start_insert=1
let g:unite_source_history_yank_enable =1
let g:unite_source_file_mru_limit = 200
nnoremap <silent> ,uy :<C-u>Unite history/yank<CR>
nnoremap <silent> ,ub :<C-u>Unite buffer<CR>
nnoremap <silent> ,uf :<C-u>UniteWithBufferDir -buffer-name=files file<CR>
nnoremap <silent> ,ur :<C-u>Unite -buffer-name=register register<CR>
nnoremap <silent> ,uu :<C-u>Unite file_mru buffer<CR>
```
