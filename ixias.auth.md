# ixias-play-authについて
ixias-play-authはIxiasを用いてPlayアプリに認証機能を導入するためのツールを提供するライブラリ。<br>
認証トークンはHTTPヘッダ方式とセッション方式がある。<br>

![ディレクトリ構造](https://miro.medium.com/max/1120/1*KrOt56KAoyUSTGYi2ubu2Q.png)

## Containerについて
Containerディレクトリは、認証トークンを生成、更新、削除するデータストアについて定義したもの。
```scala
// セッションが開始した時に自動で実行されるコールバックで認証トークンを生成するメソッド
def open(uid: Id, expiry: Duration)
  (implicit request: RequestHeader): Future[AuthenticityToken]
// 認証トークンのタイムアウトを設定するメソッド
def setTimeout(token: AuthenticityToken, expiry: Duration)
  (implicit request: RequestHeader): Future[Unit]
// 認証トークンを引数として、認証トークンと紐付いたユーザーをオプションで返すメソッド
def read(token: AuthenticityToken)
  (implicit request: RequestHeader): Future[Option[Id]]
// セッションが終了した時に実行されるコールバックで認証トークンを削除するメソッド
def destroy(token: AuthenticityToken)
  (implicit request: RequestHeader): Future[Unit]
```

[定義元](https://github.com/ixias-net/ixias/blob/develop/framework/ixias-play-auth/src/main/scala/ixias/play/api/auth/container/Container.scala)

```scala
sample 認証トークンを生成して保存する操作
// トークンを生成し、ユーザーIDと紐づけるメソッド
def open(uid: Id, expiry: Duration)
  (implicit request: RequestHeader): Future[AuthenticityToken] = {
    // トークンを生成する
    val token = AuthenticityToken(TokenGenerator().next(TOKEN_LENGTH))
    // トークンとユーザーIDを紐づける
    val authToken = AuthToken(uid, token, expiry)
    for {
      // トークンのDBへの保存
      _ <- AuthTokenRepository.add(authToken)
    } yield token
  }
```

## mvcについて
mvcディレクトリは、Play アプリケーションのコントローラーに認証機能を加えるアクションや認証情報について定義したもので、例えば、アクションビルダーとして Authenticated を用いることによって、認証済みユーザーのみが呼び出すことのできるアクションを定義することができる。<br>

[定義元](https://github.com/ixias-net/ixias/tree/develop/framework/ixias-play-auth/src/main/scala/ixias/play/api/auth/mvc)


```scala アクションビルダーとしてAuthenticatedを用いて認証済みユーザーのみが呼び出すことのでできるアクション定義
sample
// ログインしている場合のみ記事一覧を表示するアクション
def index = Authenticated(authProfile).async { implicit req =>
  authProfile.loggedIn { user =>
    for {
      posts <- PostRepository.findAllByUser(user.id)
    } yield Ok(views.html.post.PostList(ViewValuePostList(  //Twirlへのレンダリング処理
      user  = ViewValueUser.from(user),
      posts = posts.map(ViewValuePost.from)
    )))
  }
}
```
 ## tokenについて
tokenディレクトリは、認証トークンについて定義したもの。<br>

[定義元](https://github.com/ixias-net/ixias/tree/develop/framework/ixias-play-auth/src/main/scala/ixias/play/api/auth/token)

```scala
// 認証トークンの定義
Token.scala
sealed trait Tag
protected object Tag {
  trait SignedToken extends Token
  trait AuthenticityToken extends Token
}
type SignedToken = String @@ Tag.SignedToken
type AuthenticityToken = String @@ Tag.AuthenticityToken
val SignedToken = the[Identity[SignedToken]]
val AuthenticityToken = the[Identity[AuthenticityToken]]
```
```scala
// 認証トークンの生成
val token: AuthenticityToken =
  AuthenticityToken(TokenGenerator.next(TOKEN_LENGTH))
```
