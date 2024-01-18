## Chuyển đổi mô hình Replica set về Standalone

Ngữ cảnh: Tách 01 node trong cụm ra thành cluster replicaset khác phục vụ cho môi trường Dev/Test/Staging

Tham khảo bài viết sau: https://www.percona.com/blog/mongodb-converting-replica-set-to-standalone/

- Vào node Primary; xóa host cần tách. Trong ví dụ này, ta càn tách `node4`.

```
 rs.remove("node4:27017")
```

- Truy cập vào `node4`
  - Stop Mongoo: `systemctl stop mongod`
  - Xóa thông tin replicaset trong file cấu hình: /etc/mongod.conf
  ```
  ...
  #replication:
    #replSetName: "rs0"
  ```
 - Start Mongo: `systemctl start mongod`
 - Vào shell, tạo 01 user admin tạm thời
 ```
 db.getSiblingDB("admin").createUser({user: "dba_system", "pwd": "sekret","roles" : [{ "db" : "admin", "role" : "__system" }]});
 ```
 - Đăng nhập với user vừa tạo:
 ```
 db.auth('dba_system, 'sekret')
 ```
 - Xóa thông tin replicaset
 ```
 use local;
 db.system.replset.remove({})
 ```
- Đăng xuất và xóa user vừa tạo
```
db.dropUser("dba_system")
```
- Khởi động lại Mongo:  `systemctl restart mongod`

## Bonus: 

Nếu server nằm trong sharding; ta xóa thêm thông tin về sharding:

```
{"t":{"$date":"2024-01-10T11:16:53.805+07:00"},"s":"W",  "c":"SHARDING", "id":22075,   "ctx":"initandlisten","msg":"Not started with --shardsvr, but a shardIdentity document was found on disk","attr":{"namespace":"admin.system.version","shardIdentityDocument":{"_id":"shardIdentity","shardName":"rs0","clusterId":{"$oid":"658be1afd860cf716b63b198"},"configsvrConnectionString":"rsConfigServer/10.10.10.1:27019,10.10.10.2:27019,10.10.10.3:27019"}}}
```

- Cách khắc phục:
  
```
use admin
 db.system.version.deleteOne( { _id: 'shardIdentity' } )
```

- Khởi động lại Mongo:  `systemctl restart mongod`
