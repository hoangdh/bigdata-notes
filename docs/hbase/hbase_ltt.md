## Sử dụng HBase TLL (Test Load Tool)


### Đọc ngẫu nhiên 

- Đọc ngẫu nhiêu 2M rows với 10 luồng

```
/data/softs/hbase-2.3.6/bin/hbase ltt -zk hbase.hoangdh.internal:2181 -zk_root /hbase -tn hoangdh:ltt -read 10 -skip_init -num_keys 2000000
```

### Đếm số row của bảng

Yêu cầu: Phải có YARN

```
hbase-2.3.6/bin/hbase org.apache.hadoop.hbase.mapreduce.RowCounter -Dhbase.client.zookeeper.quorum=hbase.hoangdh.internal:2189 -Dzookeeper.znode.parent=/hbase hoangdh:ltt
```
