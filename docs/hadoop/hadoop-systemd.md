## Sử dụng SystemD để quản lý Hadoop HDFS

Trong ví dụ này, thông tin môi trường như sau:

- Thư mục cài đặt Hadoop: /opt/hadoop
- User: hdfs

### Các bước tiến hành

- Chỉnh sửa chỗ lưu file PID về `$HADOOP_HOME/pid`

```
export HADOOP_HOME=/opt/hadoop
mkdir -p $HADOOP_HOME/pid
chown -R hdfs: $HADOOP_HOME/pid
```

Sửa thông tin trong file `hadoop-env.sh`

```
...
export HADOOP_PID_DIR=${HADOOP_HOME}/pid
...
```

- File mẫu cho Namenode - `/etc/systemd/system/hadoop-namenode.service`

```
[Unit]
Description=Hadoop Namenode Service
After=network.target

[Service]
Type=simple
User=hdfs
PIDFile=/opt/hadoop/pid/hadoop-hdfs-namenode.pid
ExecStart=/opt/hadoop/bin/hdfs --daemon start namenode
ExecStop=/opt/hadoop/bin/hdfs --daemon stop namenode
Restart=on-abort

[Install]
WantedBy=multi-user.target
```

- File mẫu cho Datanode - `/etc/systemd/system/hadoop-datanode.service`

```
[Unit]
Description=Hadoop Datanode Service
After=network.target

[Service]
Type=simple
User=hdfs
PIDFile=/opt/hadoop/pid/hadoop-hdfs-datanode.pid
ExecStart=/opt/hadoop/bin/hdfs --daemon start datanode
ExecStop=/opt/hadoop/bin/hdfs --daemon stop datanode
Restart=on-abort

[Install]
WantedBy=multi-user.target
```

- Reload lại systemd

> systemctl daemon-reload
