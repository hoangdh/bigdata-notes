# Tạo role và gán cho user

## Mục tiêu

* Phân quyền chặt chẽ cho user
* Chỉ cấp đúng quyền hạn mà user cần thao tác

## Các bước thực thiện

* Tạo role trên DB cần theo tác
* Tạo user tập trung trên DB `admin`
* Gán role cho user

### Ví dụ thực tế

Cấp quyền CRUD cho một user trên DB `db01`; collection `coll01`

* **Bước 1:** Tạo role trên DB `db01`

```javascript
use db01

db.createRole({
  role: "user1Access",
  privileges: [
    {
      resource: { db: "db01", collection: "coll01" },
      actions: [ "find", "insert", "update", "remove", "createCollection" ]
    }
  ],
  roles: []
})
```

* **Bước 2:** Tạo user trên DB `admin` và gán role

```javascript
use admin

db.createUser({
  user: "user1",
  pwd: "pass1word@",
  roles: [ { role: "user1Access", db: "crm_bizfly_vn_log" }] 
})
```

### Sửa role

Trong thực tế; Dev sẽ xin thêm quyền đọc vào nhiều collection, nhiều DB. Trong TH, ta bổ sung quyền bằng cách sửa role có sẵn như sau:

Truy cập vào DB và xem các role hiện có:

```javascript
use db01
db.getRoles()
```

Xem role hiện tại có những quyền gì:

```javascript
db.getRole('user1Access', {showPrivileges:true}).privileges
```

Bổ sung quyền cho role `user1Access` thêm quyền xem và tạo index trên `coll1` và thao tác trên `coll2`

```javascript
use db01
db.updateRole(
    "user1Access",
    {
      privileges:
          [
            {
             resource: { db: "db01", collection: "coll1" },
             actions: [ "find", "insert", "update", "remove", "createCollection", "createIndex", "listIndexes" ]
            },
            {
             resource: { db: "db01", collection: "coll2" },
             actions: [ "find", "insert", "update", "remove", "createCollection", "createIndex", "listIndexes" ]
            }
          ],
      roles:
          []
    }
)
```

