## Bật audit log cho HDFS

- Thêm tùy chọn sau vào file `hadoop-env.sh`:

```
...
export HDFS_NAMENODE_OPTS="$HDFS_NAMENODE_OPTS -Dhdfs.audit.logger=${HDFS_AUDIT_LOGGER:-INFO,RFAAUDIT}"
```

Với bản 3.3.5:

```
...
export HDFS_AUDIT_LOGGER=INFO,RFAAUDIT
...
```


 - Kiểm tra `log4j.properties`
 
 Ta có thể thay đổi kích thước và số lượng file log tại cấu hình. Trong ví dụ này, ta đặt kích thước mỗi file là 100M với số lượng là 10.

 ```
log4j.appender.DRFAAUDIT=org.apache.log4j.RollingFileAppender
log4j.appender.DRFAAUDIT.MaxFileSize=100MB
log4j.appender.DRFAAUDIT.MaxBackupIndex=10
 ```
 
- Khởi động Namenode cụm HDFS.

## Tham khảo: 
- https://www.ibm.com/docs/en/fci/6.5.0?topic=administering-hdfs-audit-logging
- https://blog.csdn.net/u011281987/article/details/30034143
