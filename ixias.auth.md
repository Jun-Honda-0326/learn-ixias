# ixias-play-authについて
ixias-play-authはIxiasを用いてPlayアプリに認証機能を導入するためのツールを提供するライブラリ。<br>
認証トークンはHTTPヘッダ方式とセッション方式がある。<br>

![ディレクトリ構造](https://miro.medium.com/max/1120/1*KrOt56KAoyUSTGYi2ubu2Q.png)

## Containerについて
Containerディレクトリは、認証トークンを生成、更新、削除するデータストアについて定義するためのものです。認証トークンを保存するデータストアとしては、データベースや memcached があります。例えば、 Container トレイトを継承したクラスにおいて、認証トークンを生成して保存する操作を定義できます。<br>

[定義元](https://github.com/ixias-net/ixias/blob/develop/framework/ixias-play-auth/src/main/scala/ixias/play/api/auth/container/Container.scala)

## mvcについて
mvcディレクトリは、Play アプリケーションのコントローラーに認証機能を加えるアクションや認証情報について定義したものです。例えば、アクションビルダーとして Authenticated を用いることによって、認証済みユーザーのみが呼び出すことのできるアクションを定義できます。<br>

[定義元](https://github.com/ixias-net/ixias/tree/develop/framework/ixias-play-auth/src/main/scala/ixias/play/api/auth/mvc)

 ## tokenについて
tokenディレクトリは、認証トークンについて定義したものです。TokenViaHttpHeader は HTTP ヘッダ方式の認証トークンに関する操作を定義しており、TokenViaSession はセッション方式の認証トークンに関する操作を定義しています。TokenViaSession には、以下のように認証トークンを Cookie に保存するメソッドなどが定義されています。<br>

[定義元](https://github.com/ixias-net/ixias/tree/develop/framework/ixias-play-auth/src/main/scala/ixias/play/api/auth/token)
