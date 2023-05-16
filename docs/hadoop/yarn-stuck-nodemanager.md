## Nodemanager khởi động lâu

Trong log của Nodemanager sinh ra những dòng tương tự:

```
Remove container container_exxx_xxxxxxxxxxxxxxxx_xxxx_01_000010 with incomplete records
```

### Nguyên nhân: 

Do trạng thái của các container vẫn được lưu lại trong DB của NodeManager; khi khởi động các container này không còn nên NM sẽ thực hiện xóa.

### Giải pháp:

- Dừng NodeManager
- Xóa DB của NM lưu trữ trạng thái container ở cấu hình `yarn.nodemanager.recovery.dir`. Mặc định: /tmp/hadoop-yarn/yarn-nm-recovery

```
rm -rf /tmp/hadoop-yarn/yarn-nm-recovery/*
```

- Bật lại NodeManager

## Tham khảo:
- https://community.cloudera.com/t5/Support-Questions/CDH5-2-yarn-Error-starting-yarn-nodemanagers/td-p/21700
