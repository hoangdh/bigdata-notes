# Cấu hình YARN NodeManager với GPU

## 1. Cài đặt Driver của Nvidia

- Trên Ubuntu LTS: https://docs.nvidia.com/datacenter/tesla/tesla-installation-notes/index.html#ubuntu-lts
- Xem hướng dẫn ngắn gọn: https://gist.github.com/hoangdh/811a564f93637d6fd36bd64661e316ec

## 2. Cài đặt và cấu hình Hadoop YARN NodeManager

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
<configuration>
...

<configuration>
         <property>
                <name>yarn.resourcemanager.hostname</name>
		 <value>10.10.10.100</value>
        </property>
         <property>
                <name>yarn.resourcemanager.scheduler.class</name>
                <value>org.apache.hadoop.yarn.server.resourcemanager.scheduler.capacity.CapacityScheduler</value>
        </property>
         <property>
                <name>yarn.scheduler.minimum-allocation-mb</name>
                <value>512</value>
        </property>
         <property>
                <name>yarn.scheduler.maximum-allocation-mb</name>
                <value>2048</value>
        </property>
         <property>
		<name>yarn.nodemanager.resource.memory-mb</name>
                <value>2048</value>
        </property>
         <property>
                <name>yarn.nodemanager.resource.cpu-vcores</name>
                <value>2</value>
        </property>
	<property>
                <name>yarn.nodemanager.local-dirs</name>
                <value>/opt/hadoop/data_secure/tmp</value>
        </property>
      	<property>
	  	 <name>yarn.nodemanager.aux-services</name>
                 <value>mapreduce_shuffle</value>
        </property>
        <property>
                <name>yarn.nodemanager.env-whitelist</name>
        	   <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
        </property>
        <property>
                <name>yarn.resourcemanager.principal</name>
		<value>yarn/_HOST@SECURE.LAB</value>
        </property>
        <property>
                <name>yarn.resourcemanager.keytab</name>
                <value>/opt/hadoop/data_secure/security/yarn.keytab</value>
        </property>
        <property>
                <name>yarn.nodemanager.principal</name>
                <value>yarn/_HOST@SECURE.LAB</value>
        </property>
        <property>
                <name>yarn.nodemanager.keytab</name>
                <value>/opt/hadoop/data_secure/security/yarn.keytab</value>
        </property>
        <property>
                <name>yarn.nodemanager.container-executor.class</name>
                <value>org.apache.hadoop.yarn.server.nodemanager.LinuxContainerExecutor</value>
	      </property>
        <property>
                <name>yarn.nodemanager.linux-container-executor.group</name>
                <value>hadoop</value>
        </property>
        <property>
                <name>yarn.webapp.ui2.enable</name>
                <value>true</value>
	</property>
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
