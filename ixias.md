# Ixias学習

## Ixiasの導入
Ixiasはbuild.sbtに以下の記述をすることで導入できる。

```scala
scalaVersion := "2.12.11"
resolvers ++= Seq(
  "IxiaS Releases" at "http://maven.ixias.net.s3-ap-northeast-1.amazonaws.com/releases"
)
libraryDependencies ++= Seq(
  "net.ixias" %% "ixias"      % "1.1.25",
  "net.ixias" %% "ixias-aws"  % "1.1.25",
  "net.ixias" %% "ixias-play" % "1.1.25",
  "mysql" % "mysql-connector-java" % "5.1.+",
  "ch.qos.logback" % "logback-classic" % "1.1.+"
)
```

## 保育士バンクで主に使用されている主なIxiasライブラリ
- `ixias.model`
- `ixias.persistence`
- `ixias.uitl`
- `ixias.aws.s3`
- `ixias.aws.sns`
- `ixias.auth`
