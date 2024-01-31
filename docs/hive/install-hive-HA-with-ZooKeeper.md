## Hướng dẫn cài đặt Hive Metastore với MySQL

### 1. Chuẩn bị môi trường

- Cài đặt Hadoop HDFS và YARN
- Tạo user Unix cho hive

```
useradd -s /bin/bash -d /home/hive -m hive
mkdir -p /opt/softs/
```

- Tải hive từ trang chủ

```
https://downloads.apache.org/hive
```

Chúng ta tải về bản 3.1.2 - mới nhất tại thời điểm viết bài.

```
cd /opt/softs/
curl -O https://downloads.apache.org/hive/hive-3.1.2/apache-hive-3.1.2-bin.tar.gz
```

Giải nén và phân quyền cho user `hive`

```
tar -xzvf apache-hive-3.1.2-bin.tar.gz
chown hive:hive /opt/softs/apache-hive-3.1.2-bin
ln -s /opt/softs/apache-hive-3.1.2-bin /opt/softs/hive
```

- Thiết lập các biến môi trường cho `hive`

Sau khi tải xong, ta chuyển sang user `hive` và khai báo các biến môi trường sau:

```
su - hive
vi ~/.bashrc
```

Thêm nội dung sau vào file

```
...
export JAVA_HOME=/opt/softs/jdk1.8.0_112
export HIVE_HOME=/opt/softs/hive
export HIVE_DIR_CONF=$HIVE_HOME/conf

export HADOOP_HOME=/opt/softs/hadoop
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export YARN_CONF_DIR=$HADOOP_HOME/etc/hadoop

export PATH=$HIVE_HOME/bin:$JAVA_HOME/bin:$HADOOP/bin:$PATH
```

Lưu lại và thực thi lệnh để khai báo các biến

```
source ~/.bashrc
exit
```

- Tạo thư mục Data Warehouse cho `hive` trên HDFS;

```
hdfs dfs -mkdir -p /user/hive/warehouse
hdfs dfs -chown -R hive:hive /user/hive

```

- Tạo Database metastore và User hive trên MySQL

Cài đặt MySQL theo [hướng dẫn](https://github.com/hoangdh/ghichep-database/blob/master/docs/1-install-mysql-community-from-repository.md)

Đăng nhập vào mysql và thực hiện tạo 1 database và user hive

> mysql -u root -p

```
CREATE DATABASE hive_metastore CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
GRANT ALL ON hive_metastore.* TO hive@'%' IDENTIFIED BY 'Pa$$w0rdV3ry$trong';
FLUSH PRIVILEGES;
```

- Cài đặt Driver JDBC

Chúng ta sử dụng JDBC bản 5.x làm driver kết nối hive với MySQL.

```
cd /tmp
https://downloads.mysql.com/archives/get/p/3/file/mysql-connector-java-5.1.39.tar.gz
tar -xzvf mysql-connector-java-5.1.39.tar.gz
cp /tmp/mysql-connector-java-*/mysql-connector-java-*.jar $HIVE_HOME/lib/
[hive@mlbigdata7 tmp]$ ln -s /data/softs/hive/lib/mysql-connector-java-*.jar $HIVE_HOME/lib/mysql-connector-java.jar
```

### 2. Cấu hình hive

- Tạo file cấu hình `hive-site.xml` với nội dung như sau:

> su - hive

> vi $HIVE_HOME/conf/hive-site.xml

```
<configuration>
	<property>
    		<name>hive.metastore.warehouse.dir</name>
    		<value>/user/hive/warehouse</value>
	</property>
	<property>
		<name>hive.metastore.db.type</name>
    		<value>mysql</value>
	</property>
	<property>
		<name>javax.jdo.option.ConnectionURL</name>
		<value>jdbc:mysql://10.3.2.10:3306/hive_metastore?createDatabaseIfNotExist=true&amp;useSSL=false</value>
	</property>
	<property>
		<name>javax.jdo.option.ConnectionDriverName</name>
		<value>com.mysql.jdbc.Driver</value>
	</property>
	<property>
		<name>javax.jdo.option.ConnectionUserName</name>
		<value>hive</value>
	</property>
	<property>
		<name>javax.jdo.option.ConnectionPassword</name>
		<value>Pa$$w0rdV3ry$trong</value>
	</property>
        <property>
                <name>datanucleus.connectionPool.maxPoolSize</name>
                <value>5</value>
        </property>
	<property>
 		<name>datanucleus.autoCreateSchema</name>
  		<value>false</value>
	</property>
	<property>
		<name>datanucleus.fixedDatastore</name>
  		<value>true</value>
	</property>
	<property>
		<name>datanucleus.autoStartMechanism</name>
  		<value>SchemaTable</value>
	</property>
	<property>
    		<name>hive.exec.local.scratchdir</name>
		<value>/tmp/${user.name}</value>
	</property>
	<property>
 		 <name>hive.downloaded.resources.dir</name>
		 <value>/tmp/${user.name}_resources</value>
	</property>
        <property>
    		<name>hive.metastore.schema.verification</name>
    		<value>true</value>
	</property>
	<property>
  		<name>hive.server2.enable.doAs</name>
  		<value>false</value> 
  	</property>
</configuration>
```

**Lưu ý:** Thay thế thông tin phù hợp với môi trường hiện tại như thông tin MySQL, các folder /tmp,...

- Thiết lập khởi tạo Schema cho Hive

```
schematool -initSchema -dbType mysql
```

Chờ cho hive khởi tạo xong schema trên MySQL; thông báo thành công.

```
Initialization script completed
schemaTool completed
```

#### Lỗi không tương thích thư viện guava giữa Hadoop và Hive

Nếu thông báo sau xuất hiện:

```
“Exception in thread “main” java.lang.NoSuchMethodError: com.google.common.base.Preconditions.checkArgument(ZLjava/lang/String;Ljava/lang/Object;)V”
```

Ta kiểm tra thông tin thư viện giữa Hadoop và Hive

```
ls $HADOOP_HOME/share/hadoop/hdfs/lib/guava*
ls $HIVE_HOME/lib/guava*
```

Thực hiện xóa thư viện cũ và đưa thư viện tương thích vào HIVE_HOME

```
rm -rf $HIVE_HOME/lib/guava*
cp $HADOOP_HOME/share/hadoop/hdfs/lib/guava* $HIVE_HME/lib/
```

Sau đó khởi tạo lại DB với lệnh:

```
schematool -initSchema -dbType mysql
```

### 3. Khởi động HIVE và kiểm tra

#### Kiểm tra bằng câu lệnh hive

```
su - hive
hive
```

Kiểm tra các DB hiện có trong HIVE:

```
hive> show databases;
OK
default
```

#### Khởi động hiveserver2

```
mkdir $HIVE_HOME/log -p 
nohup $HIVE_HOME/bin/hive --service hiveserver2 --hiveconf hive.root.logger=INFO,console > $HIVE_HOME/log/hiveserver2.out 2> $HIVE_HOME/log/hiveserver2.log &
```

Kiểm tra và kết nối bằng beeline với port 10000 và GUI 10002

```
beeline
beeline> !connect jdbc:hive2://127.0.0.1:10000/default hive hive;
```

### 4. Cấu hình High Available cho HiveServer2

#### 4.1 Cài đặt Zookeeper Cluster
#### 4.2 Cấu hình Hive

Chúng ta cài thêm một máy chủ Hive mới theo các bước bên trên và thêm cấu hình High Available cho Hiveserver2 với ZooKeeper vào `hbase-site.xml`

```
<configuration>
	...
	<!-- HA with Zookeeper -->
	<property>
                <name>hive.server2.support.dynamic.service.discovery</name>
                <value>true</value>
        </property>
        <property>
                <name>hive.zookeeper.quorum</name>
                <value>zk1,zk2,zk3:2181</value>
	</property>
	<property>
                <name>hive.zookeeper.session.timeout</name>
                <value>3000</value>
        </property>
        <property>
		<name>hive.server2.zookeeper.namespace</name>
		<value>hiveserver2</value>
        </property>
</configuration>
```
**Chú ý**: Thay thế thông tin cụm ZooKeeper của bạn vào `zk1,zk2,zk3:2181`

Khởi động lại `hiveserver2` trên các server

```
pkill -f -9 hiveserver2

mkdir $HIVE_HOME/log -p 
nohup $HIVE_HOME/bin/hiveserver2 > $HIVE_HOME/log/hiveserver2.out 2> $HIVE_HOME/log/hiveserver2.log &
```

#### 4.4 Kiểm tra kết nối

```
beeline
beeline> !connect jdbc:hive2://zk1,zk2,zk3:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2 hive hive
```


### 5. Tham khảo

- https://docs.cloudera.com/HDPDocuments/HDP2/HDP-2.6.3/bk_command-line-installation/content/validate_installation.html
- https://phoenixnap.com/kb/install-hive-on-ubuntu
- https://stackoverflow.com/a/50753233
- http://www.mtitek.com/tutorials/bigdata/hive/install.php (Update)
- HA: 
  - https://www.ibm.com/docs/en/db2-big-sql/5.0.1?topic=availability-enabling-hiveserver2-high
  - https://docs.cloudera.com/HDPDocuments/HDP2/HDP-2.3.0/bk_hadoop-ha/content/ha-hs2-rolling-upgrade.html
  - https://community.cloudera.com/t5/Support-Questions/How-to-Enable-Zookeeper-Discovery-for-HiveServer2-HA/td-p/95392
