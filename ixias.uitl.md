# `ixias.util`パッケージについて
ixias-core内に実装

## `Enum.scala`
Enum定義を共通化するパッケージ
```scala
object EnumStatus {
  abstract class Of[T <: EnumStatus]
    (implicit ttag: ClassTag[T]) extends Enum.Of[T] {
    def apply(code: Short): T = this.find(_.code == code).get
  }
}

// 使用例
 sealed abstract class Color(red: Double, green: Double, blue: Double)
 object Color extends EnumOf[Color] {
    case object Red   extends Color(1, 0, 0)
    case object Green extends Color(0, 1, 0)
```

## `Configuration.scala`
```scala
URLのpathからConfig設定(application.conf)がどれを参照しているか取得する(stg, acc prod)
def get[A](path: String)(implicit loader: ConfigLoader[A]): A = {
    loader.load(underlying, path)
```
