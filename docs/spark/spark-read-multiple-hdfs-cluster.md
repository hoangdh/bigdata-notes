## Cấu hình đọc nhiều cụm HDFS với Spark

Để đọc được nhiều cụm, ta khai báo các Namespace tương ứng với mỗi cụm HDFS trong cấu hình spark. Cụ thể như sau:

- Mở file cấu hình spark

> vim $SPARK_HOME/conf/spark-defaults.conf

- Thêm cấu hình sau:

Lưu ý: Chỗ NameServices cần khai báo cụm HDFS mặc định của cụm YARN đang dùng. Trong ví dụ, cụm này tên là `da-spark`

```
...
spark.hadoop.dfs.nameservices da-spark,da1,da2
# DA 1
spark.hadoop.dfs.ha.namenodes.da1 nn1,nn2
spark.hadoop.dfs.namenode.rpc-address.da1.nn1 10.10.10.1:8020
spark.hadoop.dfs.namenode.rpc-address.da1.nn2 10.10.10.2:8020
spark.hadoop.dfs.client.failover.proxy.provider.da1 org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider

# DA 2
spark.hadoop.dfs.ha.namenodes.da2 nn1,nn2
spark.hadoop.dfs.namenode.rpc-address.da2.nn1 10.20.10.1:8020
spark.hadoop.dfs.namenode.rpc-address.da2.nn2 10.20.10.2:8020
spark.hadoop.dfs.client.failover.proxy.provider.da2 org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider
```
