## Tạo bảng với parquet file trên HDFS

- **Thư mục trên HDFS:** hdfs://ml-dev/data/parquet/da/

```
set dfs.nameservices=ml-dev;
set dfs.ha.namenodes.ml-dev=nn1,nn2;
set dfs.namenode.rpc-address.ml-dev.nn1=10.10.10.1:8020;
set dfs.namenode.rpc-address.ml-dev.nn2=10.10.10.2:8020;
set dfs.client.failover.proxy.provider.ml-dev=org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider;

CREATE EXTERNAL TABLE tbl_test (userId string, userName string, age long, address long) STORED AS PARQUET LOCATION 'hdfs://ml-dev/data/parquet/da/';
```
