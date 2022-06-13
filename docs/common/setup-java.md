## Hướng dẫn cài đặt Java - Oracle JDK

### 1. Tải gói jdk

Tải trực tiếp từ [trang chủ](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html).

Trong ví dụ này, chúng ta sử dụng Java phiên bản `jdk-8u202-linux`. Nếu sử dụng bản khác; vui lòng thay thế số phiên bản vào đúng các câu lệnh bên dưới.

### 2. Cài đặt jdk

- Giải nén và thiết lập biến môi trường

Giải nén vào thư mục

```
mkdir -p /usr/jdk64
tar -C /usr/jdk64 -xvzf jdk-8u202-linux-x64.tar.gz
```

Thiết lập biến môi trường

```
cat > /etc/profile.d/java.sh << EOF
export JAVA_HOME=/usr/jdk64/jdk1.8.0_202
export PATH=\${JAVA_HOME}/bin:\${PATH}
EOF

chmod +x /etc/profile.d/java.sh
bash /etc/profile.d/java.sh
```

### 3. Kiểm tra

```
echo $JAVA_HOME
java -version
```
