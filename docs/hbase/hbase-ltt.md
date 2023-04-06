## Sử dụng HBase TLL (Test Load Tool)

> hbase-2.3.6/bin/hbase ltt

```
usage: hbase org.apache.hadoop.hbase.util.LoadTestTool <options>
Options:
 -v,--verbose                    Will display a full readout of logs, including ZooKeeper
 -zk <arg>                       ZK quorum as comma-separated host names without port numbers
 -zk_root <arg>                  name of parent znode in zookeeper
 -tn <arg>                       The name of the table to read or write
 -families <arg>                 The name of the column families to use separated by comma
 -write <arg>                    <avg_cols_per_key>:<avg_data_size>[:<#threads=20>]
 -read <arg>                     <verify_percent>[:<#threads=20>]
 -update <arg>                   <update_percent>[:<#threads=20>][:<#whether to ignore nonce collisions=0>]
 -init_only                      Initialize the test table only, don't do any loading
 -bloom <arg>                    Bloom filter type, one of [NONE, ROW, ROWCOL, ROWPREFIX_FIXED_LENGTH]
 -bloom_param <arg>              the parameter of bloom filter type
 -compression <arg>              Compression type, one of [LZO, GZ, NONE, SNAPPY, LZ4, BZIP2, ZSTD]
 -data_block_encoding <arg>      Encoding algorithm (e.g. prefix compression) to use for data blocks in the test column
                                 family, one of [NONE, PREFIX, DIFF, FAST_DIFF, ROW_INDEX_V1].
 -max_read_errors <arg>          The maximum number of read errors to tolerate before terminating all reader threads.
                                 The default is 10.
 -multiget_batchsize <arg>       Whether to use multi-gets as opposed to separate gets for every column in a row
 -key_window <arg>               The 'key window' to maintain between reads and writes for concurrent write/read
                                 workload. The default is 0.
 -multiput                       Whether to use multi-puts as opposed to separate puts for every column in a row
 -batchupdate                    Whether to use batch as opposed to separate updates for every column in a row
 -in_memory                      Tries to keep the HFiles of the CF inmemory as far as possible.  Not guaranteed that
                                 reads are always served from inmemory
 -generator <arg>                The class which generates load for the tool. Any args for this class can be passed as
                                 colon separated after class name
 -writer <arg>                   The class for executing the write requests
 -updater <arg>                  The class for executing the update requests
 -reader <arg>                   The class for executing the read requests
 -num_keys <arg>                 The number of keys to read/write
 -start_key <arg>                The first key to read/write (a 0-based index). The default value is 0.
 -skip_init                      Skip the initialization; assume test table already exists
 -num_tables <arg>               A positive integer number. When a number n is specified, load test tool  will load n
                                 table parallely. -tn parameter value becomes table name prefix. Each table name is in
                                 format <tn>_1...<tn>_n
 -encryption <arg>               Enables transparent encryption on the test table, one of [AES]
 -deferredlogflush               Enable deferred log flush.
 -num_regions_per_server <arg>   Desired number of regions per region server. Defaults to 5.
 -region_replication <arg>       Desired number of replicas per region
 -region_replica_id <arg>        Region replica id to do the reads from
 -mob_threshold <arg>            Desired cell size to exceed in bytes that will use the MOB write path
```

### Ghi ngẫu nhiên

Ghi 10 cột, size 500B, 5 luồng và 2M dòng.

```
hbase-2.3.6/bin/hbase ltt -zk hbase.hoangdh.internal:2181 -zk_root /hbase -tn hoangdh:ltt -write 10:500:5 -num_keys 2000000
```

### Đọc ngẫu nhiên 

- Đọc ngẫu nhiên 2M rows với 10 luồng

```
hbase-2.3.6/bin/hbase ltt -zk hbase.hoangdh.internal:2181 -zk_root /hbase -tn hoangdh:ltt -read 10 -skip_init -num_keys 2000000
```

### Đếm số row của bảng

Yêu cầu: Phải có YARN

```
hbase-2.3.6/bin/hbase org.apache.hadoop.hbase.mapreduce.RowCounter -Dhbase.client.zookeeper.quorum=hbase.hoangdh.internal:2189 -Dzookeeper.znode.parent=/hbase hoangdh:ltt
```
