## Bật tính năng phân quyền - Authorization

Thêm cấu hình sau vào `hbase-site.xml`

```
hbase.security.authentication
simple

hbase.security.authorization
true

hbase.coprocessor.region.classes
org.apache.hadoop.hbase.security.access.AccessController

hbase.coprocessor.regionserver.classes
org.apache.hadoop.hbase.security.access.AccessController
```

Bản 2.x

```
<property>
     <name>hbase.security.authorization</name>
     <value>true</value>
</property>
<property>
  <name>hbase.security.exec.permission.checks</name>
  <value>true</value>
</property>
<property>
     <name>hbase.coprocessor.master.classes</name>
     <value>org.apache.hadoop.hbase.security.access.AccessController</value>
</property>
<property>
     <name>hbase.coprocessor.region.classes</name>
     <value>org.apache.hadoop.hbase.security.token.TokenProvider,org.apache.hadoop.hbase.security.access.AccessController</value>
</property>

```

### Cho phép ghi log Audit

Thay thế phần cấu hình log audit hiện có trong `log4j.properties`

Ref: https://stackoverflow.com/questions/61453540/enable-to-view-the-audit-trace-logs-for-hbase-accessconroller

```
#
# Security audit appender
#
hbase.security.log.file=SecurityAuth.audit
hbase.security.log.maxfilesize=256MB
hbase.security.log.maxbackupindex=10
log4j.appender.RFAS=org.apache.log4j.RollingFileAppender
log4j.appender.RFAS.File=${hbase.log.dir}/${hbase.security.log.file}
log4j.appender.RFAS.MaxFileSize=${hbase.security.log.maxfilesize}
log4j.appender.RFAS.MaxBackupIndex=${hbase.security.log.maxbackupindex}
log4j.appender.RFAS.layout=org.apache.log4j.PatternLayout
log4j.category.SecurityLogger=TRACE,RFAS
log4j.appender.RFAS.layout.ConversionPattern=%d{ISO8601} %p %c: %m%n
log4j.additivity.SecurityLogger=false
log4j.logger.SecurityLogger.org.apache.hadoop.hbase.security.access.AccessController=TRACE
log4j.logger.SecurityLogger.org.apache.hadoop.hbase.security.visibility.VisibilityController=TRACE
...
```


