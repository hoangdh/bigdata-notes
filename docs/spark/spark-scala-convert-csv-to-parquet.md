### Chuyển đổi từ CSV thành Parquet

```
val df = spark.read.options(Map("inferSchema"->"true","delimiter"->",","header"->"true")).csv("hdfs://10.10.10.10:8020/data/recommend")
df.repartition(50).write.option("compression", "snappy").parquet("/data/ec/recommend/")
```

- Đọc từ 1 cụm khác và ghi vào cụm mới có bật Erasure coding
- Gộp thành 50 files
- Sử dụng nén là SNAPPY

### Tham khảo:
- https://sparkbyexamples.com/spark/spark-convert-csv-to-avro-parquet-json/
- https://stackoverflow.com/q/69309866
- https://stackoverflow.com/a/51631645
