# Docker構築手順

## Windows版Dockerのインストール

## Dockerの作成

## Dockerの起動

## Dockerコマンド

```bash
# Dockerイメージの確認
docker images
# Dockerコンテナの確認
docker ps -a
```

## Dockerの終了


###

## コンテナの実行検証(Webサイト)

### httpdイメージの取得

```bash
docker pull httpd
```

### Dockerコンテナの実行

```bash
docker run -d -p 8080:80 httpd
```

### Webブラウザでのアクセス

Webブラウザのアドレスにlocalhosts:8080と入力する
httpdの確認ページが表示される

## DockerでSubversionサーバを立てる

### Subversion込みのDockerをインストールする

```bash
docker pull elleflorio/svn-server
```

### DockerのSVNを実行する

```bash
docker run -d --name svn-server -p 80:80 -p 3690:3690 elleflorio/svn-server
```

### SVNログイン時のユーザ名およびパスワードを登録する

```bash
docker exec -t svn-server htpasswd -b /etc/subversion/passwd <username> <password>
```

\<username>, \<password>には任意の値を設定する

### SVNへログインする

Webブラウザより(http://localhost/svnadmin)へアクセスする

「Collection of Repositories」が表示されれば構築成功

