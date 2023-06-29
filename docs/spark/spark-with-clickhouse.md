## Sử dụng Spark đọc ghi Clickhouse qua jdbc

### Chuẩn bị:
Trong ví dụ này; ta sử dụng driver phiên bản 0.4.6:

- https://mvnrepository.com/artifact/com.clickhouse/clickhouse-jdbc/0.4.6

Tải và đưa vào folder `jars` của spark

> ls -lh $PARK_HOME/jars/clickhouse-jdbc-0.4.6.jar

### Kiểm tra

Ta sử dụng Spark Shell để đọc dữ liệu bảng `dbtest`.`abc` trên server Clickhouse `10.10.10.100`:

> spark-shell

```
scala> val df = spark.read.format("jdbc").option("url", "jdbc:clickhouse://10.10.10.100:8123/dbtest").option("dbtable", "abc").option("user", "default").option("password", "Default@2023").option("driver", "com.clickhouse.jdbc.ClickHouseDriver").load()
scala> df.count()
res7: Long = 54647149
```
