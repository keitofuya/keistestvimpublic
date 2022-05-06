# Git操作方法

- [Git操作方法](#git操作方法)
	- [VS CodeでのGitセットアップ](#vs-codeでのgitセットアップ)
		- [Gitのインストール及びセットアップ](#gitのインストール及びセットアップ)
	- [VS CodeでのGit操作方法](#vs-codeでのgit操作方法)
		- [ローカルリポジトリにコミットする](#ローカルリポジトリにコミットする)
		- [ローカルリポジトリからリモートリポジトリへコミットする](#ローカルリポジトリからリモートリポジトリへコミットする)
	- [Gitの拡張機能インストール](#gitの拡張機能インストール)

## VS CodeでのGitセットアップ

### Gitのインストール及びセットアップ

1. Windows版Gitの[インストール](https://gitforwindows.org/)
2. Git for Windowsに同梱されているGit Bashを起動する
3. 以下のコマンドを実行する

   ```bash
   git config --global user.name 'username'
   git config --global user.email 'username@example.com'
   git config --global core.editor 'code --wait'
   git config --global merge.tool 'code --wait "$MERGED"'
   git config --global push.default simple
   git config --global pull.rebase false
   git config --global core.quotepath false
   ```

4. VS Codeの起動
5. Source Controlを表示し、リポジトリの初期化を選択する

## VS CodeでのGit操作方法

### ローカルリポジトリにコミットする

1. Source Controlアイコンを押下する
2. 差分を確認する
3. コミット用コメントを入力する
4. コミットするファイルにフォーカスを移動し、+ボタンを押下する(ステージングと呼ぶ)
5. ✔ボタンを押下する
※この段階ではまだローカル上での操作となるため、リモートリポジトリには影響なし

### ローカルリポジトリからリモートリポジトリへコミットする

1. Ctrl+Shift+pを入力し、pushを入力、実行する

## Gitの拡張機能インストール

* Git History
