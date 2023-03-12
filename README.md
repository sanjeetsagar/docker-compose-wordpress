# Docker Compose WordPress
Docker ComposeでSSL/TLSに対応したWordPressを構築するリポジトリです。  
[kthksgy/docker-compose-nginx-proxy](https://github.com/kthksgy/docker-compose-nginx-proxy)と組み合わせて使用します。

## 必要動作環境
- Docker `>= 20.10`
- Docker Compose `>= 2.7`

参考: [Compose ファイル - Docker ドキュメント](https://matsuand.github.io/docs.docker.jp.onthefly/compose/compose-file/)

## 使用方法

### 初期設定
`.env`ファイルにWordPressとリバースプロキシの設定を記述します。

- `DB_PASSWORD`: データベースのパスワード
- `PRIMARY_HOST`: メインのホスト名
  この名前でコンテナやネットワークを作成します。
- `HOST`: WordPressに接続可能なホスト名の一覧
  カンマ区切りで複数指定する事が出来ます。
- `EMAIL`: SSL/TLSの証明書の期限が間近の場合に連絡を受けるメールアドレス

#### `.env`ファイルの例
```
DB_PASSWORD=password
PRIMARY_HOST=www.example.com
HOST=www.example.com,example.com
EMAIL=mail@example.com
```

### 起動
先に[kthksgy/docker-compose-nginx-proxy](https://github.com/kthksgy/docker-compose-nginx-proxy)を起動しておいてください。

以下のコマンドで起動します。  
初期設定を行う際に接続したURLがWordPressのデフォルトのURLに自動で認識されるので、**起動後は必ずメインのホスト名で接続して**、WordPressの初期設定を行ってください。

```bash
$ docker-compose up -d
```

## よくある質問(FAQ)
### Q. HTTPをHTTPSに自動でリダイレクトしたい。
nginx-proxyが勝手にやってくれるので、従来の`.htaccess`を変更するような対策は不要です。

### Q. www無しからwww有りに自動でリダイレクトしたい。もしくはその逆をしたい。
WordPressが自動で1つのURLにリダイレクトするようです。  
が、この際のリダイレクト先となるURLは**初期設定を行う際に接続したURL**となるので、必ず初期設定時はメインのURLで接続するようにしてください。

例えば、`www.example.com`(www有り)と`example.com`(www無し)からWordPressに接続可能な状態である時、`www.example.com`にURLを統一したいなら`www.example.com`で初期設定を行い、逆に`example.com`にURLを統一したいなら`example.com`で初期設定を行ってください。

## 参考
### Docker Hub
- [wordpress](https://hub.docker.com/_/wordpress)
- [mariadb](https://hub.docker.com/_/mariadb)
