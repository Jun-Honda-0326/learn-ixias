# ixias.persistenceパッケージについて
ixias-core内に実装

## `SlickRepository.scala`
`Repository`トレイト、`SlickProfile`トレイトを継承しており、永続ストレージとデータの連携ができるAPIを実装している
```scala
trait SlickRepository[K <: @@[_, _], M <: EntityModel[K], P <: JdbcProfile]
    extends Repository[K, M] with SlickProfile[P] {
  trait API extends super.API
      with SlickDBIOActionOps[K, M]
  override val api: API = new API {}
}
```

## `Repository.scala`
更に`Repository`トレイトは`EntityIOAciton`を継承している
```scala
trait Repository[K <: @@[_, _], M <: EntityModel[K]]
extends Profile with EntityIOAction[K, M]
```

## `dbio.EntityIOAction.scala`
`.EntityIOAction`にはCRUD機能を実装するためのメソッドが用意されており、引数と返り値の型を統一することができる。
```scala
def get(id: Id): Future[Option[EntityEmbeddedId]]
def add(entity: EntityWithNoId): Future[Id]
def update(entity: EntityEmbeddedId): Future[Option[EntityEmbeddedId]]
def remove(id: Id): Future[Option[EntityEmbeddedId]]
```
## `SlickDBAction.scala`
```scala
  // 永続ストレージの定義(デフォルトではマスター)
  val DEFAULT_DSN_KEY = DataSourceName.RESERVED_NAME_MASTER
  object SlickRunDBAction extends SlickDBAction {
    // slickのsb.runと同じように使用できる
    def apply[A, B, T <: Table[_, P]]
      (table: T, hostspec: String = DEFAULT_DSN_KEY)
      (action: T#Query => DBIOAction[A, NoStream, Nothing])
      (implicit conv: A => B): Future[B] =
      for {
        dsn    <- Future(table.dsn.get(hostspec).get)
        value  <- SlickDBAction[T].invokeBlock(SlickDBActionRequest(dsn, table), {
          case (db, slick) => db.run(action(slick))
        })
      } yield conv(value)
  }
```

## `lifted.SlickColumnOptionOps.scala`
Play <=> SQLとDBのデータ型のマッピングを行っている

定義元は[ixias.persistence.lifted.SlickColumnOptionOps](https://github.com/ixias-net/ixias/blob/develop/framework/ixias-core/src/main/scala/ixias/persistence/lifted/SlickColumnOptionOps.scala)に記載されており、この中からマッチしているカラムに変換する処理を記述する

```scala
例
def content = column[String]("body", O.Text)
```

## model.Cursor.scala
データ検索を行う際のCursor(検索結果からデータを抜き取るための仕組み)を定義
```scala
case class Cursor(
  val offset: Long         = 0L,
  val limit:  Option[Long] = Some(10L)
) extends CursorLike
```
