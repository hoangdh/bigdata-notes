## Hướng dẫn biên dịch JSVC

### 1. Cài đặt JAVA

Tham khảo hướng dẫn [sau](/docs/common/setup-java.md).

### 2. Tải và biên dịch JSVC

- Cài đặt các gói hỗ trợ build

```
apt update
apt-get install -y build-essential
```

- Tải gói mã nguồn của JSVC từ [trang chủ](http://commons.apache.org/proper/commons-daemon/jsvc.html). 

Trong hướng dẫn này, chúng tôi sử dụng phiên bản 1.0.15.

```
wget https://archive.apache.org/dist/commons/daemon/source/commons-daemon-1.0.15-native-src.tar.gz
```

- Giải nén tập tin

```
tar -xvzf commons-daemon-1.0.15-native-src.tar.gz
```

- Chuyển tới thư mục chưa mã nguồn cho Unix và tiến hành biên dịch

```
cd commons-daemon-1.0.15-native-src/unix
bash support/buildconf.sh
./configure && make
```

- Kiểm tra

```
commons-daemon-1.0.15-native-src/unix# ls
autom4te.cache  config.log   config.status  configure.in  jsvc      Makedefs.in  Makefile.in  native
CHANGES.txt     config.nice  configure      INSTALL.txt   Makedefs  Makefile     man          support
```

> ./jsvc -help
