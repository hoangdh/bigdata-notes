## Kiểm tra các thư viện nén trong HBase

### Cách sử dụng

```sh
$ hbase org.apache.hadoop.hbase.util.CompressionTest
Usage: CompressionTest <path> lzo|gz|none|snappy|lz4|bzip2|zstd
For example:
  hbase class org.apache.hadoop.hbase.util.CompressionTest file:///tmp/testfile gz
```

Trong ví dụ, ta kiểm tra 2 thư viện nén là `gz` và `snappy`.

```
$ hbase org.apache.hadoop.hbase.util.CompressionTest file:///tmp/ahihi.txt gz
2022-11-21 11:26:51,194 DEBUG [main] util.ClassSize: Using Unsafe to estimate memory layout
2022-11-21 11:26:51,520 INFO  [main] metrics.MetricRegistries: Loaded MetricRegistries class org.apache.hadoop.hbase.metrics.impl.MetricRegistriesImpl
2022-11-21 11:26:51,630 DEBUG [main] hfile.HFile: Unable to set drop behind on ahihi.txt
2022-11-21 11:26:51,726 INFO  [main] zlib.ZlibFactory: Successfully loaded & initialized native-zlib library
2022-11-21 11:26:51,774 INFO  [main] compress.CodecPool: Got brand-new compressor [.gz]
2022-11-21 11:26:51,785 INFO  [main] compress.CodecPool: Got brand-new compressor [.gz]
2022-11-21 11:26:52,109 INFO  [main] compress.CodecPool: Got brand-new decompressor [.gz]
SUCCESS


$ hbase org.apache.hadoop.hbase.util.CompressionTest file:///tmp/ahihi.txt snappy
2022-11-21 11:26:59,953 DEBUG [main] util.ClassSize: Using Unsafe to estimate memory layout
2022-11-21 11:27:00,311 INFO  [main] metrics.MetricRegistries: Loaded MetricRegistries class org.apache.hadoop.hbase.metrics.impl.MetricRegistriesImpl
2022-11-21 11:27:00,423 DEBUG [main] hfile.HFile: Unable to set drop behind on ahihi.txt
2022-11-21 11:27:00,511 INFO  [main] compress.CodecPool: Got brand-new compressor [.snappy]
2022-11-21 11:27:00,520 INFO  [main] compress.CodecPool: Got brand-new compressor [.snappy]
2022-11-21 11:27:00,769 INFO  [main] compress.CodecPool: Got brand-new decompressor [.snappy]
SUCCESS
```

### Tham khảo:
- https://www.ibm.com/docs/en/imdm/11.6?topic=v11504-verifying-snappy-compression-library
