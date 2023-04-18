### Thiết lập rule để 'dịch' từ principal Kerberos sang user local

- Ví dụ, ta thiết lập rule sau. Rule này sẽ kiểm tra principal nào trùng với `bigdata-hoangdh-.*@SECURE.LOCAL`

```
RULE:[2:$1;$2@$0]([a-z0-9_]*;bigdata-hoangdh-.*@SECURE.LOCAL)s/;bigdata-hoangdh-.*@SECURE.LOCAL/
```

- Kiểm tra bằng lệnh `kerbname` của `hadoop`:

```
hadoop kerbname hbase/bigdata-hoangdh-100-11@SECURE.LOCAL
Name: hbase/bigdata-hoangdh-100-11@SECURE.LOCAL to hbase
```
