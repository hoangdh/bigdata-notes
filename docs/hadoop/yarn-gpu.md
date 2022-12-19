# Cấu hình YARN NodeManager với GPU

## 1. Cài đặt Driver của Nvidia
## 2. Cài đặt và cáu hình Hadoop YARN NodeManager

### 3.1 Tạo thư mục khởi tạo môi trường cho YARN:

```
mkdir -p /sys/fs/cgroup/devices/yarn
chown yarn: /sys/fs/cgroup/devices/yarn
```

**Lưu ý**: Thay thế tên user `yarn` phù hợp với môi trường hiện tại

### 3.2 Cấu hình sử dụng GPU

- Tạo file khai báo sử dụng tài nguyên GPU `resource-types.xml` trong thư mục `$HADOOP_HOME/etc/hadoop`

> vim $HADOOP_HOME/etc/hadoop/resource-types.xml

```
<configuration>
  <property>
     <name>yarn.resource-types</name>
     <value>yarn.io/gpu</value>
  </property>
</configuration>
```

- Thêm cấu hình về thông tin GPU

```
</onfiguration>
...
        <!-- GPU Configuration -->
        <property>
                <name>yarn.nodemanager.resource-plugins</name>
                <value>yarn.io/gpu</value>
        </property>
        <property>
                <name>yarn.nodemanager.resource-plugins.gpu.allowed-gpu-devices</name>
                <value>auto</value>
        </property>
        <property>
                <name>yarn.nodemanager.resource-plugins.gpu.path-to-discovery-executables</name>
                <value>$(which nvidia-smi)</value>
        </property>
        <property>
                <name>yarn.nodemanager.linux-container-executor.cgroups.mount</name>
                <value>true</value>
        </property>
        <property>
                <name>yarn.nodemanager.linux-container-executor.cgroups.mount-path</name>
                <value>/sys/fs/cgroup</value>
        </property>
        <property>
                <name>yarn.nodemanager.linux-container-executor.cgroups.hierarchy</name>
                <value>yarn</value>
        </property>
</configuration>
```

**Bài viết đang hoàn thiện.**

## Tham khảo
- https://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/UsingGpus.html
