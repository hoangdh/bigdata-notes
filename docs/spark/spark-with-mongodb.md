### Ghi chép còn sơ khai

```
/opt/spark3/bin/spark-shell \
--conf "spark.mongodb.read.connection.uri=mongodb://10.10.10.44/sample_mflix.users?readPreference=primaryPreferred" \
--conf "spark.mongodb.write.connection.uri=mongodb://10.10.10.44/sample_mflix.users" \
--packages org.mongodb.spark:mongo-spark-connector_2.12:10.2.0
```

### Tham khảo:
- https://www.mongodb.com/docs/spark-connector/current/
