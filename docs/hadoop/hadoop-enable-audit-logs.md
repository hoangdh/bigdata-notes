## Bật audit log cho HDFS

- Thêm tùy chọn sau vào file `hadoop-env.sh`:

```
...
export HDFS_NAMENODE_OPTS="$HDFS_NAMENODE_OPTS -Dhdfs.audit.logger=${HDFS_AUDIT_LOGGER:-INFO,RFAAUDIT}"
```

 - Kiểm tra `log4j.properties`

 ```
log4j.appender.DRFAAUDIT=org.apache.log4j.RollingFileAppender
log4j.appender.DRFAAUDIT.MaxFileSize=100MB
log4j.appender.DRFAAUDIT.MaxBackupIndex=10
 ```
 
- Khởi động Namenode cụm HDFS.

Ref: https://blog.csdn.net/u011281987/article/details/30034143
