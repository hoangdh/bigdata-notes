## Sử dụng Dynamic Configuration trong ZooKeeper

- **Quan trọng:** `reconfigEnabled=true`


### File cấu hình mẫu:

Ban đầu, ta có 3 server 10.10.10.111-113. File cấu hình `zoo.cfg` tương ứng trên mỗi server như sau:

```
initLimit=5
syncLimit=2
skipACL=yes
4lw.commands.whitelist=stat, ruok, conf, isro
clientPort=2181
tickTime=2000
dataDir=/data/zk
admin.enableServer=false
reconfigEnabled=true

server.1=10.10.10.111:1888:1889
server.2=10.10.10.112:2888:2889
server.3=10.10.10.113:3888:3889
```
- Bật lần lượt Zookeeper trên từng node.

### Thêm các server mới vào cụm Zookeeper

Ta thực hiện thêm 4 node mới (10.10.10.114-117) vào cụm hiện tại sử dụng Dynamic Configuation của Zookeeper như sau:

- **Bước 1:** Chuẩn bị môi trường; ta cài đặt Zookeeper và khai báo cấu hình đầy đủ như sau:

```
initLimit=5
syncLimit=2
skipACL=yes
4lw.commands.whitelist=stat, ruok, conf, isro
clientPort=2181
tickTime=2000
dataDir=/data/zk
admin.enableServer=false
reconfigEnabled=true

server.1=10.10.10.111:1888:1889
server.2=10.10.10.112:2888:2889
server.3=10.10.10.113:3888:3889
server.4=10.10.10.114:4888:4889
server.5=10.10.10.115:5888:5889
server.6=10.10.10.116:6888:6889
server.7=10.10.10.117:7888:7889
```

- **Bước 2:** Thêm các node mới vào cụm

Ta tạo 1 file khai báo thông tin các node mới vào 1 file `/tmp/add-nodes.cfg`:

```
server.1=10.10.10.111:1888:1889
server.2=10.10.10.112:2888:2889
server.3=10.10.10.113:3888:3889
server.4=10.10.10.114:4888:4889
server.5=10.10.10.115:5888:5889
server.6=10.10.10.116:6888:6889
server.7=10.10.10.117:7888:7889
```

Kết nối tới Zookeeper và thực hiện lệnh:

> zkCli.sh -server 10.10.10.111:2181

Thực hiện lệnh reconfig và `config -s` xem lại cấu hình hay đổi

```
reconfig -file /tmp/add-nodes.cfg
config -s
```

- **Bước 3:** Bật lần lượt Zookeeper trên các máy chủ mới thêm

### Tham khảo:
- https://habr.com/en/articles/586490/
- https://zookeeper.apache.org/doc/r3.5.3-beta/zookeeperReconfig.html

