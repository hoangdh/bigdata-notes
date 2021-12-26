# Biên dịch thư viện Hadoop với ISA-L trên Ubuntu 20.04

## 1. Cài đặt môi trường:

### 1.1 Cài đặt công cụ build-essentisal

> apt install -y build-essential wget git pkgconf cmake yasm nasm

### 1.2 Cài đặt Java

### 1.3 Cài đặt Maven

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

### 3. Tham khảo:
- https://www.ercoppa.org/posts/how-to-compile-apache-hadoop-on-ubuntu-linux.html
- https://blog.csdn.net/sinat_28603977/article/details/105536003
