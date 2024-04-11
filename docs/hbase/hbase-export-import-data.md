# Sử dụng `Import` và `Export` trong HBase
**Yêu cầu: YARN**

Trong ví dụ này, chúng ta có 2 cụm HBase với IP. Cụm nguồn: 10.10.10.10; cụm đich: 10.10.20.10

- Nếu không sử dụng YARN: Thêm tùy chọn này `-Dmapreduce.framework.name=local` vào mỗi câu lệnh phía dưới.

## 1. Export dữ liệu

Cú pháp:

```
hbase org.apache.hadoop.hbase.coprocessor.Export <tablename> <outputdir> [<versions> [<starttime> [<endtime>]]]
```
 
Ví dụ:
  
```
hbase org.apache.hadoop.hbase.mapreduce.Export -Dhbase.zookeeper.quorum=10.10.10.10:2181 -Dzookeeper.znode.parent=/hbase test_export hdfs://10.10.20.10:8020/tmp/hoangdh_export/ 1 1651481316727 1652777349936
```

#### Cách lấy thời gian bằng Bash

Trong ví dụ này, chúng ta lấy các dữ liệu đã được ghi vào trong khoảng thời gian 15 ngày truớc thời điểm viết bài.
  
```
hoangdh@nothing:~$ date --date="15 days ago" +%s%N | cut -b1-13
1651481316727
hoangdh@nothing:~$ date +%s%N | cut -b1-13
1652777349936
```

### Export lên S3 MinIO

**Môi trường:**
- HBase 2.3.6
- https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/2.9.0/hadoop-aws-2.9.0.jar
- https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk/1.11.934/aws-java-sdk-1.11.934.jar 

Khai báo thông tin S3 vào câu lệnh. Trong ví dụ; export bảng **test_export** ra bucket **ahihi**.

```
hbase org.apache.hadoop.hbase.mapreduce.Export \
-Dfs.s3a.access.key=acesskey \
-Dfs.s3a.secret.key=SecretKey \
-Dfs.s3a.endpoint=http://10.10.10.101:9000 \
-Dfs.s3a.connection.ssl.enabled=false \
-Dfs.s3a.impl=org.apache.hadoop.fs.s3a.S3AFileSystem \
-Dfs.s3a.path.style.access=true \
-Dhbase.zookeeper.quorum=10.10.10.10:2181 \
-Dzookeeper.znode.parent=/hbase \
-Dmapreduce.output.fileoutputformat.compress=true \
-Dmapreduce.output.fileoutputformat.compress.codec=org.apache.hadoop.io.compress.SnappyCodec \
-Dmapreduce.output.fileoutputformat.compress.type=BLOCK \
test_export s3a://ahihi/test_export_snappy 1
```

#### Bonus: Sử dụng Snapshot

- Tạo snapshot

```
echo "snapshot 'test_export', 'test_export-snapshot-20220813'" | hbase shell -n
```

- Đẩy snapshot lên S3 MinIO

```
hbase org.apache.hadoop.hbase.snapshot.ExportSnapshot \
-Dfs.s3a.access.key=acesskey \
-Dfs.s3a.secret.key=SecretKey \
-Dfs.s3a.endpoint=http://10.10.10.101:9000 \
-Dfs.s3a.connection.ssl.enabled=false \
-Dfs.s3a.impl=org.apache.hadoop.fs.s3a.S3AFileSystem \
-Dfs.s3a.path.style.access=true \
-Dmapreduce.map.memory.mb=2048 \
-snapshot test_export-snapshot-20220813 \
-copy-to s3a://ahihi/snapshot \
-copy-from hdfs://10.10.10.10:8020/hbase \
-mappers 4
```

- Kéo snapshot từ S3 MinIO về HDFS

```
hbase org.apache.hadoop.hbase.snapshot.ExportSnapshot \
-Dfs.s3a.access.key=acesskey \
-Dfs.s3a.secret.key=SecretKey \
-Dfs.s3a.endpoint=http://10.10.10.101:9000 \
-Dfs.s3a.connection.ssl.enabled=false \
-Dfs.s3a.impl=org.apache.hadoop.fs.s3a.S3AFileSystem \
-Dfs.s3a.path.style.access=true \
-Dmapreduce.map.memory.mb=2048 \
-Ddfs.replication=2 \
-snapshot test_export-snapshot-20220813 \
-copy-from s3a://ahihi/snapshot \
-copy-to hdfs://10.10.20.10:8020/hbase \
-mappers 4
```

## 2. Import dữ liệu
  
Cú pháp:

```
hbase org.apache.hadoop.hbase.mapreduce.Import <tablename> <inputdir>
```
**Lưu ý:** Tạo bảng tương đương trên cụm đích trước khi nhập dữ liệu.
  
Ví dụ:
  
```
hbase org.apache.hadoop.hbase.mapreduce.Import -Dhbase.zookeeper.quorum=10.10.20.10:2181 -Dzookeeper.znode.parent=/hbase test_export /tmp/test_export
```

### Bonus: Sử dụng CopyTable

```
Usage: CopyTable [general options] [--starttime=X] [--endtime=Y] [--new.name=NEW] [--peer.adr=ADR] <tablename | snapshotName>

Options:
 rs.class     hbase.regionserver.class of the peer cluster
              specify if different from current cluster
 rs.impl      hbase.regionserver.impl of the peer cluster
 startrow     the start row
 stoprow      the stop row
 starttime    beginning of the time range (unixtime in millis)
              without endtime means from starttime to forever
 endtime      end of the time range.  Ignored if no starttime specified.
 versions     number of cell versions to copy
 new.name     new table's name
 peer.adr     Address of the peer cluster given in the format
              hbase.zookeeper.quorum:hbase.zookeeper.client.port:zookeeper.znode.parent
 families     comma-separated list of families to copy
              To copy from cf1 to cf2, give sourceCfName:destCfName. 
              To keep the same name, just give "cfName"
 all.cells    also copy delete markers and deleted cells
 bulkload     Write input into HFiles and bulk load to the destination table
 snapshot     Copy the data from snapshot to destination table.

Args:
 tablename    Name of the table to copy

Examples:
 To copy 'TestTable' to a cluster that uses replication for a 1 hour window:
 $ hbase org.apache.hadoop.hbase.mapreduce.CopyTable --starttime=1265875194289 --endtime=1265878794289 --peer.adr=server1,server2,server3:2181:/hbase --families=myOldCf:myNewCf,cf2,cf3 TestTable 
 To copy data from 'sourceTableSnapshot' to 'destTable': 
 $ hbase org.apache.hadoop.hbase.mapreduce.CopyTable --snapshot --new.name=destTable sourceTableSnapshot
 To copy data from 'sourceTableSnapshot' and bulk load to 'destTable': 
 $ hbase org.apache.hadoop.hbase.mapreduce.CopyTable --new.name=destTable --snapshot --bulkload sourceTableSnapshot
For performance consider the following general option:
  It is recommended that you set the following to >=100. A higher value uses more memory but
  decreases the round trip time to the server and may increase performance.
    -Dhbase.client.scanner.caching=100
  The following should always be set to false, to prevent writing data twice, which may produce 
  inaccurate results.
    -Dmapreduce.map.speculative=false
```

Ví dụ:

Tạo sẵn bảng trên cụm đích.

```
hbase org.apache.hadoop.hbase.mapreduce.CopyTable -Dhbase.zookeeper.quorum=10.10.1.10:2181 -Dzookeeper.znode.parent=/hbase -Dmapreduce.job.queuename=queue1 --peer.adr=10.10.2.10:2181:/hbase --new.name=hoangdh_test_copytable hoangdh_test
```

## 3. Kiểm tra
  
Để xác minh dữ liệu đã được chuyển sang cụm mới, chúng ta sử dụng `RowCounter` - đếm tổng số rowkey của bảng.

Cụm nguồn:

```
hbase org.apache.hadoop.hbase.mapreduce.RowCounter -Dhbase.zookeeper.quorum=10.10.10.10:2181 -Dzookeeper.znode.parent=/hbase test_export
```

Cụm đích:

```
hbase org.apache.hadoop.hbase.mapreduce.RowCounter -Dhbase.zookeeper.quorum=10.10.20.10:2181 -Dzookeeper.znode.parent=/hbase test_export
```

## Lỗi 

Trong lúc chuyển dữ liệu từ HBase trên cụm Secure; lỗi `Error: Can't get Master Kerberos principal for use as renewer` xuất hiện. Lỗi này do thiếu file yarn-site.yml trong cấu hình của HBase ($HBASE_HOME/conf)

## 4. Tham khảo:
- https://hbase.apache.org/book.html#tools
- https://support.h2o.ai/support/solutions/articles/17000100343-error-can-t-get-master-kerberos-principal-for-use-as-renewer
