## Cài đặt Apache Phoenix với HBase và kích hoạt Phoenix QueryServer (PQS)

### 1. Chuẩn bị Apache Phoenix

Tiến hành tải gói binary của Apache Phoenix tại [trang chủ](http://phoenix.apache.org/download.html) và giải nén.

Trong hướng dẫn này, chúng tôi sử dụng HBase 2.3.5 - Bản stable ở thời điểm viết bài; cùng với Apache Phoenix 5.

```
mkdir /opt/softs/ -p
cd  /opt/softs/
wget http://www.apache.org/dyn/closer.lua/phoenix/phoenix-5.1.2/phoenix-hbase-2.3-5.1.2-bin.tar.gz
wget https://downloads.apache.org/phoenix/phoenix-queryserver-6.0.0/phoenix-queryserver-6.0.0-bin.tar.gz
tar -xvzf phoenix-hbase-2.3-5.1.2-bin.tar.gz
tar -xzvf phoenix-queryserver-6.0.0-bin.tar.gz

cp -r phoenix-queryserver-6.0.0-bin/* phoenix-hbase-2.3-5.1.2-bin/

ln -s /opt/softs/phoenix-hbase-2.3-5.1.2-bin/ /opt/softs/phoenix

```

### 2. Cài đặt và cấu hình

Sau khi giải nén thành công, ta đưa thư viện của `phoenix` vào HBase. Cụ thể, sao chép thư viện `phoenix-server-hbase-2.3-5.1.2.jar` vào HBase.

Ví dụ, trong hướng dẫn này. Ta làm như sau:

- Sao chép thư viện

```
cp /opt/softs/phoenix-hbase-2.3-5.1.2-bin/phoenix-server-hbase-2.3-5.1.2.jar /opt/softs/hbase/lib/
```

- Thêm một vài cấu hình cho HBase

> vi /opt/softs/hbase/config/hbase-site.xml

```
<configuration>
...
    <!-- Phoenix -->

    <property>
        <name>hbase.regionserver.wal.codec</name>
        <value>org.apache.hadoop.hbase.regionserver.wal.IndexedWALEditCodec</value>
    </property>

    <property>
        <name>phoenix.functions.allowUserDefinedFunctions</name>
        <value>true</value>
        <description>enable UDF functions</description>
    </property>

    <property>
        <name>hbase.rpc.controllerfactory.class</name>
        <value>org.apache.hadoop.hbase.ipc.controller.ServerRpcControllerFactory</value>
    </property>
</configuration>
```

- Sao chép `hbase-site.xml` vào thư mục bin của Phoenix:

```
cp /opt/softs/hbase/config/hbase-site.xml /opt/softs/phoenix/bin/hbase-site.xml
```

- Thêm vài biến môi trường liên quan tới Phoenix

> vi ~/.bashrc

```
...
export HBASE_CONF_DIR=/opt/hbase/conf
export PHOENIX_HOME=/data/softs/phoenix
export PATH=$PHOENIX_HOME/bin:$PATH
```

- Cập nhật các biến môi trường

> source ~/.bashrc

- KHỞi ĐỘNG LẠI TOÀN BỘ CỤM HBase

### 3. Kích hoạt Apache Phoenix

- Kích hoạt Phoenix

```
cd /opt/softs/phoenix
sqlline.py zk1,zk2,zk3:2181 ../examples/WEB_STAT.sql ../examples/WEB_STAT.csv
```

Thay thế ZK Quorum của bạn vào câu lệnh.

- Truy vấn thử bảng vừa tạo

```
0: jdbc:phoenix:zk1,zk2,zk3:2181> !sql select * from WEB_STAT;
```

- Kiểm tra các bảng metadata của Phoenix


> echo "list" | hbase shell -n

```
TABLE   
SYSTEM.CATALOG               
SYSTEM.CHILD_LINK                  
SYSTEM.FUNCTION               
SYSTEM.LOG                 
SYSTEM.MUTEX                      
SYSTEM.SEQUENCE                    
SYSTEM.STATS                     
SYSTEM.TASK     
WEB_STAT         
9 row(s)
Took 1.1279 seconds                 
```

- Kích hoạt Phoenix QueryServer (PQS)

```
queryserver.py start
```

- Kiểm tra port 8765

```
ss -nplt | grep 8765
```

- Hoặc sử dụng SQuirreL SQL Client

### 3. Tham khảo:

- http://phoenix.apache.org/installation.html
- http://phoenix.apache.org/server.html
- https://docs.cloudera.com/HDPDocuments/HDP2/HDP-2.6.5/bk_command-line-upgrade/content/configure-phoenix-25.html
- https://titanwolf.org/Network/Articles/Article?AID=703225d6-2617-442a-8d74-4bedab146f9a#gsc.tab=0
