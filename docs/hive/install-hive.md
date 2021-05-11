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

Chúng ta tải về bản 3.1.2 - mới nhất trong thời điểm viết bài.

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

export PATH=$HIVE_HOME/bin:$JAVA_HOME/bin:$PATH
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
FLUSH PRIVILEGES:
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
</configuration>
```

- Thiết lập khởi tạo Schema cho Hive

```
schematool -initSchema -dbType mysql
```
