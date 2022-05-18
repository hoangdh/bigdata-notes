# Cấu hình Log Rotate cho ZooKeeper

### Thêm thông tin vào file log4j.properties

> vi conf/log4j.properties

```
# Max log file size of 100MB
log4j.appender.ROLLINGFILE.MaxFileSize=100MB
# uncomment the next line to limit number of backup files
log4j.appender.ROLLINGFILE.MaxBackupIndex=10
```

### Sửa thông tin cấu hình log

Thêm giá trị **ROLLINGFILE** vào biến `ZOO_LOG4J_PROP`

> vi bin/zkEnv.sh

```
...
ZOO_LOG4J_PROP="INFO,ROLLINGFILE"
...
```

### Khởi động lại ZooKeeper

```
bin/zkServer.sh restart
```

### Xem trạng thái của ZK

```
bin/zkServer.sh status
```

### Tham khảo:
- https://support.imply.io/hc/en-us/articles/360039404753-Enabling-Apache-Zookeeper-logginglogging
