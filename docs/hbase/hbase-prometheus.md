## Giám sát thông số HBase qua Prometheus và Grafana

### 1. Cấu hình HBase

- **Bước 1**: Thêm agent JMX Prometheus 

Truy cập vào [đây](https://mvnrepository.com/artifact/io.prometheus.jmx/jmx_prometheus_javaagent/0.15.0) để tải agent và đặt vào thư mục `lib` hiện tại của HBase.

Ví dụ:

```
wget -P $HBASE_HOME/lib https://repo1.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/0.15.0/jmx_prometheus_javaagent-0.15.0.jar
```

- **Bước 2**: Tạo file cấu hình `hbase_jmx_config.yaml`

Tại đây, ta xác định các rule và pattern cần lấy. Tham khảo thêm tại [đây](https://github.com/prometheus/jmx_exporter).

> vi $HBASE_HOME/conf/hbase_jmx_config.yaml

```
---
lowercaseOutputName: true
lowercaseOutputLabelNames: true
rules:
  - pattern: Hadoop<service=HBase, name=RegionServer, sub=Regions><>Namespace_([^\W_]+)_table_([^\W_]+)_region_([^\W_]+)_metric_(\w+)
    name: HBase_metric_$4
    labels:
      namespace: "$1"
      table: "$2"
      region: "$3"
  - pattern: Hadoop<service=(\w+), name=(\w+), sub=(\w+)><>([\w._]+)
    name: hadoop_$1_$4
    labels:
      "name": "$2"
      "sub": "$3"
  - pattern: .+
```

- **Bước 3**: Khai báo sử dụng Java Agent vào HBase

Ta thực hiện khai báo các biến sau vào trong `hbase-env.sh` để kích hoạt agent.

> vi $HBASE_HOME/conf/hbase-env.sh

```
export HBASE_JMX_BASE="-Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false"

export HBASE_MASTER_OPTS="$HBASE_MASTER_OPTS $HBASE_JMX_BASE -Dcom.sun.management.jmxremote.port=20101 -javaagent:$HBASE_HOME/lib/jmx_prometheus_javaagent-0.15.0.jar=27000:$HBASE_HOME/conf/hbase_jmx_config.yaml"

export HBASE_REGIONSERVER_OPTS="$HBASE_REGIONSERVER_OPTS $HBASE_JMX_BASE -Dcom.sun.management.jmxremote.port=20102 -javaagent:$HBASE_HOME/lib/jmx_prometheus_javaagent-0.15.0.jar=27001:$HBASE_HOME/conf/hbase_jmx_config.yaml"
```

- **Bước 4**: Khởi động lại cụm HBase

- **Bước 5**: Kiểm tra

Truy cập vào địa chỉ tương ứng với các role như sau:
- Master: http://master:27000/metrics
- RegionServer: http://region:27001/metrics

### 3. Cấu hình Prometheus

### 4. Tham khảo:
- https://javamana.com/2021/03/20210329135504957C.html
- https://godatadriven.com/blog/monitoring-hbase-with-prometheus/


### Bonus

Lấy tổng request đọc/ghi của bảng

```
lowercaseOutputName: true
lowercaseOutputLabelNames: true
rules:
  - pattern: Hadoop<service=HBase, name=RegionServer, sub=Regions><>Namespace_([^\W_]+)_table_([^\W_]+)_region_([^\W_]+)_metric_(\w+)
    name: HBase_metric_$4
    labels:
      namespace: "$1"
      table: "$2"
      region: "$3"
      ahihi: "$4"
  - pattern: Hadoop<service=HBase, name=RegionServer, sub=TableLatencies><>Namespace_(\w+)_table_(\w+)_metric_(\w+)_count
    name: hbase_tablerequestcount
    labels:
      "namespace": "$1"
      "table": "$2"
      "metric": "$3"
  - pattern: .+
```
