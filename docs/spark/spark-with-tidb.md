# Sử dụng TiSpark với TiDB

## 1. Chuẩn bị

- Spark 2.4.3
- Scala 2.12
- TiSpark 2.4.3: https://github.com/pingcap/tispark/releases/tag/v2.4.3

### 2. Cài đặt và cấu hình

- Tải thư viện bên trên vào `$SPARK_HOME/jars/`
- Cấu hình thông tin TiDB vào `$SPARK_HOME/conf/spark-defauts.conf`

```
...
spark.tispark.pd.addresses 10.10.10.1:2379,10.10.10.2:2379,10.10.10.3:2379
spark.sql.extensions org.apache.spark.sql.TiExtensions
spark.sql.catalog.tidb_catalog org.apache.spark.sql.catalyst.catalog.TiCatalog
spark.sql.catalog.tidb_catalog.pd.addresses 10.10.10.1:2379,10.10.10.2:2379,10.10.10.3:2379
```
**Chú ý**: Thay thế PD của TiBD.

- Thực hiện kiểm tra bằng `spark-shell`

> spark-shell --executor-memory 2G --executor-cores 1 --num-executors 2

Truy vấn vài câu đơn giản:

```
scala> spark.sql("show databases").show()
+------------+
|databaseName|
+------------+
|     default|
|        test|
|      testdb|
|       mysql|
|      dbtest|
+------------+


scala> spark.sql("use mysql")
22/06/10 14:52:21 WARN ObjectStore: Failed to get database mysql, returning NoSuchObjectException
res4: org.apache.spark.sql.DataFrame = []

scala> spark.sql("show tables").show()
22/06/10 14:52:25 WARN ObjectStore: Failed to get database mysql, returning NoSuchObjectException
+--------+--------------------+-----------+
|database|           tableName|isTemporary|
+--------+--------------------+-----------+
|   mysql|     analyze_options|      false|
|   mysql|           bind_info|      false|
|   mysql|capture_plan_base...|      false|
|   mysql|  column_stats_usage|      false|
|   mysql|        columns_priv|      false|
|   mysql|                  db|      false|
|   mysql|       default_roles|      false|
|   mysql|expr_pushdown_bla...|      false|
|   mysql|     gc_delete_range|      false|
|   mysql|gc_delete_range_done|      false|
|   mysql|       global_grants|      false|
|   mysql|         global_priv|      false|
|   mysql|    global_variables|      false|
|   mysql|          help_topic|      false|
|   mysql|  opt_rule_blacklist|      false|
|   mysql|          role_edges|      false|
|   mysql|  schema_index_usage|      false|
|   mysql|       stats_buckets|      false|
|   mysql|      stats_extended|      false|
|   mysql|      stats_feedback|      false|
+--------+--------------------+-----------+
only showing top 20 rows


scala> 

```

### 3. Tham khảo
- https://github.com/pingcap/tispark/blob/master/docs/userguide.md#demonstration
