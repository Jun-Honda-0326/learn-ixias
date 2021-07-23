## `AmazonSNS.scala`
AWS SNSに関するライブラリ

```scala
// snsメッセージをsnsトピックに送信するメソッド
def publish(message: String): Future[Seq[PublishResult]]
```

## `backend.AmazonSNSBackend.scala`
```scala
// DataSourceを基にClientを取得
def getClient(implicit dsn: DataSourceName): Future[AmazonSNS]
```

## `AmazonSNSConfig.scala`
AWS SNSの設定のためのライブラリ

```scala
// AWSのAccessKeyを取得
protected def getAWSAccessKeyId(implicit dsn: DataSourceName): Try[String]

// AWSのシークレットキーを取得
protected def getAWSSecretKey(implicit dsn: DataSourceName): Try[String]

// AWSのリージョンを取得
protected def getAWSRegion(implicit dsn: DataSourceName): Try[Regions]

// AWSのリソースネームを取得
def getTopicARN(implicit dsn: DataSourceName): Try[Seq[String]]
```
