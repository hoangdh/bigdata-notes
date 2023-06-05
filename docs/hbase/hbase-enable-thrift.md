## Bật Thrift cho HBase

- Sửa heapsize cho Thrift trong `hbase-env.sh`

```
...
export HBASE_THRIFT_OPTS="$HBASE_OTPS $HBASE_THRIFT_OPTS -Xms1g -Xmx2g"
...
```

- Thêm cấu hình vào `hbase-site.xml`

```
...
    <property>
        <name>hbase.regionserver.thrift.framed</name>
        <value>true</value>
    </property>
    <property>
        <name>hbase.regionserver.thrift.framed.max_frame_size_in_mb</name>
        <value>3</value>
    </property>
    <property>
        <name>hbase.regionserver.thrift.compact</name>
        <value>true</value>
    </property>
    <property>
        <name>hbase.thrift.server.socket.read.timeout</name>
        <value>90000</value>
    </property>
```

- Bật Thrift

> hbase-daemon.sh start thrift

- Tham khảo:
  - https://docs.cloudera.com/documentation/enterprise/6/6.3/topics/cdh_ig_hbase_troubleshooting.html#concept_ghf_w5s_z4
  - https://community.cloudera.com/t5/Support-QueQuestions/python-thrift-TSocket-read-0-bytes/m-p/61171
