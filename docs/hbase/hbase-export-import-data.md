# Sử dụng `Import` và `Export` trong HBase
**Yêu cầu: YARN**

Trong ví dụ này, chúng ta có 2 cụm HBase với IP. Cụm nguồn: 10.10.10.10; cụm đich: 10.10.20.10

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
  
 ## 3. Kiểm tra
  
  Để xác minh dữ liệu đã được chuyển sang cụm mới, chúng ta sử dụng `RowCounter`.
  
  Cụm nguồn:
  
  ```
hbase org.apache.hadoop.hbase.mapreduce.RowCounter -Dhbase.zookeeper.quorum=10.10.10.10:2181 -Dzookeeper.znode.parent=/hbase test_export
  ```
  
  Cụm đích:
  
  ```
hbase org.apache.hadoop.hbase.mapreduce.RowCounter -Dhbase.zookeeper.quorum=10.10.20.10:2181 -Dzookeeper.znode.parent=/hbase test_export
  ```
  
  ## 4. Tham khảo:
  - https://hbase.apache.org/book.html#tools
