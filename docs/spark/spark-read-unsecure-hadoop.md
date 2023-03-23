## Dùng Spark đọc cụm Un-secure

- Lỗi xuất hiện:

```
Caused by: org.apache.hadoop.security.AccessControlException: Server asks us to fall back to SIMPLE auth, but this client is configured to only allow secure connections.
	at org.apache.hadoop.ipc.Client$Connection.setFallBackToSimpleAuth(Client.java:926)
	at org.apache.hadoop.ipc.Client$Connection.setupIOstreams(Client.java:868)
	at org.apache.hadoop.ipc.Client$Connection.access$3800(Client.java:414)
	at org.apache.hadoop.ipc.Client.getConnection(Client.java:1677)
	at org.apache.hadoop.ipc.Client.call(Client.java:1502)
	... 76 more
org.apache.hadoop.security.AccessControlException: Call From hadoop1011.local/10.10.10.11 to 10.10.10.10:8020 failed: Server asks us to fall back to SIMPLE auth, but this client is configured to only allow secure connections.
```

- Khắc phục:

> spark-shell --master yarn --conf spark.hadoop.ipc.client.fallback-to-simple-auth-allowed=true

Hoặc thêm vào file cấu hình `spark-default.xml`

```
...
spark.hadoop.ipc.client.fallback-to-simple-auth-allowed=true
```

- Tham khảo
  - https://community.cloudera.com/t5/Support-Questions/Running-distcp-between-two-cluster-One-Kerberized-and-the/m-p/94372/highlight/true#M57794
