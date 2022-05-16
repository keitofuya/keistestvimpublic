# Jenkins構築手順for Windows

- [Jenkins構築手順for Windows](#jenkins構築手順for-windows)
	- [Jenkinsの構築手順](#jenkinsの構築手順)
	- [Java1.8のインストール](#java18のインストール)
	- [Jenkinsのインストール](#jenkinsのインストール)
	- [Jenkinsのセットアップ](#jenkinsのセットアップ)
		- [プラグインのインストール](#プラグインのインストール)
	- [MSBuildによるVisual Sutdioの定期ビルド設定](#msbuildによるvisual-sutdioの定期ビルド設定)
		- [Jobの作成](#jobの作成)
		- [Jobの設定](#jobの設定)
			- [Generalタブ](#generalタブ)
			- [ソースコード管理タブ](#ソースコード管理タブ)
			- [ビルド・トリガタブ](#ビルドトリガタブ)
			- [ビルド環境タブ](#ビルド環境タブ)
			- [ビルドタブ](#ビルドタブ)
	- [SVNコミット時にビルドを実行する](#svnコミット時にビルドを実行する)
		- [Jobの作成](#jobの作成-1)
		- [Jobの設定](#jobの設定-1)
			- [Generalタブ](#generalタブ-1)
			- [ソースコード管理タブ](#ソースコード管理タブ-1)
			- [ビルド・トリガタブ](#ビルドトリガタブ-1)
			- [ビルド環境タブ](#ビルド環境タブ-1)
			- [ビルドタブ](#ビルドタブ-1)
		- [APIトークンの取得](#apiトークンの取得)
		- [SVNサーバにpost-commitフックスクリプトを仕込む](#svnサーバにpost-commitフックスクリプトを仕込む)
	- [Appendix](#appendix)
		- [ワークスペースの変更](#ワークスペースの変更)

## Jenkinsの構築手順

[Jenkins構築手順](https://techinfoofmicrosofttech.osscons.jp/index.php?Jenkins%E6%A7%8B%E7%AF%89%E6%89%8B%E9%A0%86)

## Java1.8のインストール

もしJava1.8がインストールされていない場合、インストールを先に実施する。
インストール済みの場合、本章の手順は実施不要。

1. [オラクル](https://www.oracle.com/technetwork/java/index.html)のサイトにアクセスする
2. Java SE 8.u333を選択する
3. Windowsタブを選択し、インストーラをダウンロードする
   ダウンロードにはOracleのユーザ登録が必要となるが、登録手順は割愛する
4. ダウンロードしたexeファイルを実行し、インストールを行う

## Jenkinsのインストール

1. [Jenkins](https://www.jenkins.io/)にアクセスする
2. Downloadをクリックする
3. LTS(Long Term Support)のバージョンをダウンロードする
4. ダウンロードしたmsiファイルを実行する
5. local
6. インストール完了後、[http://127.0.0.1:8080/](http://127.0.0.1:8080/)へアクセスする

## Jenkinsのセットアップ

### プラグインのインストール

MSBuild Plugin・・・Visual StudioのビルドツールMSBuild.exeを実行するプラグイン

___

## MSBuildによるVisual Sutdioの定期ビルド設定

SCM(Source Control Manager)による定期ビルド
SVNの負荷を考えるとSVNコミットをトリガーにビルドするほうが良いが、
Jenkins側の設定で完結するメリットはある。

### Jobの作成

1. Jenkinsの\[新規ジョブ作成\]を選択する。
2. 任意のジョブ名を入力する。
3. \[フリースタイ・プロジェクトのビルド\]を選択する。
4. \[OK\]を選択する。

### Jobの設定

#### Generalタブ

1. \[高度]な設定を選択する。
2. \[カスタムワークスペースを使用\]のチェックを入れる。
3. ディレクトリ入力欄にチェックアウト、ビルドするディレクトリパスを設定する。(ここがbat実行時のカレントディレクトリパスとなる)

#### ソースコード管理タブ

1. Subversionを選択する。
2. Repository URLにチェックアウトするリポジトリURLを設定する。
3. Credentialsの\[追加\]を選択する。
4. 各設定を行う。
   Domain:グローバル度面
   種類:ユーザー名とパスワード
   スコープ:グローバル
   ユーザー名:SVNチェックアウトを行うユーザー名
   パスワード:SVNチェックアウトを行うユーザーのパスワード
   ID:空欄
   説明:空欄
5. \[追加\]を押下する。

#### ビルド・トリガタブ

1. SCMをポーリングにチェックを入れる。
2. スケジュールを設定する。
   5分毎の場合、以下のように設定する。
   H/5 \* \* \* \*

#### ビルド環境タブ

デフォルト設定のまま

#### ビルドタブ

1. \[ビルド手順の追加\]-\[MSBuild\]の実行を選択する。
2. VSのバージョンを選択する。
3. MSBuildビルドファイルにソリューションファイルを設定する。
   例:"D:\SVN\build\TestProject\All.sln"
4. コマンドライン引数を設定する。
   例:/p:Configuration=Release;Platform="x64"
      Platformをx64、Releaseでビルドする場合

___

## SVNコミット時にビルドを実行する

SVNのpost-commit.bat内にJenkinsへビルドを要求する処理を追加する。

### Jobの作成

1. Jenkinsの\[新規ジョブ作成\]を選択する。
2. 任意のジョブ名を入力する。
3. \[フリースタイ・プロジェクトのビルド\]を選択する。
4. \[OK\]を選択する。

### Jobの設定

#### Generalタブ

1. \[高度]な設定を選択する。
2. \[カスタムワークスペースを使用\]のチェックを入れる。
3. ディレクトリ入力欄にチェックアウト、ビルドするディレクトリパスを設定する。(ここがbat実行時のカレントディレクトリパスとなる)

#### ソースコード管理タブ

1. Subversionを選択する。
2. Repository URLにチェックアウトするリポジトリURLを設定する。
3. Credentialsの\[追加\]を選択する。
4. 各設定を行う。
   Domain:グローバル度面
   種類:ユーザー名とパスワード
   スコープ:グローバル
   ユーザー名:SVNチェックアウトを行うユーザー名
   パスワード:SVNチェックアウトを行うユーザーのパスワード
   ID:空欄
   説明:空欄
5. \[追加\]を押下する。

#### ビルド・トリガタブ

1. \[リモートからビルド\]にチェックを入れる。
2. 認証トークンに任意の英字文字列を設定する。

#### ビルド環境タブ

デフォルト設定のまま

#### ビルドタブ

1. \[ビルド手順の追加\]-\[MSBuild\]の実行を選択する。
2. VSのバージョンを選択する。
3. MSBuildビルドファイルにソリューションファイルを設定する。
   例:"D:\SVN\build\TestProject\All.sln"
4. コマンドライン引数を設定する。
   例:/p:Configuration=Release;Platform="x64"
     Platformをx64、Releaseでビルドする場合

### APIトークンの取得

1. Jenkinsログインユーザの設定を開く。
2. トークン新規追加を選択し、生成したAPIトークンをコピーする。
    ※生成したAPIトークンは再表示されないので注意すること
3. \[保存\]を選択する。

### SVNサーバにpost-commitフックスクリプトを仕込む

SVNサーバの対象リポジトリのhooksフォルダ内post-commit.bat内に以下を追加する。

```bat
rem SVNコミット時にJenkinsでビルドを実行する
set user=Jenkinsユーザ名を設定
set api_token=APIトークンの取得で生成したAPIトークンを指定
set jenkinsURL=JenkinsのURL:ポート番号
set job=Jenkins上のJob名称
set product_token=ビルドトリガタブで設定した認証トークンを指定

curl -X POST -u %user%:%api_token% %jenkinsURL%/job/%job%/build?token=%product_token%
```

___

## Appendix

### ワークスペースの変更

1. ジョブの設定を選択する
2. Generalタブの\[高度な設定\]を選択する
3. \[カスタムワークスペース\]を使用の内容を変更する

