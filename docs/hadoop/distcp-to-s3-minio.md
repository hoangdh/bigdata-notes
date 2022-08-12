## Sao chép dữ liệu từ HDFS lên S3 MinIO

### 1. Chuẩn bị
- MinIO
- Hadoop 3.3.2

#### Khai báo thư viện cho Hadoop:

Sử dụng biến **HADOOP_OPTIONAL_TOOLS** trong **hadoop-env.sh**

**Tham khảo:** https://stackoverflow.com/a/57114787

### 2. Sao chép dữ liệu sang S3 MinIO

```
hadoop distcp \
-Dfs.s3a.access.key=acesskey \
-Dfs.s3a.secret.key=SecretKey \
-Dfs.s3a.endpoint=http://10.10.10.101:9000 \
-Dfs.s3a.connection.ssl.enabled=false \
-Dfs.s3a.impl=org.apache.hadoop.fs.s3a.S3AFileSystem \
-Dfs.s3a.path.style.access=true \
hdfs://10.10.10.10:8020/tmp/hoangdh_rec s3a://hihi/hoangdh_rec
```

Bonus: Khai báo thông tin S3 MinIO vào `hdfs-site.xml`

```
<configuration>
...
  <property>
    <name>fs.s3a.access.key</name>
    <value>acesskey</value>
  </property>
  <property>
    <name>fs.s3a.secret.key</name>
    <value>SecretKey</value>
  </property>
  <property>
    <name>fs.s3a.endpoint</name>
    <value>http://10.10.10.101:9000</value>
  </property>
  <property>
    <name>fs.s3a.connection.ssl.enabled</name>
    <value>false</value>
  </property>
  <property>
    <name>fs.s3a.impl</name>
    <value>org.apache.hadoop.fs.s3a.S3AFileSystem</value>
  </property>
  <property>
    <name>fs.s3a.path.style.access</name>
    <value>true</value>
  </property>

</configuration>
```
