## Sửa lỗi `Failed to obtain user group information: java.io.IOException: Security enabled but user not authenticated by filter`

### Nguyên nhân

Theo thử nghiệm cá nhân; từ bản Hadoop 3.2.1+; Hadoop Secure bắt buộc phải xác thực mới có thể xem Web UI của Namenode. Lỗi trên xuất hiện khi ta cấu hình `hadoop.http.authentication.type` là `simple`.

### Khắc phục

Để khắc phục, ta sửa lại phương thức xác thực về `kerberos`. Tham khảo thêm các [cấu hình liên quan](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html).

```
...
  <property>
    <name>hadoop.http.authentication.type</name>
    <value>kerberos</value>
  </property>
...
```

Khởi động lại Namenode và Datanode.
