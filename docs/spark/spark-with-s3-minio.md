# Sử dụng Spark đọc ghi S3 MinIO

## 1. Chuẩn bị

Sử dụng 2 thư viện sau: [hadoop-aws-2.7.3.jar](https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/2.7.3/), [aws-java-sdk-1.7.4.jar](https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk/1.7.4/)

## 2.Thực hiện kiểm tra bằng `spark-shell`

Trong ví dụ này, ta giả sử đã tải 2 thư viện trên vào `/tmp`:

> spark-shell --executor-memory 2G --executor-cores 1 --num-executors 2 --jars /tmp/aws-java-sdk-1.7.4.jar,/tmp/hadoop-aws-2.7.3.jar

Sau khi vào trong shell của spark; ta khai báo các biến liên quan tới s3 như sau:

```
spark.sparkContext.hadoopConfiguration.set("fs.s3a.access.key", "minio")
spark.sparkContext.hadoopConfiguration.set("fs.s3a.secret.key", "minio123")
spark.sparkContext.hadoopConfiguration.set("fs.s3a.endpoint", "127.0.0.1:9000")
spark.sparkContext.hadoopConfiguration.set("fs.s3a.connection.ssl.enabled", "false")
spark.sparkContext.hadoopConfiguration.set("fs.s3a.impl", "org.apache.hadoop.fs.s3a.S3AFileSystem")
```

**Chú ý:** Điền chính xác thông tin MinIO.

Sau đó, ta có thể đọc ghi bình thường với tiền tố là s3a. Ví dụ

- Đọc dữ liệu:

```
val df = spark.read.parquet("s3a://bucket/parquet/da1/")
```

- Ghi dữ liệu

```
df.write.parquet("s3a://bucket/parquet/da2/")
```

### 3. Cài đặt và cấu hình

- Tải 02 thư viện bên trên vào `$SPARK_HOME/jars/`
- Cấu hình thông tin S3 MinIO vào `$SPARK_HOME/conf/spark-defauts.conf`

```
...
spark.hadoop.fs.s3a.access.key minio
spark.hadoop.fs.s3a.secret.key minio123
spark.hadoop.fs.s3a.endpoint 127.0.0.1:9000

spark.hadoop.fs.s3a.connection.ssl.enabled false
spark.hadoop.fs.s3a.impl org.apache.hadoop.fs.s3a.S3AFileSystem
```
- Thực hiện kiểm tra bằng `spark-shell` theo phần trên; bỏ qua bước khai báo thư viện và thông tin S3 MinIO

> spark-shell --executor-memory 2G --executor-cores 1 --num-executors 2


### 4. Tham khảo
- https://www.codetd.com/en/article/8427627
- https://towardsdatascience.com/apache-spark-with-kubernetes-and-fast-s3-access-27e64eb14e0f
