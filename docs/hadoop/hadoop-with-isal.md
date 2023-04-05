# Biên dịch thư viện Hadoop với ISA-L trên Ubuntu 20.04

## 1. Cài đặt môi trường

### 1.1 Cài đặt công cụ build-essentisal

> apt install -y build-essential wget git pkgconf cmake yasm nasm

### 1.2 Cài đặt Java

- Theo [hướng dẫn](/docs/common/setup-java.md)

### 1.3 Cài đặt Maven

- Tải và giải nén Maven

```
curl -o /tmp/maven.tgz https://archive.apache.org/dist/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
mkdir -p /opt/maven/
tar -C /opt/maven/ -xzf /tmp/maven.tgz --strip-components=1
rm -f /tmp/maven.tgz 
```
- Thiết lập biến môi trường

```
cat > /etc/profile.d/maven.sh << EOF
export JAVA_HOME=/usr/jdk64/jdk1.8.0_202
export M2_HOME=/opt/maven
export MAVEN_HOME=/opt/maven
export PATH=\${M2_HOME}/bin:\${PATH}:\${JAVA_HOME}/bin
EOF
```

- Phân quyền thực thi cho script

```
chmod +x /etc/profile.d/maven.sh
bash /etc/profile.d/maven.sh
```


### 1.4 Cài đặt Protobuf

Hadoop vẫn chỉ hỗ trợ phiên bản Protobuf 2.5.0.

```
wget https://github.com/protocolbuffers/protobuf/releases/download/v2.5.0/protobuf-2.5.0.tar.gz                   
tar -xvzf protobuf-2.5.0.tar  
cd protobuf-2.5.0
./configure --prefix=/usr
make
make install
```

### 1.5 Biên dịch ISA-L 

```
git clone https://github.com/01org/isa-l.git
cd isa-l
make -f Makefile.unx
cp bin/libisal.so bin/libisal.so.2 /lib64
```

## 2. Biên dịch Hadoop 3.2.1

### 2.1 Tải Hadoop

```
wget https://archive.apache.org/dist/hadoop/common/hadoop-3.2.1/hadoop-3.2.1-src.tar.gz -P /opt
```

### 2.2 Biên dịch với ISA-L

```
cd /opt
tar -xzf hadoop-3.2.1-src.tar.gz
cd hadoop-3.2.1-src/
mvn clean package -Pdist,native -DskipTests -Dtar -Disal.lib=/lib64/ -Dbundle.isal=true
```

Sau khi biên dịch thành công, ta có thể lấy bản binary tại `hadoop-3.2.1-src/hadoop-dist/target`.

> ls -l /opt/hadoop-3.2.1-src/hadoop-dist/target

Và kết quả ta nhận được:

```
total 308184
drwxr-xr-x 2 hoangdh hoangdh      4096 Dec 26 00:47 antrun
drwxr-xr-x 3 hoangdh hoangdh      4096 Dec 26 00:47 classes
drwxr-xr-x 9 hoangdh hoangdh      4096 Dec 26 00:47 hadoop-3.2.1
-rw-r--r-- 1 hoangdh hoangdh 315552460 Dec 26 00:47 hadoop-3.2.1.tar.gz
drwxr-xr-x 3 hoangdh hoangdh      4096 Dec 26 00:47 maven-shared-archive-resources
drwxr-xr-x 3 hoangdh hoangdh      4096 Dec 26 00:47 test-classes
drwxr-xr-x 2 hoangdh hoangdh      4096 Dec 26 00:47 test-dir
```

Kiểm tra thư viện:

> /opt/hadoop-3.2.1-src/hadoop-dist/target/hadoop-3.2.1# bin/hadoop checknative

```
2021-12-26 18:53:56,599 WARN bzip2.Bzip2Factory: Failed to load/initialize native-bzip2 library system-native, will use pure-Java version
2021-12-26 18:53:56,607 INFO zlib.ZlibFactory: Successfully loaded & initialized native-zlib library
Native library checking:
hadoop:  true /data/softs/hadoop-3.2.1-src/hadoop-dist/target/hadoop-3.2.1/lib/native/libhadoop.so.1.0.0
zlib:    true /lib/x86_64-linux-gnu/libz.so.1
zstd  :  false 
snappy:  true /usr/lib/x86_64-linux-gnu/libsnappy.so.1
lz4:     true revision:10301
bzip2:   false 
openssl: true /usr/lib/x86_64-linux-gnu/libcrypto.so
ISA-L:   true /data/softs/hadoop-3.2.1-src/hadoop-dist/target/hadoop-3.2.1/lib/native/libisal.so.2
```

### Bonus: Biên dịch sử dụng Docker

- Cài đặt Docker
- Tải mã nguồn và biên dịch

Kích hoạt cả Snappy, ISAL, ZSTD, OpenSSL 

```
cd /opt
tar -xzf hadoop-3.2.1-src.tar.gz
cd hadoop-3.2.1-src/
./start-build-env.sh
mvn clean package -Pdist,native -DskipTests -Dtar -Dbundle.isal=true -Dbundle.snappy=true -Dbundle.zstd=true -Dbundle.openssl=true -Disal.lib=/usr/lib/ -Dopenssl.lib=/usr/lib/x86_64-linux-gnu/ -Dzstd.lib=/usr/lib/x86_64-linux-gnu/ -Dsnappy.lib=/usr/lib/x86_64-linux-gnu/
```

### 3. Tham khảo:
- /opt/hadoop-3.2.1-src/BUILDING.txt
- https://www.ercoppa.org/posts/how-to-compile-apache-hadoop-on-ubuntu-linux.html
- https://blog.csdn.net/sinat_28603977/article/details/105536003
