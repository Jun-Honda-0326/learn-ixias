# aws.s3
AWS s3に関するライブラリ

## `model.File.scala`
S3 Fileのモデル定義
```scala
case class File(
  val id:           Option[File.Id],             // Id
  val region:       String,                      // AWS region
  val bucket:       String,                      // The bucket of S3
  val key:          String,                      // The file key.
  val typedef:      String,                      // The file-type.
  val imageSize:    Option[File.ImageSize],      // If file-type is image. image size is setted.
  val presignedUrl: Option[java.net.URL] = None, // The presigned Url to accessing on Image
  val updatedAt:    LocalDateTime        = NOW,  // The Datetime when a data was updated.
  val createdAt:    LocalDateTime        = NOW   // The Datetime when a data was created.
) extends EntityModel[File.Id]
```
