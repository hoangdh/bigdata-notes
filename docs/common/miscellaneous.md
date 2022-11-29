### Bật log debug Kerberos Client với Java

```
java -Dsun.security.krb5.debug=true -Dsun.security.jgss.debug=true -Dsun.security.spnego.debug=true -jar target/test-hbase-1.0-SNAPSHOT-jar-with-dependencies.jar test_table cf
```

Hoặc sử dụng biến:

```
export _JAVA_OPTIONS="-Dsun.security.krb5.debug=true -Dsun.security.jgss.debug=true -Dsun.security.spnego.debug=true -Djava.io.tmpdir=/data/tmp"
```

### Submit MR chạy local

```
hadoop -Dmapreduce.framework.name=local ...
```

Tham khảo: https://stackoverflow.com/a/55988533
