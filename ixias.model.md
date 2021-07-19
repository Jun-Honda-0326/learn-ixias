# Ixias.modelパッケージについて
`ixias-core`内に実装
DDDにおけるエンティティを実装している。

## `Entity.scala`

```scala
//idがないエンティティの型
type   WithNoId[K <: @@[_, _], M <: EntityModel[K]] = Entity[K, M, IdStatus.Empty]
//idがあるエンティティの型
type   EmbeddedId[K <: @@[_, _], M <: EntityModel[K]] = Entity[K, M, IdStatus.Exists]
```
## `EntityModel.scala`
EntityModeトレイトに必要値を定義
```scala
type Id         = K
type WithNoId   = Entity.WithNoId  [Id, this.type]
type EmbeddedId = Entity.EmbeddedId[Id, this.type]

// Option型にすることで、Idがあるかないかを判別できる
val id: Option[Id]
```
```scala
// WithNoId型のエンティティを生成
def toWithNoId:   WithNoId   = Entity.WithNoId(this)
// EmbeddedId型のエンティティを生成
def toEmbeddedId: EmbeddedId = Entity.EmbeddedId(this)
```
