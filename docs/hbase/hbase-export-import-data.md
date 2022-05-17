# Sử dụng `Import` và `Export` trong HBase
**Yêu cầu: YARN**

## 1. Export dữ liệu

Cú pháp:

```
hbase org.apache.hadoop.hbase.coprocessor.Export <tablename> <outputdir> [<versions> [<starttime> [<endtime>]]]
```
 
Ví dụ:
  
```
/hbase org.apache.hadoop.hbase.mapreduce.Export -Dhbase.zookeeper.quorum=10.10.20.10:2181 -Dzookeeper.znode.parent=/hbase hoangdh_export hdfs://10.10.10.10:8020/tmp/hoangdh_export/ 1 1651481316727 1652777349936
```

Cách lấy thời gian bằng Bash
  
```
hoangdh@nothing:~$ date --date="15 days ago" +%s%N | cut -b1-13
1651481316727
hoangdh@nothing:~$ date +%s%N | cut -b1-13
1652777349936
```
  
## 2. Import dữ liệu
  
Cú pháp:

```
hbase org.apache.hadoop.hbase.mapreduce.Import <tablename> <inputdir>
```
**Lưu ý:** Tạo bảng tương đương trên cụm đích trước khi nhập dữ liệu.
  
Ví dụ:
  
```
hbase org.apache.hadoop.hbase.mapreduce.Import -Dhbase.zookeeper.quorum=10.10.10.10:2181 -Dzookeeper.znode.parent=/hbase hoangdh_export hdfs://10.10.10.10:8020/tmp/hoangdh_export
```
  
 ## 3. Kiểm tra
  
  Để xác minh dữ liệu đã được chuyển sang cụm mới, ta sử dụng `RowCounter`.
  
  Cụm nguồn:
  
  ```
hbase org.apache.hadoop.hbase.mapreduce.RowCounter -Dhbase.zookeeper.quorum=10.10.20.10:2181 -Dzookeeper.znode.parent=/hbase hoangdh_export
  ```
  
  Cụm đích:
  
  ```
hbase org.apache.hadoop.hbase.mapreduce.RowCounter -Dhbase.zookeeper.quorum=10.10.10.10:2181 -Dzookeeper.znode.parent=/hbase hoangdh_export
  ```
  
  ## 4. Tham khảo:
  - https://hbase.apache.org/book.html#tools
